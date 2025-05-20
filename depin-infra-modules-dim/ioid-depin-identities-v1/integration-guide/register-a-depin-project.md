# 1. Register a DePIN Project

The first step for any DePIN project intending to integrate with ioID identities is to register the project on the IoTeX blockchain to obtain a Project ID.&#x20;

Here’s how to register your project:

## Using the IoTeX CLI (`ioctl`)

{% hint style="warning" %}
Please notice that this ioctl command currently only supports the IoTeX Testnet.
{% endhint %}

```sh
ioctl ioid register "My DePIN Project"

Registerd ioID project id is 41
```

Once the transaction is completed you will receive a Project NFT with a certain Token ID, representing your DePIN Project ID on the IoTeX blockchain.

{% hint style="info" %}
Make sure you add the [Project NFT token address](https://github.com/machinefi/ioID-contracts/blob/75ee46b15284f5667cd500155f07f2e0d2544568/README.md?plain=1#L21) to your wallet app to see your projects and transfer them to other wallets if needed.
{% endhint %}

## Contract Call

**Contract:** ProjectRegistry

**Function Call:** `register() external payable returns (uint256)`

**Example:**

```solidity
let tx = await projectRegistry.registerProject();
```
