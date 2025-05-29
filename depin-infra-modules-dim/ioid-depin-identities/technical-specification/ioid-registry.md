# ioID Registry

The ioID Registry is a smart contract deployed on the IoTeX Layer 1 blockchain. It acts as the authoritative on-chain source for mapping physical devices to their corresponding:

* **Device NFTs (ERC-721 tokens)**
* **Owners’ wallet addresses**
* **Machine-Bound Accounts (MBAs)**

This registry extends the logic defined in [ERC-6551](https://eips.ethereum.org/EIPS/eip-6551), which enables NFTs to serve as smart contract wallets through token-bound accounts. The ioID Registry adapts and applies this model specifically to DePIN devices.

## Example ioID Registry Entry

The following table shows a sample entry in the ioID Registry, illustrating how a device identifier is linked to its NFT representation:

<table><thead><tr><th width="300.87109375">ioID Identifier</th><th width="114.25">NFT chainID</th><th>NFT contract</th><th>NFT tokenID</th></tr></thead><tbody><tr><td><code>5RJ1UfUCLX6...yH1bkF5CC4WpHXxa</code></td><td>1</td><td><code>0x0ca53...400c</code></td><td>123</td></tr></tbody></table>

This mapping allows contracts and off-chain services to resolve device identity, ownership, and associated MBAs using deterministic lookups.

## Key Functions

### **Device Ownership Lookup**

To determine the current owner of a device:

```solidity
ownerOf(uint256 tokenid)
```

This function is called on the NFT contract. The tokenId corresponds to the unique identifier for the device’s NFT.

### **MBA Address Resolution**

To retrieve the MBA (Machine-Bound Account) address associated with a device, call the account(...) function in the MBA factory contract:

```solidity
function account(
    address implementation,
    bytes32 salt,
    uint256 chainId,
    address tokenContract,
    uint256 tokenId
) external view returns (address account)
```

This function returns the on-chain address of the MBA smart contract tied to the specific device NFT. This allows external Dapps to securely interact with the device’s programmable wallet.
