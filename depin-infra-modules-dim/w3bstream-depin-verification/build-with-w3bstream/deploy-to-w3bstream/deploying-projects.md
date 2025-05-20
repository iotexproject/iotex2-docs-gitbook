# Deploying Projects

When your W3bstream project file is ready, you need to deploy it to the IoTeX blockchain for it to become discoverable by the W3bstream network. Let's go through the stpes.

## Prerequisites

{% hint style="info" %}
**Register a DePIN Project in ioID**

Ensure you have registered a DePIN project NFT on the IoTeX Blockchain and you are using the wallet account that owns the project NFT.

[→ Learn how to obtain a DePIN project NFT](../../../ioid-depin-identities-v1/integration-guide/register-a-depin-project.md)&#x20;
{% endhint %}

## Register your DePIN Project in W3bstream

The first step is to register your DePIN Project ID into the W3bstream Projects Registry. This operation has a cost of 1 IOTX per project on the IoTeX Testnet (corresponding to $$10^{18} Rau$$).

Here is an example W3bstream project registration:&#x20;

```sh
ioctl ws project register --id 1234 --amount 1000000000000000000
```

{% hint style="info" %}
**Note:** The project ID matches the Project NFT Token ID.
{% endhint %}

The output of the ioctl command will reveal the newly created W3bstream project's settings and status:

```javascript
{
  "projectID": 1234,
  "owner": "io1qr384jh3602cscwlwyr3nlyhcslujahkhmwmpj",
  "uri": "",
  "hash": "0000000000000000000000000000000000000000000000000000000000000000",
  "isPaused": true
}
```

## Set the W3bstream project file

With your project registered in W3bstream it's now possible to deploy your project file:

```sh
ioctl ws project update --id 1234 --path "test/project/10000"
```

The output of the command will show the new status of the project:

```javascript
project updated
{
  "projectID": 6459,
  "owner": "io1qr384jh3602cscwlwyr3nlyhcslujahkhmwmpj",
  "uri": "ipfs://ipfs.mainnet.iotex.io/QmbwfizPGXUBijMuVDi2YgGcgdHWeSaU8kpNFojtuVRPvb",
  "hash": "2d89e8aa5b76f2ba70137b53778ed31e5ddd2022c927f7955d54f51f127d2dd4",
  "isPaused": true
}
```

The project file has been uploaded to IPFS and the uri  along with the hash has been set for your project in the W3bstream registry.

The last thing to note is that your project is paused by default, which means W3bstream nodes will not process any task available on the DA Layer that targets your project until the project activity is resumed.

## Start the project

```sh
ioctl ws project resume --id 1234
```
