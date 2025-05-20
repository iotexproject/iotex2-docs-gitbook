# Register a Device

After binding a Device NFT contract to a DePIN project and requesting a specific number of ioIDs for the project, physical devices can be registered in the ioIDRegistry contract, effectively "_activating_" the ioID.&#x20;

{% hint style="info" %}
[→ Jump to the hands-on tutorial](../../../builders/depin/ioid-step-by-step-tutorial/)
{% endhint %}

This activation process must be carried out by the device owner and involves minting an ioID NFT to the device owner's account. During the minting process the ioID is associated with a specific Device NFT token, where the device's DID (Decentralized Identifier) serves as the token's ID.&#x20;

## Contract Call

**Contract:** ioIDRegistry

**Function Call:** `function register(address deviceContract, uint256 tokenId, address device, bytes32 hash, string calldata uri, uint8 v, bytes32 r, bytes32 s ) external`

**Example:**

```solidity
// Device NFT contract address
const deviceContract = "0xabcdefabcdefabcdefabcdefabcdefabcdefabcdef"; 
// Token ID of the Device NFT
const tokenId = 1; 
// Address of the device's DID
const device = "0x9876543210abcdef9876543210abcdef98765432"; 
// Example hash of the device DID document
const hash = ""; 
// IPFS URI of the DID document
const uri = "ipfs://QmTzQ1N12ucF18hTP8mViqU2LPoQzZiLjipMuYYgQ6gXht"; 
// Example signature value for v
const v = 27; 
// Example signature value for r
const r = "0x47a204cd03a7c45a3ad68ef3d5d8b979a4e84ebcd57934055050c2d2e5a6eaa5"; 
// Example signature value for s
const s = "0x2c4c75c3b79a5a8a2bb9cb1bb25a178fb20abbb0d1e6dd6c0c8b27ed5a9538b5"; 

// ...

// Function to register the device
const tx = await ioIDRegistry.register(deviceContract, tokenId, device, hash, uri, v, r, s);
        
```

Please refer to this code to clarify the meaning of the method call arguments:

