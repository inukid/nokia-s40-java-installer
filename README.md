# Nokia 6230i Java Game Installer for Linux

A practical guide for installing Java MIDlets (`.jar` and `.jad` files) on the Nokia 6230i and other Nokia Series 40 2nd Edition phones from Linux over Bluetooth.

## Background

The Nokia 6230i has several constraints that make downloading games through its built-in WAP browser unreliable:

- The browser uses a small temporary download cache, so larger files may fail with `Not enough phone memory to download file` even when the phone has free storage.
- Sending a JAR with generic Bluetooth Object Push usually places it in the Gallery as a data file. Series 40 cannot launch a MIDlet directly from the file manager.
- Gammu can communicate with the phone's Java application manager and install the MIDlet into the Games or Applications menu.

## Device limitations

| Specification | Limit |
| --- | --- |
| Platform | Series 40 2nd Edition |
| Native resolution | 208 x 208 px |
| Maximum JAR size | 512 KB |
| Java profile | MIDP 2.0 / CLDC 1.1 |

> [!WARNING]
> Keep JAR files below 512 KB. Larger files may fail to install or cause an `Out of Memory` error. Games designed for a 208 x 208 display will generally provide the best results.

## 1. Prerequisites

Install Gammu and the BlueZ Bluetooth tools for your distribution.

### Debian, Ubuntu, or Linux Mint

```bash
sudo apt update
sudo apt install -y gammu bluez bluez-tools
```

### Arch Linux or Manjaro

```bash
sudo pacman -S gammu bluez bluez-utils
```

### Fedora or RHEL

```bash
sudo dnf install -y gammu bluez
```

You may also need a tool that generates JAD descriptors, such as `jadmaker`. Check whether it is available on your distribution before continuing:

```bash
command -v jadmaker
```

## 2. Pair the phone over Bluetooth

1. On the phone, open **Menu > Settings > Connectivity > Bluetooth**.
2. Turn Bluetooth on.
3. Set **Phone visibility** to **Shown to all**.
4. Start the Bluetooth service on Linux:

   ```bash
   sudo systemctl enable --now bluetooth
   ```

5. Start the Bluetooth control shell:

   ```bash
   bluetoothctl
   ```

6. At the `bluetoothctl` prompt, scan for the phone:

   ```text
   power on
   agent on
   default-agent
   scan on
   ```

7. Note the Nokia's Bluetooth MAC address, then pair and trust it. Replace the example address with the address found during scanning:

   ```text
   pair 00:14:A7:69:9B:63
   trust 00:14:A7:69:9B:63
   quit
   ```

If the phone asks for a pairing code, confirm the same code on both devices.

## 3. Configure Gammu

Create `~/.gammurc` and replace the example MAC address with your phone's address:

```bash
cat > ~/.gammurc <<'EOF'
[gammu]
device = 00:14:A7:69:9B:63
connection = bluephonet
EOF
```

Test the connection:

```bash
gammu identify
```

The output should identify the handset as Nokia. The exact firmware, IMEI, and hardware values will differ between phones.

If `bluephonet` does not connect, consult the Gammu documentation for the connection type supported by your installed version. Some older configurations use `phonetblue` or `blueobex`.

## 4. Prepare the MIDlet

A Series 40 installation normally needs both the executable JAR and its Java application descriptor, the JAD file.

Change to the directory containing the JAR and generate a descriptor:

```bash
cd ~/Downloads
jadmaker yourgame.jar
```

Confirm that both files exist:

```bash
ls -lh yourgame.jar yourgame.jad
```

If the game already includes a compatible JAD file, use that file instead of generating a new one.

## 5. Install the game

Install the game in the handset's **Games** menu:

```bash
gammu nokiaaddfile Game yourgame
```

To install it in the **Applications** or **Collection** menu instead:

```bash
gammu nokiaaddfile Application yourgame
```

Gammu should report progress while writing the JAD and JAR files. When it finishes, open the selected menu on the phone and launch the game.

> [!NOTE]
> Pass the filename without the `.jar` or `.jad` extension to `nokiaaddfile`; Gammu uses the matching JAR and JAD files in the current directory.

## Troubleshooting

### `Not enough phone memory to download file`

Install the game over Bluetooth with Gammu instead of downloading it through the phone's WAP browser. Also check the JAR size and delete unused files from the phone.

### `File format not supported`

The JAR was probably copied to the Gallery as a generic file. Remove it if necessary and install it through `gammu nokiaaddfile` so the Java application manager registers it correctly.

### Gammu cannot connect

- Confirm that Bluetooth is enabled and the phone is paired and trusted.
- Check the MAC address in `~/.gammurc`.
- Make sure the Bluetooth service is running.
- Keep the phone close to the computer during the transfer.
- Try a connection type supported by your Gammu version, such as `phonetblue`.

### Installation fails or the game crashes

- Verify that the JAR is below the phone's size limit.
- Confirm that the MIDlet targets MIDP 2.0 and CLDC 1.1.
- Prefer a build designed for a 208 x 208 screen.
- Make sure the JAD's `MIDlet-Jar-Size` value matches the actual JAR size.

## Useful references

- [Gammu documentation](https://docs.gammu.org/)
- [BlueZ project](https://www.bluez.org/)
