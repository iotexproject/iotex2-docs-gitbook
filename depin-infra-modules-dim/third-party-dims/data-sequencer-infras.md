# Data Sequencer Infras

A DePIN Sequencer can function either as a centralized service or as a decentralized network. Its primary role is to **sort data packets from smart devices** and pack them in a Data Availability Network in a format that can be processed by the off-chain computing layer (W3bstream).&#x20;

<figure><img src="../../.gitbook/assets/image (115).png" alt=""><figcaption></figcaption></figure>

A DePIN sequencer is usually responsible for device authentication, data message validation, sorting, filtering, and other tasks depending on project requirements.

While researchers have not yet reached a consensus on the architecture and functions of a DePIN Sequencer, the need for high customizability and programmability is evident. Many emerging decentralized Sequencer networks focus on Ethereum scalability but lack the programmability required by DePIN projects.&#x20;

## Supported Sequencers

While IoTeX does not provide any Data Sequencer DIM, W3bstream will support popular Data Availability (DA) infrastructures. We anticipate that third-party projects will develop DIMs to serve as DePIN Sequencers, integrating seamlessly with W3bstream.

{% hint style="info" %}
**Reference Implementation**

A W3bstream-compatible DA Layer should aggregate one ore multiple device messages into a [W3bstream Task](w3bstream-tasks.md) and store these tasks on a supported DA infra.

W3bstream provides a [reference implementation](https://github.com/iotexproject/w3bstream/tree/develop/cmd/sequencer) for a DePIN Sequencer for W3bstream that features:

* message aggregation
* ioID authentication
* DID-based secure device communication
* Postgres as the destination DA infra.
{% endhint %}
