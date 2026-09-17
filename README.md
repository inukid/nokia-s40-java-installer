# nokia-6230i-midlet-installer-nokia-s40-linux-installer-
The JAR downloaded successfully but failed to install on the Nokia 6230i with a “not enough phone memory to download file” error. Downloading it on Linux, extracting/repackaging it, and transferring it via Bluetooth resolved the issue, suggesting a JAR packaging or web installation problem rather than runtime RAM usage.

# Nokia 6230i (Series 40) Java Game Installer for Linux

A lightweight guide and workflow for installing Java MIDlets (`.jar`/`.jad`) onto the **Nokia 6230i** (and other Nokia Series 40 2nd Edition devices) using Linux via Bluetooth.

---

## The Problem

1. **Native Browser Buffer Limit:** The built-in WAP browser stages downloads in a small RAM cache (~100 KB–256 KB), causing `Out of memory` or `Insufficient memory` errors even with 20 MB+ free storage.
2. **OBEX Push Fallback:** Sending a `.jar` via generic Bluetooth push saves it to the **Gallery**, triggering `File format not supported` because S40 cannot execute raw archives from the file browser.

Using **Gammu** bypasses these bottlenecks by pushing the MIDlet directly to the phone's internal Java application manager.

---

## Hardware Limitations

| Specification | Limit |
|---|---|
| **Platform** | Series 40 2nd Edition |
| **Native Resolution** | 208 × 208 px |
| **Max JAR Size** | 512 KB |
| **Java Profile** | MIDP 2.0 / CLDC 1.1 |

> [!WARNING]
> Keep `.jar` files strictly under **512 KB**. Anything larger will fail to install or launch. Target **208×208** builds to avoid scaling artifacts and runtime out-of-memory errors.

---

## 1. Prerequisites

Install `gammu` and the BlueZ Bluetooth stack:

### Debian / Ubuntu
```bash
sudo apt update
sudo apt install -y gammu bluez

Arch Linux
Bash

sudo pacman -S gammu bluez bluez-utils

Fedora
Bash

sudo dnf install -y gammu bluez

(Note: jadmaker is included with Gammu packages on most distributions).
2. Bluetooth Pairing

    On your Nokia 6230i, navigate to:
    Menu > Settings > Connectivity > Bluetooth

        Turn Bluetooth On.

        Set Phone visibility to Shown to all.

    Start the Bluetooth service:
    Bash

sudo systemctl enable --now bluetooth

Open bluetoothctl to pair and trust the device:
Bash

bluetoothctl

Inside the bluetoothctl prompt:
Plaintext

scan on

Locate your Nokia MAC address (e.g., 00:14:A7:69:9B:63), then run:
Plaintext

    pair 00:14:A7:69:9B:63
    trust 00:14:A7:69:9B:63
    exit

    (Enter matching PINs, such as 1234, on both host and handset when prompted).

3. Configuration

Write the Gammu configuration file to ~/.gammurc (replace with your device's MAC address):
Bash

cat <<EOF> ~/.gammurc
[gammu]
device = 00:14:A7:69:9B:63
connection = bluephonet
EOF

Verify connectivity:
Bash

gammu identify

Expected output: Device model (6230i), firmware revision, and IMEI.

    [!NOTE]
    If bluephonet fails to connect, try editing ~/.gammurc and setting connection = phonetblue or connection = blueobex.

4. Installing Games

Gammu requires both the .jar and an accompanying .jad descriptor file.
Step 4.1: Generate the .jad Descriptor

Navigate to your game directory and generate the JAD file:
Bash

cd ~/Downloads
jadmaker yourgame.jar

Step 4.2: Push to Device

Execute nokiaaddfile using the base name only (omit extensions):

    Install into the Games menu:
    Bash

gammu nokiaaddfile Game yourgame

Install into the Applications / Collection menu:
Bash

gammu nokiaaddfile Application yourgame
