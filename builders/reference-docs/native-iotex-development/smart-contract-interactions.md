# Smart Contract Interactions

The `iotx.Contract` class of Antenna makes it easy to interact with smart contracts on the IoTeX blockchain. When you create a new `Contract` object, just initialize it with the JSON interface (ABI) of the respective contract, and the Contract object will take care of converting all contract method calls into low-level ABI calls over RPC for you.&#x20;

{% hint style="success" %}
**Need a full example**? [Check the integration test](https://github.com/iotexproject/iotex-antenna/blob/master/src/\_\_test\_\_/iotx.integration.test.ts#L98).
{% endhint %}

* [Compiling Solidity Code](https://docs.iotex.io/developer/sdk/account-create#compiling-solidity-code)
* [Deploying a Smart Contract](https://docs.iotex.io/developer/sdk/account-create#deploying-a-smart-contract)
* [Interacting with a Smart Contract](https://docs.iotex.io/developer/sdk/account-create#interacting-with-a-smart-contract)

### Compiling Solidity Code <a href="#compiling-solidity-code" id="compiling-solidity-code"></a>

Antenna SDK does not compile solidity code itself; however you can get the ABI and bytecode of your contract in one of the following options:

* **Option 1**: in Node.js, import and use the [`solc`](https://www.npmjs.com/package/solc) node package, as in the following example:

```cpp
import solc from "solc";

const solidityFileString = `
pragma solidity ^0.4.16;

contract SimpleStorage {
   uint storedData;

   function set(uint x) public {
       storedData = x;
   }

   function get() public view returns (uint) {
       return storedData;
   }
}
`;
const contractName = ":SimpleStorage";
const output = solc.compile(solidityFileString, 1);
const abi = JSON.parse(output.contracts[contractName].interface);
const bytecode = output.contracts[contractName].bytecode;
```

* **Option 2**: on the browser side, you can use [browser-solc ](https://www.npmjs.com/package/browser-solc)package. Check out this [example from iotex-explorer](https://github.com/iotexproject/iotex-explorer/blob/master/src/shared/wallet/contract/deploy.tsx#L114).

### Deploying a Smart Contract <a href="#deploying-a-smart-contract" id="deploying-a-smart-contract"></a>

Once you get the ABI and bytecode from the step above, then you can deploy it by sending the deploy action to the IoTeX blockchain network.

{% tabs %}
{% tab title="Javascript" %}
```javascript
import Antenna from "iotex-antenna";
import { toRau } from "iotex-antenna/lib/account/utils";

(async () => {
  const antenna = new Antenna("https://api.testnet.iotex.one");

  const bytecode =
    "608060405234801561001057600080fd5b5060df8061001f6000396000f3006080604052600436106049576000357c0100000000000000000000000000000000000000000000000000000000900463ffffffff16806360fe47b114604e5780636d4ce63c146078575b600080fd5b348015605957600080fd5b5060766004803603810190808035906020019092919050505060a0565b005b348015608357600080fd5b50608a60aa565b6040518082815260200191505060405180910390f35b8060008190555050565b600080549050905600a165627a7a7230582043be766a6a271867521090c3e12130ea8891a8f59d4833bc205a3e2e2c70b4730029";

  const abi = `[
    {
      constant: false,
      inputs: [{ name: "x", type: "uint256" }],
      name: "set",
      outputs: [],
      payable: false,
      stateMutability: "nonpayable",
      type: "function"
    },
    {
      constant: true,
      inputs: [],
      name: "get",
      outputs: [{ name: "", type: "uint256" }],
      payable: false,
      stateMutability: "view",
      type: "function"
    },
    {
      inputs: [{ name: "_x", type: "uint256" }],
      payable: false,
      stateMutability: "nonpayable",
      type: "constructor"
    }
  ]`;

  const creator = antenna.iotx.accounts.privateKeyToAccount(
    "73c7b4a62bf165dccf8ebdea8278db811efd5b8638e2ed9683d2d94889450426"
  );

  const actionHash = await antenna.iotx.deployContract({
    from: creator.address,
    amount: "0",
    abi: abi,
    data: Buffer.from(bytecode, "hex"),
    gasPrice: toRau("1", "Qev"),
    gasLimit: "100000"
  });
})();
```


{% endtab %}

{% tab title="Go Lang" %}
```go
package main

import (
	"context"
	"encoding/hex"
	"fmt"
	"log"
	"math/big"
	"strings"

	"github.com/ethereum/go-ethereum/accounts/abi"

	"github.com/iotexproject/iotex-antenna-go/account"
	"github.com/iotexproject/iotex-antenna-go/iotex"
	"github.com/iotexproject/iotex-proto/golang/iotexapi"

)

func main() {
	conn, err := iotex.NewDefaultGRPCConn("api.testnet.iotex.one")
	if err != nil {
		log.Fatalf("connection error : %v", err)
	}
	defer conn.Close()

	creator, _ := account.HexStringToAccount("73c7b4a62bf165dccf8ebdea8278db811efd5b8638e2ed9683d2d94889450426")
	c := iotex.NewAuthedClient(iotexapi.NewAPIServiceClient(conn), creator)

	abi, err := abi.JSON(strings.NewReader(`[{"constant":false,"inputs":[{"name":"x","type":"uint256"}],"name":"set","outputs":[],"payable":false,"stateMutability":"nonpayable","type":"function"},{"constant":true,"inputs":[],"name":"get","outputs":[{"name":"","type":"uint256"}],"payable":false,"stateMutability":"view","type":"function"},{"inputs":[{"name":"_x","type":"uint256"}],"payable":false,"stateMutability":"nonpayable","type":"constructor"}]`))
	if err != nil{
		log.Fatalf("JSON error : %v", err)
	}
	bytecode := "608060405234801561001057600080fd5b5060df8061001f6000396000f3006080604052600436106049576000357c0100000000000000000000000000000000000000000000000000000000900463ffffffff16806360fe47b114604e5780636d4ce63c146078575b600080fd5b348015605957600080fd5b5060766004803603810190808035906020019092919050505060a0565b005b348015608357600080fd5b50608a60aa565b6040518082815260200191505060405180910390f35b8060008190555050565b600080549050905600a165627a7a7230582043be766a6a271867521090c3e12130ea8891a8f59d4833bc205a3e2e2c70b4730029"
	data, err := hex.DecodeString(bytecode)
	if err != nil{
		log.Fatalf("Hex Decoding error : %v", err)
	}

	actionHash, err := c.DeployContract(data).SetGasPrice(big.NewInt(1)).SetGasLimit(1000000).SetArgs(abi, big.NewInt(10)).Call(context.Background())
	if err != nil {
		log.Fatalf("deploy error: %v", err)
	}
	fmt.Println(actionHash)
}
```


{% endtab %}
{% endtabs %}

Once the action is broadcast, you can query it:

{% tabs %}
{% tab title="Javascript" %}
```cpp
const receipt = await antenna.iotx.getReceiptByAction({
  actionHash: actionHash
});
```


{% endtab %}

{% tab title="Golang" %}
```go
action, err := wallet.Iotx.GetActions(&iotexapi.GetActionsRequest{
  Lookup: &iotexapi.GetActionsRequest_ByHash{
    ByHash: &iotexapi.GetActionByHashRequest{
      ActionHash:   actionHash,
      CheckPending: true,
    },
  },
})
```


{% endtab %}
{% endtabs %}

Finally, once the action is minted, you can query the receipt

{% tabs %}
{% tab title="Javascript" %}
```javascript
const receipt = await antenna.iotx.getReceiptByAction({
  actionHash: actionHash
});
```


{% endtab %}

{% tab title="Golang" %}
```go
receipt, err := wallet.Iotx.GetReceiptByAction(&iotexapi.GetReceiptByActionRequest{
  ActionHash: actionHash,
})
```


{% endtab %}
{% endtabs %}
