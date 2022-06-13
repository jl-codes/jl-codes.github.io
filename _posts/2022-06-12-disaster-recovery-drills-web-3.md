---
layout: post
title: 'Disaster Recovery Drills in Web 3.0'
permalink: /blog/disaster-recovery-drills-in-web-3
categories: [blog]
---

I spent several hours tracking down a small notebook earlier this week. The reason? I wrote down my seed phrases in this tiny notebook, which I carelessly tracked. Why am I telling you this? This management strategy is a horrible practice. Had I not performed a disaster recovery drill for my digital assets, I might’ve continued storing my seed phrase insecurely. Disaster recovery drills are foundational for security in web 3.0 and enable continued access to blockchain assets. 


## Web 3 Wallet Security Basics

Securing your seed phrase is foundational to security because the seed phrase is the [cryptographic basis for accessing the wallet](https://getcoinplate.com/blog/is-a-seed-phrase-the-same-as-a-private-key-the-ultimate-guide-to-private-keys-and-recovery-seed-phrases/). The seed phrase represents a long string of random numbers — and your wallet uses it to generate the private keys that let you send and spend your crypto. Individuals and institutions should securely secure their wallet seed phrases in multiple locations and, ideally, partitioned. Without the seed phrase, wallet owners will have no recourse for recovering their wallets.

There are well-established standards for [storing seed phrases](https://www.coinbase.com/learn/crypto-basics/what-is-a-seed-phrase). The best practices boil down to:



* Store your seed phase in multiple locations.
* Ensure access to your seed phrase is sufficiently restricted.
* If feasible, [only store parts of the seed phrase at any one site](https://bitcoin.stackexchange.com/questions/60540/practical-way-to-split-a-bip39-seed-into-a-2-out-of-3-factor-auth) to avoid a single point of compromise.
* Factor in the possibility of a natural disaster–it may be worth creating etched plates to hold parts of a seed phrase rather than hoping pieces of paper survive a fire, flood, or other acts of g*d.
* Memorize your seed phrase–you could get hit by a bus, but if you can find a hardware wallet in heaven, you’ll have access to your digital assets. (This is obviously a joke – crypto degens don’t go to heaven)

Wallet owners will inevitably make tradeoffs between security and convenience, but this is par the course for making security decisions. Retaining your seed phrase is fundamental to maintaining access to your digital assets; therefore, protecting this seed phrase should be the top priority of a wallet’s owner. 


#### Back-Up Private Keys

Creating a backup of private keys is fundamental for creating a successful disaster recovery plan for digital assets. Wallets generate private keys using the seed phrase as input. Seed phrases and private keys aren’t the same, although both allow someone to spend coins in a wallet. 

There are two main approaches for maintaining secure custody of private keys, depending on the responsible entity type:



* For **individual users**, store your private keys securely. The best practices for storing this data are comparable to the above guidelines for storing seed phrases. Utilizing a hardware wallet for long-term storage is the most secure option to mitigate the risk to your private keys.
* For **institutions**, store private keys within a secure secret manager, such as [Vault](https://www.vaultproject.io/) or AWS Secrets Manager. Wallet custodians should treat these private keys like credentials and other secrets. To state the obvious: private keys, regardless of form, should never be stored in plain text internally nor on the blockchain.


### Web 3 Disaster Recovery For Developers:

Disasters affect developers too. Thus, it is in the best interest of developers who touch decentralized applications to have a well-thought-out, well-tested disaster recovery plan mapped out. For example, consider all the points of access developers need to take a dApp from code to deployment. Not only should developers take care to protect their wallets, but they should protect [access to their code and deployment infrastructure](https://www.preethikasireddy.com/post/the-architecture-of-a-web-3-0-application) and should develop a contingency plan to handle hacks and other business logic exploits which may come to light.

An average deployment on leading blockchains [can cost thousands of dollars](https://medium.com/scrappy-squirrels/estimating-smart-contract-costs-f65acf818c26). For this reason, organizations should protect developer access to deployed applications–this entails planning around the risk of developer lockout and enforcing access controls. Access restrictions implemented according to the [principle of least privilege](https://cycode.com/blog/using-the-principle-of-least-privilege-for-maximum-security/?utm_source=rss&utm_medium=rss&utm_campaign=using-the-principle-of-least-privilege-for-maximum-security) help ensure workflow recovery is possible–take steps to ensure you build with the ability to update, patch, and recover access to deployed smart contracts.


#### Handling Tech Debt

In the same way that entropy is inevitable to any physical system, tech debt is a byproduct of the early, do-or-die stages of product development. Once the product has a bit of runway to work with, major refactors can be necessary to improve performance and address any other rickety implementations. This friction in the SDLC introduced by tech debt can lead to immediate and later insecurities if left uncorrected.

In addition, [tech debt](https://en.wikipedia.org/wiki/Technical_debt) can lead to churn that harms developer velocity. Continuously fighting with poorly implemented routines, wrangling complicated test cases, and deciphering unconventional logic takes a toll on productivity. Reimplementation comes with costs, but the learning acquired throughout the MVP development process often allows for higher quality. This refactoring can make it easier to add features and capabilities on top of existing applications.


#### Security Training and Assistance

One of the best ways to help prevent security vulnerabilities from escaping to your smart contract or live dApp is to institute security training for developers. This security awareness is especially pertinent for developers of exchanges and other DeFi applications, especially since [exploitable bugs can appear in the smart contracts](https://arstechnica.com/information-technology/2021/12/hackers-drain-31-million-from-cryptocurrency-service-monox-finance/), and the [dApp frontend provides an additional attack surface](https://forkbomb.io/blog/pastejacking-smart-contracts) if not carefully guarded. 

To improve the security of applications interacting with payment card data, PCI DSS 6.5 requirements state that the developers must receive regular security training at least once a year. Though [regulation on digital assets is still underway](https://forkbomb.io/blog/crypto-executive-order-explained), organizations will see the benefits of providing basic security training to developers in terms of time saved resolving bugs and other vulnerabilities. 

One may even recommend solutions that entail [automated remediation](https://cycode.com/blog/cycode-workflows/) as a solution, as a means of maximizing the time saved–such tools can even help train developers to support improvements to an organization’s security posture.

**Bottom line: Ensure sufficient security training for developers and foster a culture of pride in the quality, and it’ll pay off both figuratively and literally.**


## Recovery Drills for Cryptocurrencies and Digital Assets

There’s no stand-in for actual disaster recovery practice. Developers should undergo manual wallet recovery training to practice saving digital assets since there are no second chances when it comes to cryptographic assets. Though [new digital asset regulation](forkbomb.io) has been hinted at to help mitigate the risks imposed by cryptocurrencies, [NFTs](https://forkbomb.io/blog/maximizing-the-benefit-and-utility-of-minting-nfts), and other assets tied to cryptographic wallets, early adopters are still mainly on their own regarding the security of their assets.

Even after regulation hits, every wallet owner should create and test a plan for their digital asset recovery–it’s the best way to ensure the success of a contingency plan–just make sure you don’t lock yourself out while testing. 

Cyber-attacks have hit [crypto exchanges](https://www.hedgewithcrypto.com/cryptocurrency-exchange-hacks/), [dApps](https://mirror.xyz/infinitesn4ke.eth/xSu1ssilKkKQocyHABHrs5CWATwJN89EwqQ1fGkeT94), and other web 3 projects. The Architectural & Security Initiatives for Decentralized Software Security Framework (ACID-SF) is an [experimental cybersecurity framework designed to help improve web 3.0 project security](https://mirror.xyz/infinitesn4ke.eth/KpvzZHbpXuQEsmz9C30dmdhI7Xv6QFZ1nxBgt12GXu8). The ultimate goal of this framework is to enable project stakeholders to make more informed cybersecurity decisions.