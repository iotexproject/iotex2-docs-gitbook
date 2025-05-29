# Troubleshooting

## Access the Device Info Menu

Opening the **Info Menu** on your Pebble Tracker will give you access to the IMEI and Serial Number, as well as allow you to obtain the device’s public key using the Pebble Desktop tool, if needed.

{% embed url="https://youtu.be/c5uX2ePeQvA" %}

## Retrieve the Device Public Key

### 1. Install PebbleUSBTools

Make sure you installed the required driver and PebbleUSBTool by following the section "**Install and run Pebble USB tools" in** [#offline-firmware-upgrade](firmware-update.md#offline-firmware-upgrade "mention")

### 2. Access the Info menu in your Pebble Tracker&#x20;

Check out the video in the previous section for a visual guidance:

1. Power off the device
2. Press and keep pressing the arrow down
3. Press the power button once
4. Use the arrow keys to select the "About Pebble" and the power button to confirm

### 3. Retrieve the Public Key

Inside the Pebble USB tool, wait for the status bar to show the "_Pebble Connected"_ message and then click "**Fetch PubKey**"

<figure><img src="../../../.gitbook/assets/image (1) (1) (1) (1) (1) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>
