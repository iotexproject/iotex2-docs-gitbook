# API Overview

The **ioID API** allows projects and developers to programmatically register, bind and manage ioID identities for machines. These endpoints support the full lifecycle of an ioID, from generation to ownership binding and data submission.

Use the API to:

* Create an ioID for a new device using a unique serial or hardware identifier
* Bind a device’s ioID on-chain identity to an owner’s wallet
* Upload verifiable data and activity metrics for indexing and analytics on depinscan.io

All API requests require authentication with your project’s API key (available after registration on [depinscan.io](https://depinscan.io)).

### **Generate an ioID for a device**

```
POST /api/v1/ioid/generate
```

```json
{
  "projectName": "SmartHome",
  "deviceIdentifier": "SN123456"
}
```

### **Bind an ioID to the owner's wallet**

```
POST /api/v1/ioid/owner/bind
```

```json
{
  "projectName": "SmartHome",
  "deviceSerial": "SN123456",
  "chainid": 1,
  "tokenContract": "0xabc",
  "tokenid": 123
}
```

### **Upload device metrics**

```
POST /api/upload-device-metrics
```

```json
{
  "uid": "device-id",
  "events": [{
    "publisher": "sensor-a",
    "timestamp": "2024-06-01T00:00:00Z",
    "custom": { "longitude": 1, "latitude": 1 }
  }]
}
```
