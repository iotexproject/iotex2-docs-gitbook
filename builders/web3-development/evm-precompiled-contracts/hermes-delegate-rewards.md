---
hidden: true
---

# HERMES Delegate Rewards

## Overview

**HERMES** is a tool developed by the IoTeX team designed to simplify and automate the rewards distribution process for delegates within the IoTeX network. The platform ensures transparency and efficiency in how rewards are managed and distributed to voters.

## How HERMES Works

In the IoTeX network, every delegate has two options for distributing rewards:

1. **Manual Distribution** – Delegates can choose to distribute rewards based on their own custom schedules and logic.
2. **Opt-in to HERMES** – Alternatively, delegates can opt into HERMES for automatic rewards distribution.

When a delegate chooses to use HERMES, the Dapp will:

* Receive the delegate's rewards from the IoTeX protocol.
* Distribute those rewards daily based on parameters set by the delegate operator.

All distributions are performed **on-chain** via a smart contract, ensuring transparency and verifiability. This process allows both delegates and community members to [index the contract ](../chain-indexing/)and create custom analytics for reporting purposes.

## Identifying Delegates Using HERMES

Delegates that utilize the HERMES system can be easily identified on the **IoTeX Staking Portal** by a small "wing" icon displayed next to their name. This visual marker helps voters quickly recognize which delegates are using the HERMES system for reward distribution.

<figure><img src="../../../.gitbook/assets/image (3) (1) (1) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

## Accessing Distribution Reports

Voters can access detailed reports about the reward distribution process at [hermes.to](https://hermes.to/). These reports provide transparency and allow voters to verify how rewards are being managed and distributed.

## Smart Contract Details

HERMES operates via a smart contract on the IoTeX blockchain, ensuring transparency and immutability of the rewards distribution process.

**Smart Contract Address:** `0x4839f8718709d4f76c6f23625908cfdc4330ef82`

<details>

<summary><strong>Smart Contract ABI</strong></summary>

```javascript
[
    {
        "type": "constructor",
        "inputs": [
            {
                "name": "_contractStartEpoch",
                "type": "uint256"
            },
            {
                "name": "_multisendAddress",
                "type": "address"
            },
            {
                "name": "_forwardRegistrationAddress",
                "type": "address"
            },
            {
                "name": "_analyticsEndpoint",
                "type": "string"
            }
        ],
        "stateMutability": "nonpayable"
    },
    {
        "type": "function",
        "name": "addAddressToWhitelist",
        "inputs": [
            {
                "name": "addr",
                "type": "address"
            }
        ],
        "outputs": [
            {
                "name": "success",
                "type": "bool"
            }
        ],
        "stateMutability": "nonpayable"
    },
    {
        "type": "function",
        "name": "addAddressesToWhitelist",
        "inputs": [
            {
                "name": "addrs",
                "type": "address[]"
            }
        ],
        "outputs": [
            {
                "name": "success",
                "type": "bool"
            }
        ],
        "stateMutability": "nonpayable"
    },
    {
        "type": "function",
        "name": "analyticsEndpoint",
        "inputs": [],
        "outputs": [
            {
                "name": "",
                "type": "string"
            }
        ],
        "stateMutability": "view"
    },
    {
        "type": "function",
        "name": "commitDistributions",
        "inputs": [
            {
                "name": "endEpoch",
                "type": "uint256"
            },
            {
                "name": "delegateNames",
                "type": "bytes32[]"
            }
        ],
        "outputs": [],
        "stateMutability": "nonpayable"
    },
    {
        "type": "function",
        "name": "contractStartEpoch",
        "inputs": [],
        "outputs": [
            {
                "name": "",
                "type": "uint256"
            }
        ],
        "stateMutability": "view"
    },
    {
        "type": "function",
        "name": "distributeRewards",
        "inputs": [
            {
                "name": "delegateName",
                "type": "bytes32"
            },
            {
                "name": "endEpoch",
                "type": "uint256"
            },
            {
                "name": "recipients",
                "type": "address[]"
            },
            {
                "name": "amounts",
                "type": "uint256[]"
            }
        ],
        "outputs": [],
        "stateMutability": "payable"
    },
    {
        "type": "function",
        "name": "distributedAmount",
        "inputs": [
            {
                "name": "",
                "type": "bytes32"
            }
        ],
        "outputs": [
            {
                "name": "",
                "type": "uint256"
            }
        ],
        "stateMutability": "view"
    },
    {
        "type": "function",
        "name": "distributedCount",
        "inputs": [
            {
                "name": "",
                "type": "bytes32"
            }
        ],
        "outputs": [
            {
                "name": "",
                "type": "uint256"
            }
        ],
        "stateMutability": "view"
    },
    {
        "type": "function",
        "name": "distributions",
        "inputs": [
            {
                "name": "",
                "type": "bytes32"
            },
            {
                "name": "",
                "type": "uint256"
            }
        ],
        "outputs": [
            {
                "name": "distributedCount",
                "type": "uint256"
            },
            {
                "name": "amount",
                "type": "uint256"
            }
        ],
        "stateMutability": "view"
    },
    {
        "type": "function",
        "name": "endEpochs",
        "inputs": [
            {
                "name": "",
                "type": "uint256"
            }
        ],
        "outputs": [
            {
                "name": "",
                "type": "uint256"
            }
        ],
        "stateMutability": "view"
    },
    {
        "type": "function",
        "name": "forwardRegistration",
        "inputs": [],
        "outputs": [
            {
                "name": "",
                "type": "address"
            }
        ],
        "stateMutability": "view"
    },
    {
        "type": "function",
        "name": "getEndEpochCount",
        "inputs": [],
        "outputs": [
            {
                "name": "",
                "type": "uint256"
            }
        ],
        "stateMutability": "view"
    },
    {
        "type": "function",
        "name": "multisender",
        "inputs": [],
        "outputs": [
            {
                "name": "",
                "type": "address"
            }
        ],
        "stateMutability": "view"
    },
    {
        "type": "function",
        "name": "owner",
        "inputs": [],
        "outputs": [
            {
                "name": "",
                "type": "address"
            }
        ],
        "stateMutability": "view"
    },
    {
        "type": "function",
        "name": "recipientEpochTracker",
        "inputs": [
            {
                "name": "",
                "type": "bytes32"
            },
            {
                "name": "",
                "type": "address"
            }
        ],
        "outputs": [
            {
                "name": "",
                "type": "uint256"
            }
        ],
        "stateMutability": "view"
    },
    {
        "type": "function",
        "name": "removeAddressFromWhitelist",
        "inputs": [
            {
                "name": "addr",
                "type": "address"
            }
        ],
        "outputs": [
            {
                "name": "success",
                "type": "bool"
            }
        ],
        "stateMutability": "nonpayable"
    },
    {
        "type": "function",
        "name": "removeAddressesFromWhitelist",
        "inputs": [
            {
                "name": "addrs",
                "type": "address[]"
            }
        ],
        "outputs": [
            {
                "name": "success",
                "type": "bool"
            }
        ],
        "stateMutability": "nonpayable"
    },
    {
        "type": "function",
        "name": "setAnalyticsEndpoint",
        "inputs": [
            {
                "name": "_endpoint",
                "type": "string"
            }
        ],
        "outputs": [],
        "stateMutability": "nonpayable"
    },
    {
        "type": "function",
        "name": "setMultisendAddress",
        "inputs": [
            {
                "name": "_multisendAddress",
                "type": "address"
            }
        ],
        "outputs": [],
        "stateMutability": "nonpayable"
    },
    {
        "type": "function",
        "name": "transferOwnership",
        "inputs": [
            {
                "name": "newOwner",
                "type": "address"
            }
        ],
        "outputs": [],
        "stateMutability": "nonpayable"
    },
    {
        "type": "function",
        "name": "whitelist",
        "inputs": [
            {
                "name": "",
                "type": "address"
            }
        ],
        "outputs": [
            {
                "name": "",
                "type": "bool"
            }
        ],
        "stateMutability": "view"
    },
    {
        "type": "event",
        "name": "CommitDistributions",
        "inputs": [
            {
                "name": "endEpoch",
                "type": "uint256",
                "indexed": false
            },
            {
                "name": "delegateNames",
                "type": "bytes32[]",
                "indexed": false
            }
        ],
        "anonymous": false
    },
    {
        "type": "event",
        "name": "Distribute",
        "inputs": [
            {
                "name": "startEpoch",
                "type": "uint256",
                "indexed": false
            },
            {
                "name": "endEpoch",
                "type": "uint256",
                "indexed": false
            },
            {
                "name": "delegateName",
                "type": "bytes32",
                "indexed": true
            },
            {
                "name": "numOfRecipients",
                "type": "uint256",
                "indexed": false
            },
            {
                "name": "totalAmount",
                "type": "uint256",
                "indexed": false
            }
        ],
        "anonymous": false
    },
    {
        "type": "event",
        "name": "OwnershipTransferred",
        "inputs": [
            {
                "name": "previousOwner",
                "type": "address",
                "indexed": true
            },
            {
                "name": "newOwner",
                "type": "address",
                "indexed": true
            }
        ],
        "anonymous": false
    },
    {
        "type": "event",
        "name": "WhitelistedAddressAdded",
        "inputs": [
            {
                "name": "addr",
                "type": "address",
                "indexed": false
            }
        ],
        "anonymous": false
    },
    {
        "type": "event",
        "name": "WhitelistedAddressRemoved",
        "inputs": [
            {
                "name": "addr",
                "type": "address",
                "indexed": false
            }
        ],
        "anonymous": false
    }
]

    
```



</details>

## **What You Can Index**

* **Rewards distribution data**: By monitoring the `Distribute` event, you can track how much was distributed, when, and to whom (delegates and recipients).
* **Delegate rewards summary**: Use the `distributedAmount` and `distributedCount` functions to track total amounts distributed per delegate and number of distributions.
