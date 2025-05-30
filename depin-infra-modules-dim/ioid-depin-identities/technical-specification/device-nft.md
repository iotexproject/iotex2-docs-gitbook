# Device NFT

Device NFTs are [ERC-721](https://eips.ethereum.org/EIPS/eip-721) tokens that serve as the on-chain representation of physical machines or autonomous agents. They are a core primitive in the ioID architecture, acting as the canonical link between the real world and on-chain identity, ownership, and programmability.

{% hint style="success" %}
In many cases, a **Device NFT** already exists within a DePIN project as part of its native infrastructure, to represent their deployed devices. However, if a Device NFT contract is not already available, it can be created as part of project registration  flow.
{% endhint %}

## Purpose

Each Device NFT functions as a unique digital certificate for a specific machine or AI agent, enabling:

* On-chain registration of new physical assets
* Ownership tracking and transfers
* Access control via smart contracts (e.g., MBAs)
* Integration into reward systems, staking mechanisms, or AI agent networks

## When is a Device NFT Minted?

Device NFTs are typically minted at the moment of asset creation. This may include:

* Manufacturing of a physical device (e.g., an IoT sensor, drone, EV charger)
* Bootstrapping of an AI agent or digital twin
* Provisioning a new node in a decentralized network (DePIN, DeAI)

Minting represents the genesis event that brings the real-world asset representation onto the blockchain.
