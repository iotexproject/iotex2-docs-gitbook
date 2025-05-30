# Machine-Bound Accounts (MBA)

Machine-Bound Accounts (MBAs) are smart contract wallets bound to device NFTs using the [ERC-6551](https://eips.ethereum.org/EIPS/eip-6551) standard. They enable autonomous, secure interactions between devices (or agents) and on-chain applications — **making machines first-class actors in the blockchain ecosystem.**

MBAs bring programmability, ownership enforcement, and permissioned execution to devices, allowing them to:

* **Receive and manage rewards**
* **Perform on-chain staking or lending**
* **Execute delegated actions**
* **Interact with Dapps autonomously or via authorized signers**

## How MBAs Work

An MBA is a smart contract wallet that is:

* Deterministically generated based on a device unique ID (e.g., serial number, IMEI number, etc..)
* Fully programmable, just like externally owned accounts (EOAs) or other smart contract wallets
* Securely bound to the identity and ownership of the NFT

This binding ensures that only the owner of the device can control or delegate permissions for the MBA.

## MBA Generation

MBA generation follows the [ERC-6551 registry specification](https://eips.ethereum.org/EIPS/eip-6551#registry). The account address is computed from the following parameters:

* Implementation contract address
* Unique salt value
* Chain ID
* NFT contract address
* NFT token ID

This process is deterministic — meaning the same device will always resolve to the same MBA wallet address.

## Example MBA Addresses

While ERC-6551 is EVM-specific, the concept of token-bound accounts can be adapted to other chains like Solana, using comparable account abstraction primitives.

<table data-header-hidden><thead><tr><th width="150.8046875">Chain</th><th>Example MBA Address</th></tr></thead><tbody><tr><td>EVM</td><td><code>0xb2119aF10cc641162968ED255dC4E688dC4B2B6E</code></td></tr><tr><td>Solana</td><td><code>FDNY674k4dC9zoGSnGzaajjYnXoKKvAAp3GZU7sAfnGX</code></td></tr></tbody></table>

## When is an MBA Created?

In practice, an MBA is automatically generated when a DePIN project registers an ioID for the device on-chain
