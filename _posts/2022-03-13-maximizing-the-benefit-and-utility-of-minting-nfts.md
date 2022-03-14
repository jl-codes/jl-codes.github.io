---
layout: post
title: 'Maximizing the Benefit and Utility of Minting NFTs'
permalink: /blog/maximizing-the-benefit-and-utility-of-minting-nfts
categories: [blog]
---

NFTs have been a hot trend in tech but face criticism regarding their lack of obvious utility. I started this off as a vent post for my frustration regarding the current discourse around NFTs. Still, I decided about midway through that energy might be better spent trying to help answer the pertinent questions: What are the benefits of minting NFTs, and how can we maximize the utility of NFTs?

This post will start with my preamble of the problem existing with current common viewpoints, describe the technology behind NFTs in a way that makes sense to a human, and discuss the potential applications of this technology that can, with any luck, help further the discussion surrounding the use of NFTs. 


## The Problem with the Current Approach to NFTs

The tragedy of commons isn’t a new idea, having origins in Greek antiquity. NFTs are still a new concept, but they have seen widespread exposure. The utility of minting NFTs is not well defined; we will waste energy in extraneous applications of blockchain technologies until we understand the benefits. 

**To understand the usefulness of this emerging technology, we must first understand the nature of the technology behind NFTs.**

Sentiment regarding NFTs stands inverse to knowledge surrounding NFTs: 

![pastejacking-crypto-wallets-forkbomb-address-theft-defi](../../../../../assets/image/maximizing-nft-utility-chart-knowledge-forkbomb.png){:class="img-responsive"}

That annoying, MLM-pitching guy on Instagram posting about crypto and NFTs bragging about how it’s the future? He has no idea how blockchain technology works nor the benefits of such applications. The self-righteous netbanger virtue signaling how they don’t want to contribute to capitalism and pollution? They are correct about the unsustainability but are missing the obvious solution.

There is an issue rooted in the more significant problem of human unsustainability in many categories, including methods of energy generation. This failure should not reflect on the technical abilities of an immature technology. If you:

- drive a personal car, 

- play video games, or 

- use air conditioning, 

then your anger is misplaced. This statement doesn't intend to exemplify a _tu quoque_ fallacy but direct attention to what energy sources are familiar–coal, gas, or another nonrenewable. Our system is at fault, not this tool. 

NFTs are data defined on the blockchain. The technology is only as powerful as the application.

Beyond this lack of understanding, we don't lead with the key benefits to minting NFTs. There is an obsession with the ability of NFTs to provide ownership. This common consensus is warped–the power of NFTs to provide origination proof is the most significant benefit of the technology.

Put another way, NFTs are ultimately just tools. Holding such strong sentiment about a tool makes about as much sense as raging against the concept of hammers. Having a solid opinion regarding hammers makes more sense than dying on any hill related to NFTs since:



* the use case of hammers is defined and well established
* the technology surrounding hammers is mature

To properly use a tool, one must understand the benefits. What can the technology do efficiently? The answer to this question leads us to achieve utility for NFTs.


## The Key Benefits to Minting NFTs


#### Immutable 

Once a person mints an NFT, **the decentralized ledger stores the uploaded data forever**. This data cannot be modified, making disputes regarding the ownership history of a digital asset a thing of the past. No one may change information uploaded to the blockchain through the minting process.

This attribute of immutability is a double-edged sword; if clerical errors are introduced and not corrected before minting, they are propagated across the nodes of the system and cannot be modified. The lack of remediation options is a crucial downside to using a public blockchain: with no central authority, no resource exists to remediate mistakes or provide reassurance of ownership–it is for the same reason that [nearly 1 in 5 bitcoins are lost forever](https://www.coindesk.com/tech/2021/12/08/bitcoins-lost-coins-are-worth-the-price/). 


#### Decentralized 

The decentralized nature of blockchain technology means that users may readily access data stored in this ledger. In addition, it is nearly impossible to tamper with data within this ledger. The only “feasible” way to tamper with decentrally stored data is to simultaneously tamper with each blockchain node.

Data stored on the blockchain is much more readily available than from non-distributed ledgers, thus improving the accessibility of the data. This plays a part in preventing retrieval delays and bottlenecks that can occur with centralized databases that don't take advantage of CDNs. 

The three main pillars of information security include confidentiality, integrity, and availability. **The decentralized nature of blockchain-bound assets, including NFTs, allows for verification of the integrity and availability of data**. However, as all data tied to the blockchain is publically available, confidentially cannot be expected. Credentials and secrets, regardless of encryption, should never be added to the immutable ledger.


#### Smart Contract Bound

NFTs may utilize smart contracts as part of their design. A smart contract is the agreement-in-code within the blockchain, enabling the network to store the information indicated within an NFT transaction. This smart contract may be reused as a means of transferring NFT ownership from wallet to wallet.

**Smart contract developers may bake usage stipulations into the NFT**. This capability unlocks possibilities for artists: **royalties** may be hardcoded into the smart contract. We have already seen disruptions within the art world resulting from the fact that artists can now profit from their art in the short term–one doesn’t have to come from money to survive as an artist.


## Use Cases Maximizing NFT Utility

Now that we have laid out the benefits provided by NFTs let’s discuss use cases taking these benefits into account.


#### Proof of Origination / Provenance

**Tracing the custodial history of an asset** is not a new concept–provenance helps establish an item’s significance and value based on its chain of ownership. The decentralized, immutable nature of the blockchain means that interested parties may trace the provenance of an NFT through the ledger.

[As evidenced in Moxie’s demonstration](https://moxie.org/2022/01/07/web3-first-impressions.html), the NFT’s integrity is only as strong as the integrity of the utilized hosting service and the authenticity of the 3.0 implementation. If a service is inauthentic in its use of Web 3.0 architecture (looking at you OpenSea) and the host has a mind to tamper with the content, there is no guarantee that the content has not been corrupted.

Because of the expensive and rising gas costs, it’s unrealistic to expect public blockchains to store ever-growing file sizes. Thus, holding the entirety of an image on the blockchain is unrealistic for most applications.

To counter this, we may use a form of validation to ensure data integrity. For example, verifying the originality could be performed by visual cryptography. However, the origination point referenced when the creator mints the NFT is immutable even without validation. Therefore, **minting an NFT establishes a point of origination that ties a work back to the original wallet regardless of the number of total transactions**.


#### Certification

The fact that the reference is immutable and the origination of NFTs is reliable makes this digital asset perfect for providing a form of certification, such as a diploma, an accreditation, or another form of a certificate. For this use case, using an NFT to represent a certificate takes advantage of the proof of origination capabilities to provide enhanced verification of authenticity. If the issuing institution publically lists the wallet used to generate NFT certificates, interested parties can prove the certificates by scanning for the originating address. **Forging a paper of digital document is easy, but faking origination tied to a wallet address is next to impossible**. 

Depending on the specific implementation and the terms outlined in the smart contract, developers building this underlying smart contract may make this certificate to expire after a particular timestamp. This functionality solves an additional problem: checking whether accreditation is expired, revoked, or otherwise invalid.


#### Untamperable Checksums

[Checksums](https://en.wikipedia.org/wiki/Checksum#:~:text=A%20checksum%20is%20a%20small,upon%20to%20verify%20data%20authenticity.) are small, byte-sized blocks of data derived from a source of data. They are used to detect errors that transmission or storage may have introduced. Developers often use checksums to verify data integrity, but since they do not contain origination information, we cannot verify data authenticity.

**Creating checksum NFTs solves the problem of verifying data authenticity because relevant stakeholders may prove the origination of these checksums**. Because these NFT checksums are immutable and decentralized, organizations may use them to guarantee that errors and tampering have not occurred.


### Conclusion: Maximizing the Benefits of NFTs

Here’s the bottom line: NFTs and even blockchain broadly are technologically immature. Necessity existing as the mother of invention is dead: instead, we unlock capabilities and flail about seeking optimal applications. We must understand the benefits of the technology to maximize the tool's utility. These include:
- The immutability of information added to public blockchains.
- The decentralized nature of NFT storage.
- The guaranteeds offered by smart contracts.

We’re looking at NFTs in their dial-up phase. The 90s Yahoo version. The AOL messenger phase. The first phase of smart-contract bound digital assets is here. It’s crude, nebulous, and not well explored, but to explore the unknown is human—the frontier beckons. 