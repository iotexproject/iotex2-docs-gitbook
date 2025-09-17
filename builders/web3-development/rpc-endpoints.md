# RPC Endpoints

Below, you find a list of official and third-party IoTeX endpoints for both full node and archive nodes. These endpoints can be used to configure any Ethereum wallet or developer tool to interact with the IoTeX blockchain.

{% hint style="info" %}
**→ Interested in running an IoTeX RPC Node?**&#x20;

Checkout the dedicated section  to learn how to run an IoTeX full node if you want to spin up an IoTeX RPC endpoint.

[→ Setup an IoTeX RPC  Node](../node-operators/setup-an-iotex-rpc-node.md)
{% endhint %}

{% hint style="info" %}
**Official Ethereum JSON-API**

Please refer to the official Ethereum Documentation for the RPC JSON API:&#x20;

[→ Ethereum JSON RPC API](https://ethereum.github.io/execution-apis/api-documentation/)&#x20;
{% endhint %}

## IoTeX Mainnet

`EVM Chain ID: 4689`

### Full Node

<table><thead><tr><th width="344.4995649735719">Endpoint</th><th width="91">Type</th><th width="173">Provider</th><th>More</th></tr></thead><tbody><tr><td>https://babel-api.mainnet.iotex.io</td><td>HTTP</td><td>IoTeX Foundation</td><td><a href="https://iotex.io">iotex.io</a></td></tr><tr><td>wss://babel-api.mainnet.iotex.io/ws</td><td>WSS</td><td>IoTeX Foundation</td><td><a href="https://iotex.io">iotex.io</a></td></tr><tr><td>https://babel-api.mainnet.iotex.one</td><td>HTTP</td><td>IoTeX Foundation</td><td><a href="https://iotex.io">iotex.io</a></td></tr><tr><td>https://babel-api.fastblocks.io</td><td>HTTP</td><td>Fastblocks</td><td><a href="https://fastblocks.io/">fastblock.io</a></td></tr><tr><td>https://iotexrpc.com</td><td>HTTP</td><td>Ankr</td><td><a href="https://iotexrpc.com">iotexrpc.com</a></td></tr><tr><td>https://rpc.ankr.com/iotex</td><td>HTTP</td><td>Ankr</td><td><a href="https://ankr.com">ankr.com</a></td></tr><tr><td>https://4689.rpc.thirdweb.com</td><td>HTTP</td><td>Thirdweb</td><td><a href="https://thirdweb.com/iotex-network">Thirdweb</a></td></tr><td>https://iotex.rpc.grove.city/v1/01fdb492</td><td>HTTP</td><td>Grove 🌿</td><td><a href="https://grove.city">Grove</a></td></tr></tbody></table>

### Archive Node

<table><thead><tr><th width="344.4995649735719">Endpoint</th><th width="91">Type</th><th width="173">Provider</th><th>More</th></tr></thead><tbody><tr><td>https://archive-mainnet.iotex.io</td><td>HTTP</td><td>IoTeX Foundation</td><td><a href="https://iotex.io">iotex.io</a></td></tr><tr><td>https://iotex.rpc.grove.city/v1/01fdb492</td><td>HTTP</td><td>Grove 🌿</td><td><a href="https://grove.city">Grove</a></td></tr></tbody></table>

## IoTeX Testnet

`EVM Chain ID: 4690`

<table><thead><tr><th width="343.5471637775761">Endpoint</th><th width="94">Type</th><th width="177">Provider</th><th>More</th></tr></thead><tbody><tr><td>https://babel-api.testnet.iotex.io</td><td>HTTP</td><td>IoTeX Foundation</td><td><a href="https://iotex.io">iotex.io</a></td></tr><tr><td>wss://babel-api.testnet.iotex.io/ws</td><td>WSS</td><td>IoTeX Foundation</td><td><a href="https://iotex.io">iotex.io</a></td></tr><tr><td> https://babel-api.testnet.iotex.one</td><td>HTTP</td><td>IoTeX Foundation</td><td><a href="https://iotex.io">iotex.io</a></td></tr><tr><td> https://babel-api.testnet.iotex.one/wss</td><td>WSS</td><td>IoTeX Foundation</td><td><a href="https://iotex.io">iotex.io</a></td></tr></tbody></table>

### **Archive Node**

<table><thead><tr><th width="344.4995649735719">Endpoint</th><th width="91">Type</th><th width="173">Provider</th><th>More</th></tr></thead><tbody><tr><td>https://archive-testnet.iotex.io</td><td>HTTP</td><td>IoTeX Foundation</td><td><a href="https://iotex.io">iotex.io</a></td></tr></tbody></table>

## **Examples**

#### **Query an IoTeX full node to get the current IOTX balance for an address**&#x20;

The code below utilizes `curl` to query the public IoTeX RPC full-node endpoint to check the balance of `0xE584...C5D46` on the IoTeX Blockchain:

```bash
» curl -X POST -H "Content-Type:application/json" --data '{"id": 1, "jsonrpc": "2.0", "method": "eth_getBalance", "params": ["0xE584ca6F469c11140Bb9c4617Cb8f373E38C5D46", ""]}' https://babel-api.mainnet.iotex.io
{
  "id": 1,
  "jsonrpc": "2.0",
  "result": "0x10f0cf064dd59200000"
}
```

Which returns the balance in WEI `0x10f0cf064dd59200000` (decimal `5000¹⁸` , equivalent to `5000 IOTX`).

#### **Query an IoTeX archive node to get the IOTX balance for an address in the past**

Input block: 30,000,000 (0x1C9C380)

```sh
curl -X POST https://archive-mainnet.iotex.io \
     -H "Content-Type: application/json" \
     -d '{ 
           "jsonrpc": "2.0", 
           "method": "eth_getBalance", 
           "params": ["0xe46be34b4b78ed661783acf8bd241d0d074ce9ff", "0x1C9C380"], 
           "id": 1 
         }'
```

#### Query all Transfer events for ioUSDT on IoTeX in a certain blocks range:

```sh
curl -X POST https://archive-mainnet.iotex.io \
-H "Content-Type: application/json" \
-d '{
  "jsonrpc": "2.0",
  "method": "eth_getLogs",
  "params": [{
    "fromBlock": "0x1ce29c0",
    "toBlock": "0x1ce29e0",
    "address": "0x6fbcdc1169b5130c59e72e51ed68a84841c98cd1",
    "topics": ["0xddf252ad1be2c89b69c2b068fc378daa952ba7f163c4a11628f55a4df523b3ef"]
  }],
  "id": 1
}'
```
