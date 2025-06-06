# ioID Identifier & DID Document Structure

## ioID Identifier

Each ioID is a Decentralized Identifier (DID) formatted as:

```
did:io:<identifier_string>
```

The identifier\_string is derived from a combination of:

* A **unique project name**
* A **project-specific device identifier** (e.g., serial number, IMEI, MAC address)

<figure><img src="../../../.gitbook/assets/image (2).png" alt=""><figcaption></figcaption></figure>

**Example**

`did:io:5RJ1UfUCLX68KeFMmuvaVsm6m5Y7yH1bkF5CC4WpHXxa`

This `did:io` format uniquely and immutably identifies a physical device in a decentralized manner.

## DID Document

Each `did:io` identifier is associated with a **DID Document**, a JSON-LD metadata object stored securely on **IPFS**. The document describes key attributes of the device and its ownership.

It may include:

* Public keys or verification methods
* Authentication and authorization settings
* Device owner or controller information
* Associated services (e.g., data feeds, project memberships)
* Machine-bound account (MBA) references

## DID Doc Example (Without Public Key)

```json
{
  "@context": [
    "https://www.w3.org/ns/did/v1",
    "https://w3id.org/security/suites/ed25519-2020/v1" // Assuming a context for owner's potential signature or controller key
  ],
  "id": "did:io:abcdef1234567890geodnet-serial-kitchen-temp-sensor", // Example did:io identifier
  "controller": "did:ethr:0xOwnerWalletAddress", // Example: Controlled by the owner's Ethereum wallet DID
                                                // Alternatively, this could be the project's DID.
  "service": [
    {
      "id": "did:io:abcdef1234567890geodnet-serial-kitchen-temp-sensor#depin-project",
      "type": "DePINProjectMembership",
      "serviceEndpoint": "https://depinscan.io/projects/geodnet",
      "projectName": "GEODNET", //
      "deviceIdentity": "serial-kitchen-temp-sensor" //
    },
    {
      "id": "did:io:abcdef1234567890geodnet-serial-kitchen-temp-sensor#owner",
      "type": "DeviceOwner",
      "serviceEndpoint": "https://depinscan.io/owner/0xOwnerWalletAddress" // Link to owner information if available publicly
    }
  ],
  "assertionMethod": [ // The owner/controller would make assertions on behalf of this DID
    "did:ethr:0xOwnerWalletAddress#key-1"
  ],
  "authentication": [ // The owner/controller authenticates on behalf of this DID
    "did:ethr:0xOwnerWalletAddress#key-1"
  ]
}
```

### DID Doc Example (With Public Key)

```json
{
  "@context": [
    "https://www.w3.org/ns/did/v1",
    "https://w3id.org/security/suites/ed25519-2020/v1" // Example context for Ed25519 keys
  ],
  "id": "did:io:5RJ1UfUCLX68KeFMmuvaVsm6m5Y7yH1bkF5CC4WpHXxa", // Using example from PRD [cite: 47]
  "verificationMethod": [
    {
      "id": "did:io:5RJ1UfUCLX68KeFMmuvaVsm6m5Y7yH1bkF5CC4WpHXxa#keys-1",
      "type": "Ed25519VerificationKey2020", // Type updated to a more current one if applicable
      "controller": "did:io:5RJ1UfUCLX68KeFMmuvaVsm6m5Y7yH1bkF5CC4WpHXxa", // Device controls its own key
      "publicKeyMultibase": "zH3C2AVvLMfThF3xGk8qEsvTZbW2nK7QULaFVpJ6zC1db" // publicKeyBase58 from PRD [cite: 60] example, converted to multibase format
    }
  ],
  "authentication": [
    "did:io:5RJ1UfUCLX68KeFMmuvaVsm6m5Y7yH1bkF5CC4WpHXxa#keys-1"
  ],
  "assertionMethod": [
    "did:io:5RJ1UfUCLX68KeFMmuvaVsm6m5Y7yH1bkF5CC4WpHXxa#keys-1"
  ],
  "capabilityInvocation": [ // Example: for invoking capabilities like signing transactions for its MBA
    "did:io:5RJ1UfUCLX68KeFMmuvaVsm6m5Y7yH1bkF5CC4WpHXxa#keys-1"
  ],
  "capabilityDelegation": [ // Example: for delegating capabilities
    "did:io:5RJ1UfUCLX68KeFMmuvaVsm6m5Y7yH1bkF5CC4WpHXxa#keys-1"
  ],
  "service": [
    {
      "id": "did:io:5RJ1UfUCLX68KeFMmuvaVsm6m5Y7yH1bkF5CC4WpHXxa#mba",
      "type": "MachineBoundAccount",
      "serviceEndpoint": "erc6551:1/0xNFTContractAddress/TokenID_Corresponding_To_ioID" // Example URI to locate MBA info
    },
    {
      "id": "did:io:5RJ1UfUCLX68KeFMmuvaVsm6m5Y7yH1bkF5CC4WpHXxa#depin-data",
      "type": "DePINDataService",
      "serviceEndpoint": "https://api.project.example/device/5RJ1UfUCLX68KeFMmuvaVsm6m5Y7yH1bkF5CC4WpHXxa/data"
    }
  ]
}
```

## Summary

* `did:io` identifiers are **globally unique**, **decentralized**, and **resolvable** via IPFS-hosted DID Documents.
* These documents define who **controls a device**, how it **authenticates**, and what **services or projects it belongs to**.
* Public keys are optional but enable advanced use cases like **self-sovereign devices** or **on-device signing**.
