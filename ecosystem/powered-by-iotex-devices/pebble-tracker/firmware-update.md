# Online Firmware Update

When your Pebble Tracker is powered on, **check the software version** displayed on the screen:

{% hint style="warning" %}
If your device shows a software version  **`v1.0.x`**, please follow the migration guide to upgrade to **`v2.0`** first → [migrating-to-pebble-v2.0](migrating-to-pebble-v2.0/ "mention")&#x20;
{% endhint %}

{% hint style="success" %}
### About the new v2.0 software version

The new Pebble Tracker software `v2.0` introduces support for **ioID**, IoTeX’s innovative decentralized identity framework designed specifically for DePINs.&#x20;

This upgrade transitions each Pebble device identity, already registered on the IoTeX blockchain, into the new ioID system, offering greater composability and standardization. These enhancements enable seamless integration with DeFi and DePIN dApps on the IoTeX L1, unlocking broader possibilities for Pebble Tracker users.

[-> Read more about ioID in this blog post](https://iotex.io/blog/ioid-on-chain-device-identity-for-verifiable-depins/)
{% endhint %}

### Start the OTA Firmware Update

1. Make sure your **SIM card** is correctly installed in the SIM card slot of the device

<figure><img src="../../../.gitbook/assets/image (1) (1) (1) (1) (1).png" alt="" width="563"><figcaption></figcaption></figure>

2. If your device is turned on, **turn it off** by pressing the `Power/Confirm` button and keeping it pressed until the status LED flashes green:

<figure><img src="../../../.gitbook/assets/image (4) (1) (1).png" alt="" width="563"><figcaption></figcaption></figure>

2. Press and keep pressing the `Up Arrow` button, then press and release the `Power/Confirm` button (while keeping the Up Arrow still pressed):

<figure><img src="../../../.gitbook/assets/image (6).png" alt="" width="563"><figcaption></figcaption></figure>

2. Release the up arrow once the device is powered on and you see the message “_**OTA Firmware Update. Connecting...**_” message on the display
3. Wait for the device to connect to the cellular network and the message “_P**lease choose an app from the portal**_” is displayed
4. Open [portal.machinefi.com](https://portal.machinefi.com/) in a Metamask compatible Browser, or directly inside [ioPay mobile](https://iopay.me/), and connect to the portal with your MachineFi blockchain account (you can create one by following the [get started guide](https://app.gitbook.com/s/f2s3zCHPO4kfjqwDZ9Gw/get-started/quick-start#creating-a-machinefi-account))
5. Select the “**App Store**” tab, then locate and open the “**Firmware Upgrade**” app and click the “**Install**” button
6. In the following dialog, select the device you want to upgrade (see note → ✳️ below)
7. Make sure your Pebble is still showing the “_**Please, choose an app on the portal**_” message on the display, and click the `Next` button on the portal
8. Go ahead and click `Next` on the second dialog to trigger the firmware upgrade request transaction for your device
9. Confirm the transaction in your Metamask wallet
10. Wait for the Pebble tracker to detect the request and start downloading the firmware

{% hint style="info" %}
-> ✳️ If you own multiple devices, you can identify your devices from the “**Devices**” tab on the MachineFi portal by matching the IMEI number with the one printed on the Pebble box, and the serial number printed on the box with the one printed on the back of the device
{% endhint %}

{% hint style="info" %}
The download and installation of the firmware may require some time. In some cases, the download can fail due to a network error: this is usually caused by a low cellular signal in your area, especially if your SIM card connects using the NB-IoT band. In this case, you can try again, or just try the offline firmware update method detailed below.
{% endhint %}

