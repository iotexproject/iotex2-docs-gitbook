# Exchange Integration

This guide will walk you through the process of integrating the IoTeX token (`$IOTX`) on your exchange. Since IoTeX is fully **Ethereum-compatible**, most integration steps will resemble those of any EVM blockchain. Below are the detailed steps, requirements, and resources necessary for seamless integration.

If you require further assistance, please don’t hesitate to [reach out to our support team](exchange-integration.md#id-8.-support-and-contact).

## 1. Overview of IoTeX and $IOTX Token

IoTeX is a leading decentralized **layer-1 blockchain,** focused on enabling decentralized physical infrastructure networks (DePIN). The IoTeX native token, `$IOTX`, powers the IoTeX ecosystem by facilitating blockchain transactions, staking, governance, as well as data computing and identities management for DePINs.

• **Token Name:** IoTeX

• **Ticker Symbol**: IOTX

• **Blockchain**: IoTeX Mainnet

• **Standard**: Native Protocol Token

## 2. Blockchain Basics and Compatibility

### IoTeX Mainnet Information

• **Network Name**: IoTeX Mainnet

• **Chain ID**: 4689

• **Symbol**: IOTX

**• RPC URL**: https://babel-api.mainnet.iotex.io (see [more endpoints](../../builders/web3-development/rpc-endpoints.md#iotex-mainnet))&#x20;

• **Explorer URL**: [https://iotexscan.io](https://iotexscan.io)

### IoTeX Testnet Information

**• Network Name**: IoTeX Testnet

**• Chain ID**: 4690

• **Symbol**: IOTX

**• RPC URL**: https://babel-api.testnet.iotex.io (see [more endpoints](../../builders/web3-development/rpc-endpoints.md#iotex-testnet))

• **Explorer URL**: [https://testnet.iotexscan.io](https://testnet.iotexscan.io)

{% hint style="success" %}
Since IoTeX is **fully compatible with Ethereum’s virtual machine (EVM)**, exchanges can use familiar Ethereum tools and libraries (such as Web3.js, Ethers.js, and Truffle) for `$IOTX` integration.
{% endhint %}

## 3. Wallet Integration

To support deposits and withdrawals for `$IOTX`, exchanges need to implement **wallet integration**. As IoTeX is fully Ethereum-compatible, most Ethereum-compatible wallets (such as MetaMask, Trust Wallet, and hardware wallets like Ledger) will work seamlessly for managing `$IOTX` tokens.

{% hint style="info" %}
Metamask-compatible wallets can also be automatically configured on the [IoTeX Developer Portal](https://developers.iotex.io).
{% endhint %}

### To integrate $IOTX into your platform’s wallet

• Use Ethereum-compatible libraries like Web3.js or Ethers.js to interact with the IoTeX blockchain.

• Set the correct RPC endpoint to ensure interaction with the IoTeX blockchain.

#### Example:

```javascript
const Web3 = require('web3');
const web3 = new Web3('https://babel-api.mainnet.iotex.io');

// Query the balance of an address
web3.eth.getBalance('0x4d8a5c5b52f45fcfd2ce7eea58390b214909ef24').then(console.log);
```

## 4. Node Setup and API Endpoints

Exchanges can either use public RPC endpoints for convenience, or set up their own IoTeX nodes **for higher reliability and security**.

{% hint style="warning" %}
Running your own node ensures maximum control over latency, security, and performance.
{% endhint %}

### Full Node Setup

If you prefer to run your own full node, you can follow the [IoTeX full node setup guide](https://github.com/iotexproject/iotex-bootstrap#release-status).&#x20;

### Public RPC Endpoints

• **IoTeX Mainnet:**  https://babel-api.mainnet.iotex.io (see [more endpoints](../../builders/web3-development/rpc-endpoints.md#iotex-mainnet))&#x20;

• **IoTeX Testnet**: https://babel-api.testnet.iotex.io (see [more endpoints](../../builders/web3-development/rpc-endpoints.md#iotex-testnet))

## 5. Deposits and Withdrawal

### Deposit Flow

<details>

<summary>First-Time Integration of IoTeX Blockchain</summary>

To integrate `$IOTX` deposits:

1. **Generate a 0x Deposit Address**: Use any Web3 tool to create a deposit address for users.
2. **Compute `io1` Address**: Since some users still use the older `io1` address format, compute the corresponding `io1` address (both addresses represent the same account and share the same private key - [see examples](../../builders/reference-docs/native-iotex-development/account-cryptography.md#address-conversion-examples)).
3. **Address Display**: Show the 0x address as the primary option, with the io1 as an alternative.
4. **Monitor Transactions**: Use the [IoTeXscan API](https://index.iotexscan.io/doc/ui) or direct RPC queries to your full node to monitor the IoTeX blockchain for deposits.
5. **Transaction Confirmations**: Confirm transactions according to your policy, noting that IoTeX transactions are final after the first block.

</details>

<details>

<summary>Existing IoTeX Integration</summary>

1. **If you support Multiple Networks**: Ensure users can select the IoTeX blockchain as the deposit network.
2. **Compute `0x` Address**: If you already use the native `io1` format, always compute the corresponding `0x` address too (both formats share the same private key and represent the same account - [see examples](../../builders/reference-docs/native-iotex-development/account-cryptography.md#address-conversion-examples)).
3. **Address Display**: Present the `0x` address as the primary option and the `io1` as an alternative.
4. **Monitor Transactions**: Use the [IoTeXscan API](https://index.iotexscan.io/doc/ui) or direct RPC queries to your full node to monitor the IoTeX blockchain for deposits.
5. **Transaction Confirmations**: Confirm transactions according to your policy, noting that IoTeX transactions are final after the first block.

</details>

<figure><img src="../../.gitbook/assets/image (2) (1) (1) (1) (1) (1).png" alt=""><figcaption><p>Example UI for $IOTX deposit flow on the IoTeX Blockchain</p></figcaption></figure>

### Withdrawal Flow

To integrate $IOTX withdrawals:

<details>

<summary>First-Time Integration of IoTeX Blockchain</summary>

1. **Input Recipient Address**: Allow users to input withdrawal addresses in either `0x` or `io1` formats.
2. **Convert Address**: If the user provides an `io1` format address, convert it to `0x` (see how to [convert IoTeX addresses](../../builders/reference-docs/native-iotex-development/account-cryptography.md#address-conversion-examples)).
3. **Send Transactions**: Use the Ethereum-compatible `eth_sendTransaction` API with your preferred Web3 tool to generate and send withdrawal transactions.

</details>

<details>

<summary>Existing IoTeX Integration</summary>

1. **Network Selection**: Ensure users can select the IoTeX Native blockchain for withdrawals if you support multiple networks.
2. **Input Recipient Address**: Accept both `0x` and `io1` formats for the withdrawal address.
3. **Convert Address**: Convert the recipient address to `0x` if they provide `io1`, or convert it back if using the IoTeX native API ([see examples](../../builders/reference-docs/native-iotex-development/account-cryptography.md#address-conversion-examples)).
4. **Send Transaction**: Generate and send the withdrawal transaction using the Ethereum-compatible `eth_sendTransaction` API with your preferred Web3 tool or the IoTeX native API/SDK based on your configuration.

</details>

{% hint style="warning" %}
Use the correct gas price and gas limit based on IoTeX network conditions.
{% endhint %}

#### Example transaction code (using Web3.js):

```javascript
const Tx = require('ethereumjs-tx').Transaction;
const privateKey = Buffer.from('your_private_key', 'hex');

const txData = {
  nonce: web3.utils.toHex(txCount),
  gasLimit: web3.utils.toHex(21000),
  gasPrice: web3.utils.toHex(web3.utils.toWei('10', 'gwei')),
  to: 'recipient_address',
  value: web3.utils.toHex(web3.utils.toWei('1', 'ether')),
  chainId: 4689, // IoTeX Mainnet Chain ID
};

const tx = new Tx(txData);
tx.sign(privateKey);

const serializedTx = tx.serialize();
const rawTx = '0x' + serializedTx.toString('hex');
web3.eth.sendSignedTransaction(rawTx)
  .on('receipt', console.log);
```

## 6. Trading and Smart Contract Integration

### Smart Contract Compatibility

Since IoTeX is EVM-compatible, it supports all Ethereum smart contracts, including those written in Solidity. If your exchange supports Ethereum-based trading, you can use the same mechanisms to enable `$IOTX` trading.

## 7. Listing $IOTX

To list `$IOTX`, use the following token details:

• **Symbol**: IOTX

• **Decimals**: 18

## 8. Security Considerations

• **Private Key Management**: Ensure private keys are stored securely (e.g., using HSMs, cold storage, or other secure key management systems).

• **Multi-Sig Wallets**: Implement multi-signature wallets for withdrawals to minimize the risk of unauthorized transactions.

• **Monitoring and Alerts:** Set up monitoring for large transactions, unusual activity, and sudden spikes in gas fees.

## 8. Support and Contact

For any issues or further questions regarding the `$IOTX` integration, feel free to contact the IoTeX team:

• **Email**: developers@iotex.io

• **Official Documentation**: [https://docs.iotex.io](https://docs.iotex.io)

• **Developer Resources**: [IoTeX Developer Portal](https://dev.iotex.io)
