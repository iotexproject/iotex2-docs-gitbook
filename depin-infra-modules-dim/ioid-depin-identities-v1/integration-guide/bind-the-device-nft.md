# Bind your Device NFT

{% hint style="warning" %}
Ensure you have already registered a Project ID&#x20;

[↗ Register a DePIN Project](register-a-depin-project.md)
{% endhint %}

After registering a DePIN project, the next step is to set the _Device NFT_ contract for that project. This contract must be set before any user attempts to register a device ioID for your project.

{% hint style="info" %}
While the Device NFT contract is required to implement the ERC-721, the ioID framework does not impose specific implementation requirements, allowing for project-specific customization.&#x20;
{% endhint %}

Your Device NFT contract must be linked to your Project ID and only users who own a valid device NFT for a DePIN project are entitled to register a new ioID identity for a physical device for that project and bind it to their blockchain wallet.&#x20;

While the Device token contract could be a plain NFT721, below is an example for a typical customization that can be used as a Device NFT by a DePIN project.

<details>

<summary>Example Device NFT Contract</summary>

```solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.0;

import "@openzeppelin/contracts/token/ERC721/ERC721.sol";
import "@openzeppelin/contracts/access/Ownable.sol";

contract DeviceNFT is ERC721, Ownable {
    event MinterConfigured(address indexed minter, uint256 minterAllowedAmount);
    event MinterRemoved(address indexed minter);

    mapping(address => bool) internal minters;
    mapping(address => uint256) internal minterAllowed;
    uint256 nextId;

    constructor() ERC721("Example Device NFT", "EDN") {}

    function minterAllowance(address minter) external view returns (uint256) {
        return minterAllowed[minter];
    }

    function isMinter(address account) external view returns (bool) {
        return minters[account];
    }

    function configureMinter(address _minter, uint256 _minterAllowedAmount) external onlyOwner {
        minters[_minter] = true;
        minterAllowed[_minter] = _minterAllowedAmount;
        emit MinterConfigured(_minter, _minterAllowedAmount);
    }

    function removeMinter(address _minter) external onlyOwner {
        minters[_minter] = false;
        minterAllowed[_minter] = 0;
        emit MinterRemoved(_minter);
    }

    function mint(address _to) external returns (uint256) {
        require(_to != address(0), "zero address");

        uint256 mintingAllowedAmount = minterAllowed[msg.sender];
        require(mintingAllowedAmount > 0, "exceeds minterAllowance");
        unchecked {
            minterAllowed[msg.sender] -= 1;
        }

        uint256 _tokenId = ++nextId;
        _mint(_to, _tokenId);
        return _tokenId;
    }
}
```

</details>

{% hint style="info" %}
[Learn how to deploy an NFT token contract on IoTeX](../../../builders/defi/deploy-tokens/deploy-an-nft-token.md)
{% endhint %}

## Bind the Device NFT using IoTeX CLI (`ioctl`)

```
ioctl ioid device -p [PROJECT_ID] [DEVICE_NFT_CONTRACT_ADDRESS]
```

## Contract Call

**Contarct:** ioIDStore

**Function Call:** `setDeviceContract(uint256 _projectId, address _contract) external`

**Example:**

```solidity
let tx = await ioIDStore.setDeviceContract(41, "0x9876543210abcdef9876543210abcdef98765432");
```

