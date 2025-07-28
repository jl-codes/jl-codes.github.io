---
layout: post
title: "Bullet Train Sushi Robot: Flutter + Arduino Control for Zenpo Sushi"
permalink: /blog/bullet-train-sushi-robot
categories: [blog]
description: "Designing and building a bullet train–style sushi delivery robot with Flutter (client) and Arduino (controller). Networked control, safety interlocks, telemetry, and production-ready next steps."
---

> **TL;DR** — I built a **bullet train sushi robot** control stack for Zenpo Sushi.  
The mobile UI is a **Flutter** app that drives a Wi‑Fi robot over simple HTTP commands; the firmware is **Arduino** (WiFiNINA) orchestrating dual motors, a Hall sensor for position, a hardware **Return/Home** button, and idle‑safety logic. This post walks through the architecture, the code, and the production hardening plan.

---

## Why a “bullet train” instead of a conveyor?

Two things: **precision** and **experience**. Traditional conveyors are constant‑motion and high‑maintenance. A point‑to‑point “train” lets us schedule deliveries to specific seats, **stop exactly where needed**, and create a guest experience that feels futuristic without a full AGV stack.

---

## System architecture

**Client (Flutter, Material 3)**  
- Select “Robot 1” or “Robot 2” (two cars on the line).  
- Send commands over HTTP: `START`, `STOP`, `REVERSE`, `RETURN`, `GOTO{n}`, `SENSOR`.  
- Poll sensor data every 2 seconds for live position.

**Controller (Arduino + WiFiNINA)**  
- Hosts a minimal HTTP server on port **80**.  
- Drives two motors (enable/direction/speed) via motor drivers.  
- **Hall effect sensor** increments/decrements `position` (0–15 stops).  
- **Return button** ISR to home the train immediately.  
- **Idle safety**: if no movement for 30s away from home → auto return.

**Network**  
- Static IPs per robot: `192.168.1.201` and `192.168.1.207`.  
- Flat HTTP interface (human‑readable), easy to script and test.

---

## The Flutter app (operator UI)

The app uses `http` and a periodic `Timer` to poll the robot sensor endpoint, show current status, and drive motion by tap. Controls are intentionally **simple and fast**—kitchen staff shouldn’t fight UI when service is slammed.

```dart
// Pick robot by toggling a switch
String get robotIpAddress => isRobot1Selected
  ? '192.168.1.201'
  : '192.168.1.207';

// Send human-readable commands over HTTP
Future<void> sendCommand(String command) async {
  try {
    final res = await http.get(Uri.parse('http://$robotIpAddress/$command'));
    if (res.statusCode == 200) {
      setState(() {
        sensorData = res.body;
        if (command == 'SENSOR') {
          // Expect: "Current Position: X"
          final parts = sensorData.trim().split(' ');
          position = int.tryParse(parts.isNotEmpty ? parts.last : '0') ?? 0;
        }
      });
    } else {
      setState(() => sensorData = 'Error: ${res.statusCode}');
    }
  } catch (e) {
    setState(() => sensorData = 'Error: $e');
  }
}
```

**Nice touches:**
- Periodic telemetry: Timer.periodic(Duration(seconds: 2), ...) keeps the UI synced without spamming the controller.
- Declarative commands: goToPosition(12) maps to GET /GOTO12—clear and inspectable.
- Dual‑robot switch: one UI pane, two IPs, zero confusion during service.


## The Arduino controller (deterministic movement)

On the microcontroller, interrupts turn a low‑cost Hall sensor into a reliable position counter; each magnet “tick” increments or decrements depending on direction. Movement is bounded within 0..15 stops, and we stop on end‑stops or when the target position is reached.

```dart
volatile int position = 0;
volatile bool magnetDetected = false;
volatile bool returnButtonPressed = false;
int targetPosition = -1;
unsigned long lastMotorActivityTime = 0;

void hallSensorISR() { magnetDetected = true; }

void startMotors() {
  digitalWrite(enablePin1, HIGH);  // forward on motor 1
  digitalWrite(directionPin1, LOW);
  analogWrite(speedPin1, 255);

  digitalWrite(enablePin2, HIGH);  // forward on motor 2 (opposite wiring)
  digitalWrite(directionPin2, HIGH);
  analogWrite(speedPin2, 255);

  lastMotorActivityTime = millis();
}

void reverseMotors() {
  digitalWrite(enablePin1, HIGH);
  digitalWrite(directionPin1, HIGH);
  analogWrite(speedPin1, 255);

  digitalWrite(enablePin2, HIGH);
  digitalWrite(directionPin2, LOW);
  analogWrite(speedPin2, 255);

  lastMotorActivityTime = millis();
}

void stopMotors() {
  digitalWrite(enablePin1, LOW);
  analogWrite(speedPin1, 0);
  digitalWrite(enablePin2, LOW);
  analogWrite(speedPin2, 0);
}
```

A tiny HTTP handler parses the request line and routes commands:

```dart
void executeCommand(String cmd, WiFiClient &client) {
  if (cmd == "START")       { startMotors(); client.println("Motors started"); }
  else if (cmd == "STOP")   { stopMotors();  client.println("Motors stopped"); }
  else if (cmd == "REVERSE"){ reverseMotors(); client.println("Motors reversed"); }
  else if (cmd.startsWith("GOTO")) {
    const int newPos = cmd.substring(4).toInt();
    if (newPos >= 0 && newPos <= 15) {
      targetPosition = newPos;
      (targetPosition > position) ? startMotors() : reverseMotors();
      client.println(String("Moving to position: ") + targetPosition);
    } else client.println("Invalid position");
  } else if (cmd == "SENSOR") {
    client.println(String("Current Position: ") + position);
  } else if (cmd == "RETURN") {
    returnButtonPressed = true;
    client.println("Returning to original position");
  }
}

void loop() {
  if (magnetDetected) {
    magnetDetected = false;
    // Decide direction from pin state
    position += (digitalRead(directionPin1) == LOW) ? 1 : -1;
    position = max(0, min(position, 15));
    if (position == 0 || position == 15 || position == targetPosition) {
      stopMotors();
      targetPosition = -1;
    }
  }

  if (returnButtonPressed) {
    returnButtonPressed = false;
    targetPosition = -1;
    reverseMotors(); // head toward home (0)
  }

  // Serve HTTP requests (headers omitted here for brevity)
  if (WiFiClient client = server.available()) {
    String currentLine = "", command = "";
    while (client.connected()) {
      if (client.available()) {
        char c = client.read();
        if (c == '\n') {
          if (currentLine.length() == 0) {
            client.println("HTTP/1.1 200 OK");
            client.println("Content-type:text/plain");
            client.println("Access-Control-Allow-Origin: *");
            client.println();
            executeCommand(command, client);
            client.println();
            break;
          } else { currentLine = ""; }
        } else if (c != '\r') {
          currentLine += c;
          if (currentLine.startsWith("GET /")) {
            command = currentLine.substring(5, currentLine.indexOf(' ', 5));
          }
        }
      }
    }
    client.stop();
  }

  // Idle safety: after 30s off-home, return to 0
  if (millis() - lastMotorActivityTime > 30000) {
    if (position != 0 && !digitalRead(enablePin1) && !digitalRead(enablePin2)) {
      reverseMotors();
    }
  }

  delay(100);
}
```

Why this works well in a kitchen
- Deterministic: magnets = discrete stops; no encoders necessary.
- Fail‑safe: end‑stop bounds + idle watchdog + physical home button.
- Observable: the API is human‑readable—curl is a valid control panel.

## Command API (human‑readable, dead simple)

| Action         | HTTP            | Behavior                                   |
|----------------|-----------------|--------------------------------------------|
| Start          | `GET /START`    | Drive forward at full speed                |
| Stop           | `GET /STOP`     | Disable drivers and PWM                    |
| Reverse        | `GET /REVERSE`  | Drive in reverse at full speed             |
| Go to `n`      | `GET /GOTO{n}`  | Move to magnet index `n` (0–15)            |
| Sensor         | `GET /SENSOR`   | Returns `Current Position: X`              |
| Return (home)  | `GET /RETURN`   | Immediately head back toward `0` (home)    |

**Debugging tip:**
```bash
curl http://192.168.1.201/SENSOR
curl http://192.168.1.201/GOTO7
```

## Production hardening roadmap

*What I’d ship for a multi‑car line and all‑day service reliability.*

### 1) DevSecOps & updates
- Private Wi‑Fi SSID + VLAN segmentation; PSK rotation playbook.
- HMAC‑signed commands or mTLS for controller endpoints.
- Reproducible builds, versioned artifacts, and **OTA updates**.
- SLSA‑aligned binaries + SBOM for the firmware.

### 2) Observability
- Structured logs → a lightweight collector (e.g., fluent‑bit → Loki).
- Traces for command latency; counters for stops hit/missed, emergency returns.
- Grafana dashboard for FOH/BOH visibility.

### 3) Scheduling & choreography
- Small **job queue** (seat → stop) with collision avoidance when multiple cars share track segments.
- SLA timing windows (e.g., “deliver within 60s”) and escalation to a runner.

### 4) Safety & mechanicals
- Speed caps near end‑stops; PWM ramping for gentle docking.
- Current sensing → stall detection → auto stop + alert.

### 5) Developer Experience
- Quickstart, API reference, and a **demo Flutter app** for operators.
- Workshop curriculum at the makerspace for onboarding new staff/volunteers.

---

## Lessons learned

- **Interrupts > polling** for physical signals—cheap sensors become reliable state.
- **Bound everything:** speed, range, retries—kitchens are adversarial environments.
- **Flat HTTP is underrated** in local robotics—inspectable, testable, scriptable.
- **UX is throughput:** one‑tap “Go to N” buttons beat free‑form inputs during a dinner rush.

---

## Hiring, consulting, and contact

This project blends **embedded systems**, **real‑time control**, **DevSecOps**, and **developer experience**—the same mix I bring to teams building robotics, industrial IoT, or applied AI.

- **Consulting / Fractional CTO:** [Start a consultation](/consulting/)
- **Email:** turingxo@gmail.com
- **Makerspace:** [frontiertower.io](https://frontiertower.io)

If you’re hiring for **Platform/DevSecOps**, **Robotics/IoT**, or **Developer Advocacy** (I lead a 5,000+ developer community in SF), I’d love to talk.
