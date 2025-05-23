---
description: A "Proxy Contract" Approach
---

# Integrate ioID in your cloud

In scenarios where a DePIN project cannot integrate ioID directly into their device’s firmware, this approach enables the project’s cloud to manage ioID registration and message signing on behalf of the devices. In this setup, a “Proxy ioID Contract” serves as an intermediary between the project’s cloud and ioID contracts. The cloud acts as a “trusted entity” within the proxy contract, authorizing on-chain ioID registrations and binding them to the actual device owner.

{% hint style="info" %}
**IoID Web Tools**

[IoID Web Tools ](https://hub.iotex.io/dev/ioid)are designed to streamline experimentation and development for integrating **IoID** into DePIN projects. These tools, currently under development by the IoTeX Foundation, aim to provide a seamless and interactive user experience for creating and managing IoID-based projects.&#x20;

While this page explains how to register your project in IoID using the `ioctl` command line, you can achieve the same using the [IoID Web Tools](https://hub.iotex.io/dev/ioid) where you can list, and manage all your projects interactively, including deploying a default Device NFT contract.
{% endhint %}

{% hint style="warning" %}
* Ensure you followed the initial steps to [register your Project on the IoTeX chain](./#id-1.-register-your-depin-project-on-iotex).
* Ensure you have development tools like git and nodejs installed
{% endhint %}

## Deploy the ioID Proxy Contract

### Deploy using Hardhat

If you haven’t already, clone the ioID-contracts repository:

```bash
git clone https://github.com/iotexproject/ioID-contracts
cd ioID-contracts && npm install
```

Export the private key for deploying the ioID Proxy contract.&#x20;

{% hint style="warning" %}
This account will control your ioID proxy contract, particularly for setting the “Verifier” address. The “Verifier” is typically your service account responsible for managing ioID registrations on behalf of the devices on the cloud side

**Ensure you use a dedicated wallet account as the owner of your ioID Proxy contract. Whenever possible, consider using a hardware wallet for enhanced security on mainnet.**
{% endhint %}

```bash
export PRIVATE_KEY=0x
```

Export the ioID Store and Project Registry contract addresses (check the [repository on GitHub](https://github.com/iotexproject/ioID-contracts#deployment) for updated addresses):

```bash
export IOID_STORE=0x60cac5CE11cb2F98bF179BE5fd3D801C3D5DBfF2
export PROJECT_REGISTRY=0x060581AA1A4e0cC92FBd74d251913238De2F13cd 
```

Deploy your ioID Proxy contract with:

```bash
npx hardhat run scripts/deploy-proxy.ts --network testnet
```

### Deploy using the ioID Web Tools

Developers can also deploy an ioID proxy contract and manage it interactively on the IoTeX Hub:

* Browse the [ioID Web Tools on the IoTeX Hub](../../../welcome-to-iotex-2.0/tokenomics/)
* Select the Advanced tab:

<figure><img src="../../../.gitbook/assets/image (1) (1).png" alt=""><figcaption></figcaption></figure>

* Click the `Deploy VerifyingProxy Contract` button to deploy a new Proxy contract, or
* Click the `Import VerifyingProxy Address` button to import an already deployed Proxy
* Provice the required information then click `Confirm`

<figure><img src="../../../.gitbook/assets/image (2) (1).png" alt=""><figcaption></figcaption></figure>

## Interact with the Proxy contract

Here is an example repository that shows how to generate ioIDs for your devices in the cloud and how to register them on-chain through the Proxy contract:

{% embed url="https://github.com/iotexproject/ioid-register-flow-demo/tree/main/examples/with-contract-flow" %}
