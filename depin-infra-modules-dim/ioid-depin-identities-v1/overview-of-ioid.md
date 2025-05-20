# Overview of ioID

The ioID Module is IoTeX's comprehensive decentralized identity solution based on the IoTeX blockchain and the ioID specification. It is an essential module for developers aiming to integrate decentralized identity verification into their DePIN applications.

The IoTeX ioID module comprises four main components:

1. **ioID Smart Contracts**
2. **DID-based secure communication protocol**
3. **Client-side tools and SDK**
4. **DID Resolver Service**

Together, these components facilitate the robust management of both on-chain and off-chain identities for smart devices, users, and nodes within a decentralized network.

## High-Level Architecture Overview

This document provides a high-level overview of the architecture of the ioID module and it's components.

<figure><img src="../../.gitbook/assets/image (38).png" alt=""><figcaption><p>ioID Module components are highlighted in green</p></figcaption></figure>

### **ioID Contracts**

The ioID smart contracts provide a secure and decentralized framework for managing the identities of DePIN projects, users, devices, and nodes based on the IoTeX Blockchain.

[-> Learn more about ioID Contracts](integration-guide/ioid-smart-contracts.md)

### **Secure DID-based communication**

Unlike traditional centralized IoT clouds that rely on secret certificates, ioConnect enables devices to use Decentralized Identifiers (DIDs) for establishing secure communication channels.&#x20;

[-> Learn more about ioID Contracts](broken-reference)

### **Client tools and SDK**

Several client tools are provided to interact with ioID both for DEPIN Builders, device manufacturers, and device owners. ioConnect SDK includes a dedicated API to implement the secure DID communication between devices and servers. The IoTeX CLI ioctl facilitates interactions for DEPIN Builders, while wallet.iotex.io implements the device owner experience.

[-> Learn more about ioConnect SDK for ioID](../ioconnect-hardware-sdk/)

\-> Learn more about ioctl for ioID

\-> Learn more about wallet.iotex.io for device owners&#x20;

### DID Resolver Service

The DID Resolver is an essential service for enabling DID-based communications within a DePIN project. It serves the critical function of resolving Decentralized Identifiers (DIDs) to their corresponding DID Documents, which contain the necessary metadata and public keys required for secure interactions.

[-> Learn more about the DID Resolver](broken-reference)
