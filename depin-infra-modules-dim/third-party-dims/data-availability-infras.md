# Data Availability Infras

The Data Availability Layer (DAL) is the input layer for W3bstream. It can be either a centralized service or a decentralized network that stores blocks of data for a specified period as determined by a DePIN project. Once this period expires, the data may be immediately deleted or transferred to a dedicated Long-Term Storage Layer. Depending on the project's configuration, the DAL also periodically commits data sets (e.g., a hash) to the IoTeX Blockchain to ensure data integrity.

<figure><img src="../../.gitbook/assets/image (114).png" alt=""><figcaption></figcaption></figure>

## Supported DA Infra

While IoTeX does not provide any Data Availability DIM, W3bstream will support popular Data Availability (DA) infrastructures. We anticipate that third-party projects will develop DIMs to serve as DA layers, integrating seamlessly with W3bstream.

{% hint style="info" %}
**Reference Implementation**

A W3bstream-compatible DA Layer should implement a [simple interface](https://github.com/iotexproject/w3bstream/blob/develop/datasource/datasource.go).

W3bstream provides a [reference implementation](https://github.com/iotexproject/w3bstream/tree/develop/datasource) for a Postgres DB.
{% endhint %}
