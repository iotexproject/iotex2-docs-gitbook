# Accounts & Identities

An "account" is a fundamental concept in the IoTeX blockchain, represented by a pair of cryptographic keys: a **private key** and a **public key,** utilized to sign and verify [blockchain actions](blockchain-actions.md).&#x20;

IoTeX accounts could represent individual users, machines, or entities, facilitating interactions with the blockchain and dApps. Like Ethereum, IoTeX's architecture includes two types of accounts:

* **Externally Owned Accounts (EOAs):** Controlled by private keys, these accounts represent individual users or entities.
* **Contract Accounts:** These are associated with smart contracts deployed on the blockchain and operate based on their programmed logic. Each contract account maintains a balance, nonce, and storage, contributing to IoTeX's global state.

[-> Learn more about IoTeX Account Cryptography](broken-reference)

## Account Abstraction

IoTeX supports Account Abstraction as defined by  [**ERC-4337**](https://eips.ethereum.org/EIPS/eip-4337) v0.6.0.

{% hint style="info" %}
_"Account Abstraction (AA)_ _allows users to use smart contract wallets containing arbitrary verification logic instead of EOAs as their primary account._"&#x20;
{% endhint %}

By enabling people to use Smart Contracts as their primary accounts, ERC-4337 introduces many user experience benefits.

ERC-4337 runs on top of the blockchain and does not require any changes to the blockchain itself. Currently, the IoTeX Account Abstraction code is based on ERC-4337 0.6.0 release version.

## ioID Identities

The blockchain also supports ioID identities, providing a secure and verified identity system that integrates seamlessly with various DePIN projects and applications.

[↗ Learn more about ioID](../../ioid-depin-identities-v1/)
