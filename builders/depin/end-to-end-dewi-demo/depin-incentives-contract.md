# DePIN Incentives Contract

{% hint style="info" %}
This documentation is work in progress.
{% endhint %}

In previous tutorials, we covered how to create the device firmware for a WiFi access point for our demo DeWi infrastructure, and how to write and deploy the W3bstream prover logic to compute rewards generated per device.

In this tutorial, we will take the next step by creating a smart contract that implements basic DePIN incentives for the owners of the WiFi routers in our network.

## **Assumptions**

1. **Device Tokenization:** a Device NFT contract for our WiFi routers has already been deployed on IoTeX. This contract tokenizes each device on-chain and the token ID represents our custom ID for the device (e.g.a serial nomber)
2. **Identity Registration:** Both the devices and their owners have their identities registered in the IoTeX [ioID Identity Module](../../../depin-infra-modules-dim/ioid-depin-identities-v1/).
3. **Device Messages:** These devices send messages about their online status and the number of WiFi clients they have served. These messages have been processed by W3bstream using a Risc0 ZK prover to determine the amount of rewards accumulated by each device based on their activity.
4. **A DePIN token** (ERC20) to be used for incentivising device owners is already deployed on IoTeX.

The computation results and their ZK proofs are sent to our smart contract by W3bstream. Our contract below will verify the proof, lookup device owners in the ioID registry, and distribute the rewards as verifiably computed by W3bstream.

## The code

**References**

* [Build a W3bstream prover using Risc0](../../../depin-infra-modules-dim/w3bstream-depin-verification/build-with-w3bstream/build-the-prover-code/risc-zero.md)
* [Risc0 Verifier contract deployment on IoTeX Testnet](https://github.com/iotexproject/w3bstream/tree/develop/smartcontracts#deployment)
* [Risc0 Verifier source code](https://github.com/iotexproject/w3bstream/blob/develop/examples/risc0-circuit/contract/RiscZeroGroth16Verifier.sol)
* [Library to parse the Journal output created by our DeWi prover](https://github.com/machinefi/iotex-dewi-demo/tree/main/blockchain/contracts/lib)

```solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.9;

// Importing the Risc0 verifier contract interface to verify ZK proofs.
import {IRiscZeroVerifier} from "./IRiscZeroVerifier.sol";

// Importing the JournalParser library to decode reward data from the W3bstream output.
// This library is used to parse the Risc0 journal, which contains a mapping
// of device IDs to their respective rewards. For example: {"0":45,"3":24,"2":53,"1":62}.
import "./lib/JournalParser.sol";

// Importing the ERC721 interface to interact with the tokenized devices.
// The contract interacts with tokenized devices to find their ioID identities.
// It is assumed that the W3bstream output returns a custom device ID instead
// of the device DID, for gas efficiency.
import "@openzeppelin/contracts/token/ERC721/IERC721.sol";

// Importing the ioID registry interface to look up device identities.
// The contract uses the ioID registry to find the ioID identity associated with a given device NFT.
import "./interfaces/IioIDRegistry.sol";

// Importing the ioID contract interface to find device owners.
// The contract interacts with the ioID contract to look up the owner accounts of devices.
import "./interfaces/IioID.sol";

// Importing the ERC20 interface to distribute rewards in the form of tokens.
// The contract uses an ERC20 token to distribute rewards to device owners.
import "@openzeppelin/contracts/token/ERC20/IERC20.sol";

// Importing the Ownable contract to manage ownership and administrative functions.
// The Ownable contract is used to restrict access to certain functions to the contract owner.
import "@openzeppelin/contracts/access/Ownable.sol";

contract DePINDappContract is Ownable {
    // Reference to the Risc0 Verifier contract used for verifying ZK proofs.
    IRiscZeroVerifier private verifier;
    // The image ID of the Risc0 prover program.
    bytes32 imageId;
    // Reference to the ioID NFT identity contract.
    IioID public ioID;
    // Reference to the ioID registry contract.
    IioIDRegistry public ioIDRegistry;
    // Reference to the ERC20 token used for distributing rewards.
    IERC20 private token;

    // Event emitted when a proof is successfully verified.
    event ProofVerified(address sender, bytes32 imageId, bytes32 postStateDigest);
    // Event emitted when rewards are distributed to device owners.
    event RewardsDistributed(address receiver, uint256 amount);

    // Constructor to initialize the contract with the necessary addresses.
    // Takes addresses of the verifier, token, ioID, and ioID registry contracts as input.
    constructor(
        address _verifierAddress,
        address _tokenAddress,
        address _ioIDAddress,
        address _ioIDRegistryAddress
    ) {
        verifier = IRiscZeroVerifier(_verifierAddress);
        ioID = IioID(_ioIDAddress);
        ioIDRegistry = IioIDRegistry(_ioIDRegistryAddress);
        token = IERC20(_tokenAddress);
    }

    // Function to verify the ZK proof and distribute rewards based on the journal data.
    // Takes project ID, prover ID, task ID, and encoded data containing the proof and journal as input.
    function verifyAndExecute(
        uint256 _projectId,
        uint256 _proverId, 
        uint256 _taskId, 
        bytes calldata _data
    ) external {
        // Decode the input data to extract the proof seal and journal.
        (bytes memory proof_snark_seal, bytes memory proof_snark_journal) = abi.decode(_data, (bytes, bytes));
        
        // Calculate the SHA-256 hash of the proof journal.
        bytes32 proof_journal_hash = sha256(proof_snark_journal);

        // Verify the ZK proof using the Risc0 verifier contract.
        require(verifier.verifySnark(proof_snark_seal, imageId, proof_journal_hash), "Proof verification failed.");
        
        // Emit the ProofVerified event upon successful verification.
        emit ProofVerified(msg.sender, imageId, proof_journal_hash);

        // Decode the deviceID-rewards mapping from the journal data.
        (JournalParser.Device[] memory devices, uint256 devicesLen) = JournalParser.parseDeviceJson(proof_snark_journal);

        // Distribute rewards to the owners of the devices.
        _distributeRewards(devices, devicesLen);
    }

    // Internal function to distribute rewards to device owners.
    // Takes an array of devices with their rewards and the length of the array as input.
    function _distributeRewards(JournalParser.Device[] memory devices, uint256 devicesLen) internal {
        for (uint256 i = 0; i < devicesLen; i++) {
            uint256 deviceId = devices[i].id;

            // Get the ioID token ID associated with the device ID.
            uint256 ioIDTokenId = ioIDRegistry.deviceTokenId(deviceId);

            // Get the owner of the ioID token ID.
            address owner = ioID.ownerOf(ioIDTokenId);

            // Transfer the reward amount to the device owner.
            require(token.transfer(owner, devices[i].reward), "Token transfer failed");
            // Emit the RewardsDistributed event after successful transfer.
            emit RewardsDistributed(owner, devices[i].reward);
        }
    }

    // Function to update the verifier contract address (only callable by the owner).
    // Takes the new verifier contract address as input.
    function updateVerifierAddress(address newVerifierAddress) external onlyOwner {
        verifier = IRiscZeroVerifier(newVerifierAddress);
    }

    // Function to update the token contract address (only callable by the owner).
    // Takes the new token contract address as input.
    function updateTokenAddress(address newTokenAddress) external onlyOwner {
        token = IERC20(newTokenAddress);
    }
    
    // Function to set the image ID of the Risc0 prover (only callable by the owner).
    // Takes the new image ID as input.
    function setImageId(bytes32 _imageId) public onlyOwner {
        imageId = _imageId;
    }

    // Function to get the image ID of the Risc0 prover.
    // Returns the current image ID.
    function getImageId() public view returns (bytes32) {
        return imageId;
    }
}

```
