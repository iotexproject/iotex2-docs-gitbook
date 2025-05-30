# API Overview

The **ioID API** allows projects and developers to programmatically register, bind and manage ioID identities for machines. These endpoints support the full lifecycle of an ioID, from generation to ownership binding and data submission.

Use the API to:

* Create an ioID for a new device using a unique serial or hardware identifier
* Bind a device’s ioID on-chain identity to an owner’s wallet
* Upload verifiable data and activity metrics for indexing and analytics on depinscan.io

All API requests require authentication with your project’s API key (available after registration on [depinscan.io](https://depinscan.io)).

### **Generate an ioID for a device**

