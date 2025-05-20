# ioID Smart contracts quick reference

The ioID suite of smart contracts provides a robust framework for decentralized identity management on the IoTeX blockchain. These contracts collectively provide a robust framework for identity management and interaction within the IoTeX ecosystem.&#x20;

{% hint style="warning" %}
↗  For more details, source code and deployments, you can visit the [GitHub repository](https://github.com/machinefi/ioID-contracts).
{% endhint %}

Here’s a brief overview of each contract and its main functions.

* [DePIN Projects Registry](ioid-smart-contracts.md#depin-projects-registry)
* [ioID NFT](ioid-smart-contracts.md#ioid-nft)
* [ioID Store](ioid-smart-contracts.md#ioid-store)
* [ioID Registry](ioid-smart-contracts.md#ioid-registry)

## DePIN Projects Registry

**ProjectRegistry.sol**

The DePIN Project Registry is an NFT-based registry that manages all DePIN projects. It ensures that each project is uniquely identified and authenticated within the network.

{% hint style="info" %}
**Relevant methods**

**`register`**\
Registers a new project and mints a new project ID to the project owner in the form of an NFT.
{% endhint %}

## ioID NFT

**ioID.sol**

The ioID NFT contract is an essential part of the ioID framework for decentralized identity management on the IoTeX blockchain. It's directly managed by the Project Registry and is in charge for creating and assigning unique ioID tokens for devices. This involves **linking devices to project IDs and owners**, and generating associated wallet addresses according to the ERC6551 standard.

{% hint style="info" %}
**Relevant Methods**

**`projectIDs`**\
Retrieves a paginated list of device addresses associated with a specific project.

**`did`**\
Retrieves the Decentralized Identifier (DID) associated with a specific device.
{% endhint %}

## ioID Store

**ioIDStore.sol**

The ioID Store is responsible for managing the application and activation of ioID across all projects. It handles the lifecycle of identity management applications, ensuring that identities are correctly set up and maintained.

{% hint style="info" %}
**Relevant Methods**

**`applyIoIDs`**\
Allows project owners to apply for ioIDs by paying the required amount.

**`setDeviceContract`**\
Associates a device contract with a project.
{% endhint %}

## ioID Registry

**ioIDRegistry.sol**

The ioID Registry contract is used for registering devices on-chain and activating their ioID. It also serves as a DID resolver, providing a reliable means for verifying device identities across different projects.

{% hint style="info" %}
**Relevant Methods**

**register**\
Registers a new device with a given DID and device contract.

**documentID**\
Returns the DID for a device.

**documentURI**\
Returns the URI of the DID record for a device.
{% endhint %}
