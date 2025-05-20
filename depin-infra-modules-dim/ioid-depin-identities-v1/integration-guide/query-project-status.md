# Query Project Status

You can check the project status and details using the ioctl command below:

```sh
ioctl ioid project --projectId <your-project-id>

Project #41 detail:
{
	"projectContractAddress": "0x6972C35dB95258DB79D662959244Eaa544812c5A",
	"deviceNFT": "0x0000000000000000000000000000000000000000",
	"appliedIoIDs": "1",
	"activedIoIDs": "0",
}
```

**Output Explanation:**

* **projectContarctAddress** represents the NFT token contract address for registered projects (add it to your wallet app to check your project IDs and manage ownership).
* **deviceNFT** represents the Device NFT contract address you have set for your depin project
* **appliedIoIDs**: is the number of ioIDs you have applied, consequently the number of devices that can be activated for your project
* **activatedIoIDs**: is the number of ioIDs activated and assigned to registered devices
