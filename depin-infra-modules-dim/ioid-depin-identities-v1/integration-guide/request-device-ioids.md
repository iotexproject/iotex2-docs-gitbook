# Reserve Device ioIDs

DePIN projects must **reserve** a specific number of ioIDs by paying the required amount before they can register their devices on IoTeX. ioIDs can only be reserved by DePIN projects that are **registered** on IoTeX, so please ensure that your project [is registered](register-a-depin-project.md) and you own the `Project ID` before continuing.

{% hint style="info" %}
You will pay the required amount to complete the application for the number of ioIDs requested. The current cost on the IoTeX testnet is `0.1 IOTX` per ioID.&#x20;
{% endhint %}

## Using the IoTeX CLI (`ioctl`)

```sh
ioctl ioid apply --project-id <your_project_id> --amount <number_of_ioIDs>
```

After the transaction, the requested number of ioIDs will be reserved to your Project ID.

## Contract Call

**Contract:** ioIDStore

**Function Call:** `applyIoIDs(uint256 _projectId, uint256 _amount) external payable`&#x20;

**Example:**

```solidity
const { ethers } = require('ethers');
// ...

// Create a wallet signer
const wallet = new ethers.Wallet(PRIVATE_KEY, provider);

// --- Instantiate the Contract ---
const ioIDStore = new ethers.Contract(IOIDSTORE_ADDRESS, ABI, wallet);

// Cost per ioID in IOTX
const costPerIoID = ethers.utils.parseEther('0.1'); // 0.1 IOTX

const numberOfIoIDs = 10;
const projectID = 41;

// Calculate total cost using BigNumber to avoid precision issues
const totalCost = costPerIoID.mul(numberOfIoIDs); // 0.1 IOTX * 10 = 1 IOTX

// Send the transaction with the required value
try {
  const tx = await ioIDStore.apply(projectID, numberOfIoIDs, {
    value: totalCost
  });
  console.log('Transaction submitted:', tx.hash);
  
  // Wait for the transaction to be mined
  await tx.wait();
  console.log('Transaction confirmed');
} catch (error) {
  console.error('Error applying for ioIDs:', error);
}
```
