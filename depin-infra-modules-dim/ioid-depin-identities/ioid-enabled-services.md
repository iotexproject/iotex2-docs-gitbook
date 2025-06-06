# ioID Enabled Services

## Reward Distribution Service

Rewards are fully computed and distributed on the blockchain, based on physical work data provided by devices through the project’s data computation layer. This model ensures full transparency in the sense that the reward algorithm and the entire history of reward calculations and distributions are permanently and publicly accessible on-chain. With ioID in place, the reward distribution service can be realized as shown in the figure below.

<figure><img src="../../.gitbook/assets/image (1).png" alt=""><figcaption></figcaption></figure>

## Proof-of-Liveness (PoL) Service

A complete, immutable on-chain log that records connection and disconnection events for a specific device. This enables public verification of a device’s operational availability over time and can compose with the rewards distribution service (i.e. Provide a default rewards distribution policy based on device liveness). Works similar to the rewards distribution service, but the DePIN project uploads a payload of connection/disconnection records for each device periodically to the liveness registry contract. Other on-chain services can tap into the liveness contract to know if a certain device is currently online or what % of time it was online in a time range.

## Staking Service

A ready-to-use staking contract tailored for DePIN projects, fully compatible with ioID. DePIN project participants who hold an ioID can stake the project’s native token and receive interest according to a set of staking rules configurable by the DePIN project. These include flexible options such as daily reward pool sharing, lock periods, and withdrawal grace periods. DePIN projects can leverage this staking service out of the box — no need to build their own contract. The only requirements: a valid project ID on ioID and users with registered ioID devices. The staking service can be implemented as shown in the figure below.

<figure><img src="../../.gitbook/assets/image (1) (1).png" alt=""><figcaption></figcaption></figure>

## Financing Service

These could be pre-set configurable contracts that DePIN projects can decide to use, all of them integrated with ioID.

*   **Device Bonding**

    Users or investors can bond tokens in exchange for the right to operate or own a specific device within the DePIN network — e.g., _stake 1,000 tokens to activate a weather station node for 12 months_.
*   **Pre-Staking**

    A project may offer token incentives to users who commit capital early to help fund device rollouts. For instance, a vault contract where a participant can pre-stake the DePIN Project's tokens to receive priority access once the network goes live. The funds are used to financialise the initial deployment of new edge nodes.
*   **Revenue-Backed Financing**

    Instead of buying a device, users can fund specific devices (or operators) in exchange for a share of future on-chain revenue generated by that device
