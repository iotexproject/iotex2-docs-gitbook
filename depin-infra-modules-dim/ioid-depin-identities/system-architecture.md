# System Architecture

## System Architecture

A high-level system architecture of ioID is illustrated in the figure below.

<figure><img src="../../.gitbook/assets/Screenshot 2025-05-20 at 12.07.40 PM.png" alt=""><figcaption><p>A High-Level Architecture of ioID</p></figcaption></figure>

### Components of ioID

ioID consists of the following core components:

**Decentralized Identifier**\
A globally unique, W3C-compliant [decentralized identifier](https://www.w3.org/TR/did-1.0/) used to represent the identity of a machine or agent in a portable and verifiable way.

**Device NFT**\
A digital representation of a physical or virtual device on-chain, implemented as an [ERC-721](https://eips.ethereum.org/EIPS/eip-721) token. This NFT is typically minted by a DePIN (Decentralized Physical Infrastructure Network) project and serves as the anchor for identity and ownership.

**Machine-Bound Account (MBA)**\
A smart contract wallet inspired by [ERC-6551](https://eips.ethereum.org/EIPS/eip-6551), created for each device NFT. The MBA enables the machine to interact directly with decentralized applications (dApps), manage assets, and execute transactions autonomously.
