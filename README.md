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
