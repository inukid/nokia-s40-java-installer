# nokia-6230i-midlet-installer-nokia-s40-linux-installer-
The JAR downloaded successfully but failed to install on the Nokia 6230i with a “not enough phone memory to download file” error. Downloading it on Linux, extracting/repackaging it, and transferring it via Bluetooth resolved the issue, suggesting a JAR packaging or web installation problem rather than runtime RAM usage.

# Nokia 6230i (Series 40) Java Game Installer for Linux

A lightweight guide and workflow for sideloading Java MIDlets (`.jar` / `.jad`) onto the **Nokia 6230i** (and other Nokia Series 40 2nd Edition devices) using Linux via Bluetooth.

---

## Background & Issues Solved

* **Browser Cache Bottleneck:** Attempting to download games over-the-air using the phone's native WAP browser causes a `Not enough phone memory to download file` or `Insufficient memory` error. The built-in browser stages incoming files in a tiny internal RAM cache (~100 KB–256 KB) before saving, even if the device has 20 MB+ of free local storage.
* **Bluetooth OBEX Misrouting:** Sending a raw `.jar` via generic desktop Bluetooth Object Push routes the archive into the phone's **Gallery** as generic data. Attempting to open it results in `File format not supported` because Series 40 cannot launch executables from the file manager.
* **The Solution:** Using **Gammu** on Linux communicates directly with the phone's Java Application Manager, bypassing browser memory limits and properly registering the MIDlet into the **Games** or **Applications** menu.

---

## Hardware Limitations

| Specification | Limit |
|---|---|
| **Platform** | Series 40 2nd Edition |
| **Native Resolution** | 208 × 208 px |
| **Max JAR Size** | 512 KB |
| **Java Profile** | MIDP 2.0 / CLDC 1.1 |

> [!WARNING]
> Keep `.jar` files strictly under **512 KB**. Files exceeding this limit will fail to install or crash with an `Out of Memory` exception. For optimal display, target builds designed for **208×208** screens.

---

## 1. Prerequisites

Install `gammu` and the BlueZ Bluetooth stack for your Linux distribution:

### Debian / Ubuntu / Linux Mint
```bash
sudo apt update
sudo apt install -y gammu bluez
