# Verify Smart Contracts

## Overview

Smart contracts aim to be trustless, requiring users to trust the contract execution without depending on third parties. This is ensured through source code verification, allowing users to confirm that some source code matches the code executed on the IoTeX blockchain.

Source code verification differs from formal verification, which checks if the contract behaves as intended. Typically, "contract verification" refers to the former, focusing on the match between high-level source code and the blockchain's bytecode.

## Verify using a UI

If you are the contract owner and would like to verify your contract using a UI, please go to the following page and follow the requested steps:

[→ IoTeX Contract Verification UI](https://iotexscout.io/verifyContract)

Follow the guide below to verify multiple contracts using Hardhat:

## Verify using Hardhat <a href="#introduction" id="introduction"></a>

In this tutorial, we will guide you through the entire process of verifying a smart contract using [IoTeXscan](https://iotexscan.io/verify-contract). Starting with contract creation, we'll then deploy it to the IoTeX testnet, and conclude by verifying the contract through [Hardhat](https://hardhat.org/hardhat-runner/docs/guides/verifying).

Or you can refer this repository [https://github.com/iotexproject/hardhat-verify-smaple](https://github.com/iotexproject/hardhat-verify-smaple)

### Setting Up a Hardhat Project <a href="#setting-up-a-hardhat-project" id="setting-up-a-hardhat-project"></a>

1. **Install NodeJS:** Ensure NodeJS is installed on your system.
2.  **Install Hardhat:** Execute the following command to install Hardhat:



    ```sh
    npm install --save-dev hardhat
    ```
3.  **Initialize a Hardhat Project:** To start a new Hardhat project, run:



    ```sh
    npx hardhat init
    ```

    For further details on creating smart contracts with Hardhat, refer to the official tutorial.

### Coding the Contract <a href="#coding-the-contract" id="coding-the-contract"></a>

1.  **Create the Contract File:** Inside your Hardhat project directory, navigate to the `contracts` subfolder and create a file named `greeter.sol`. Insert the following code:



    ```solidity
    // SPDX-License-Identifier: UNLICENSED pragma solidity ^0.8.0; 
    import "@openzeppelin/contracts/token/ERC1155/ERC1155.sol"; 

    contract NFT is ERC1155 { 
      constructor() ERC1155("URL") { 
        _mint(msg.sender, 0, 1, ""); 
      } 
    }
    ```
2.  **Install OpenZeppelin Contracts:** Run the following command to include the OpenZeppelin contracts package:



    ```sh
    npm install @openzeppelin/contracts
    ```
3.  **Compile Your Contract:** Compile the smart contract with:



    ```sh
    npx hardhat compile
    ```

### Preparing for Deployment <a href="#preparing-for-deployment" id="preparing-for-deployment"></a>

1. **Setup IoTeX Testnet Account:** Ensure you have a developer account on the IoTeX testnet with a balance of test IOTX tokens. Visit the IoTeX documentation for instructions on account creation and funding.
2.  **Configure Hardhat:** Modify `hardhat.config.js` to add the IoTeX network configurations, including your IoTeX Developer account private key:



    ```javascript
    ...
    networks: {
        hardhat: {
        },
        mainnet: {
          url: 'https://babel-api.mainnet.IoTeX.io',
          accounts: [ PRIVATE_KEY ],
          chainId: 4689,
          gas: 8500000,
          gasPrice: 1000000000000
        },
        testnet: {
          url: 'https://babel-api.testnet.IoTeX.io',
          accounts: [ PRIVATE_KEY ],
          chainId: 4690,
          gas: 8500000,
          gasPrice: 1000000000000
        }
      },
      ...
    ```
3.  **Deploy Script:** Edit `scripts/deploy.js` with the following code to deploy your contract:



    ```javascript
    const hre = require("hardhat");

    async function main() {
      const NFT = await hre.ethers.deployContract("NFT");
      await NFT.waitForDeployment();
      console.log("NFT deployed to:", NFT.target);
    }

    main().catch((error) => {
      console.error(error);
      process.exitCode = 1;
    });
    ```
4.  **Deploy the Contract:** Deploy your contract to the IoTeX testnet by running:



    ```sh
    npx hardhat run scripts/deploy.js --network testnet
    ```


5. Note the contract address provided in the deployment log.

### Verifying the Contract <a href="#verifying-the-contract" id="verifying-the-contract"></a>

1.  **Install hardhat-verify Plugin:** To enable contract verification, install the hardhat-verify plugin:



    ```sh
    npm install --save-dev @nomicfoundation/hardhat-verify
    or
    npm install --save-dev @nomicfoundation/hardhat-toolbox
    ```
2.  **Update Hardhat Configuration:** Edit `hardhat.config.js` by adding the configuration for contract verification, make sure you use your API key:



    ````javascript
    ```typescript
    import 'dotenv/config'
    import { HardhatUserConfig } from "hardhat/config";
    import "@nomicfoundation/hardhat-toolbox";

    const config: HardhatUserConfig = {
      solidity: "0.8.20",
      networks: {
        iotex: {
          chainId: 4689,
          url: 'https://babel-api.mainnet.iotex.io',
          accounts: [
            process.env.PRIVATE_KEY!
          ]
        }
      },
      etherscan: {
        apiKey: "arbitrary value",// It's here just for hardhat arguments validation
        //It  can be set as arbitrary value
        customChains: [
          {
            network: "mainnet",
            chainId: 4689,
            urls: {
              apiURL: "https://iotexscan.io/api",
              browserURL: "https://iotexscan.io"
            }
          },
          {
            network: "testnet",
            chainId: 4690,
            urls: {
              apiURL: "https://testnet.iotexscan.io/api",
              browserURL: "https://testnet.iotexscan.io"
            }
          }
        ],
      },
    };

    export default config;

    ```
    ````
3.  **Verify the Contract:** Execute the following command, replacing `<address>` with your contract's address:



    ```sh
    npx hardhat verify --network testnet YOUR_CONTRACT_ADDRESS [...arguments in constructor]
    ```
4. Upon successful execution, you'll receive a link to your contract's verified code on IoTeXscan.
