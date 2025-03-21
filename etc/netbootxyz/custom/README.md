```markdown
# Skyscope Sentinel Intelligence - Secure Operating System Network Boot

## Overview

This repository is a fork of **netboot.xyz** with a custom iPXE script (`custom.ipxe`) designed for the **Skyscope Sentinel Intelligence - Secure Operating System Network Boot - Install and PC Recovery Utility v1.0**. The script provides a menu-driven interface to install a security-enhanced Arch Linux system with full-disk encryption and YubiKey integration.

The `custom.ipxe` script is located in the `etc/netbootxyz/custom/` directory of this repository.

## Purpose

The `custom.ipxe` script creates a custom iPXE menu for **netboot.xyz**, allowing users to:

- Install a security-enhanced Arch Linux system using the **Skyscope Sentinel Security Enhanced Arch Linux Auto Installer**.
- Automatically download and execute the `install-arch-fde-yubikey.sh` script, which sets up full-disk encryption, YubiKey authentication, and other security features.
- Future versions may include additional options for PC recovery utilities.

## Menu Structure

The menu displayed by `custom.ipxe` is as follows:

```
Skyscope Sentinel Intelligence - Secure Operating System Network Boot - Install and PC Recovery Utility v1.0

Install Options
  Skyscope Sentinel Security Enhanced Arch Linux Auto Installer
```

### Menu Options

**Skyscope Sentinel Security Enhanced Arch Linux Auto Installer:**
- Boots the Arch Linux net install environment in CLI mode.
- Downloads the `install-arch-fde-yubikey.sh` script from the Skyscope Sentinel repository.
- Verifies the script's integrity using a SHA256 hash.
- Executes the script to set up a secure Arch Linux installation with full-disk encryption and YubiKey integration.

## Prerequisites

- A working internet connection for downloading the Arch Linux kernel, initrd, and the installation script.
- A YubiKey device inserted into the system (required by the `install-arch-fde-yubikey.sh` script for authentication setup).
- A **netboot.xyz** USB or network boot environment configured to chainload the `custom.ipxe` script.

## Usage

### Step 1: Boot into Netboot.xyz

1. Create a **netboot.xyz** USB using the official image from [netboot.xyz/downloads](https://netboot.xyz/downloads).
2. Boot your system from the **netboot.xyz** USB.

### Step 2: Chainload the Custom Script

1. Press `Esc` at the **netboot.xyz** menu to access the iPXE debug shell. You’ll see a prompt like:
   ```
   iPXE>
   ```
2. Chainload the `custom.ipxe` script from this repository:
   ```
   chain https://raw.githubusercontent.com/skyscope-sentinel/netboot.xyz/development/etc/netbootxyz/custom/custom.ipxe
   ```

**Alternative: Set as Custom Menu URL**

1. From the **netboot.xyz** menu, navigate to **“Utilities” > “Set custom menu url”**.
2. Enter the URL of the `custom.ipxe` script:
   ```
   https://raw.githubusercontent.com/skyscope-sentinel/netboot.xyz/development/etc/netbootxyz/custom/custom.ipxe
   ```
3. Return to the main menu, select **“Arch Linux”**, and it will chainload the script.

### Step 3: Use the Menu

The custom menu will appear:

```
Skyscope Sentinel Intelligence - Secure Operating System Network Boot - Install and PC Recovery Utility v1.0

Install Options
  Skyscope Sentinel Security Enhanced Arch Linux Auto Installer
```

Use the arrow keys to highlight **“Skyscope Sentinel Security Enhanced Arch Linux Auto Installer”** and press `Enter`.

### Step 4: Installation Process

1. The script will boot the Arch Linux net install environment in CLI mode.
2. It will download the `install-arch-fde-yubikey.sh` script from the Skyscope Sentinel repository.
3. The script’s SHA256 hash will be verified to ensure integrity.
4. If the hash matches, the script will execute, setting up a secure Arch Linux installation with full-disk encryption and YubiKey integration.
5. Follow any prompts provided by the `install-arch-fde-yubikey.sh` script (e.g., YubiKey setup).

## Troubleshooting

### Network Issues

If the script fails to download the kernel, initrd, or installation script, verify your network connectivity in the iPXE debug shell:

```shell
ping 8.8.8.8
```

If the ping fails, configure the network manually:

```shell
ifopen net0
set net0/ip 192.168.1.100
set net0/netmask 255.255.255.0
set net0/gateway 192.168.1.1
set net0/dns 8.8.8.8
```

Adjust the IP settings to match your network.

### Hash Mismatch

If the hash check fails, the `install-arch-fde-yubikey.sh` script may have been modified since the hash was calculated. Recalculate the hash:

```shell
curl -s https://raw.githubusercontent.com/skyscope-sentinel/Arch-Linux-1-Partition-Full-Disk-Encryption-with-YubiKey/main/install-arch-fde-yubikey.sh | sha256sum
```

Update the hash in `custom.ipxe` (search for the `sha256sum` comparison) with the new value.

### Boot Failure

If Arch Linux fails to boot, the kernel or initrd URLs may have changed. Manually boot Arch Linux from the **netboot.xyz** menu and note the URLs displayed in the iPXE output. Update the kernel and initrd URLs in `custom.ipxe` accordingly.

### YubiKey Not Detected

Ensure your YubiKey is inserted before the `install-arch-fde-yubikey.sh` script runs. In the Arch Linux live environment, verify the YubiKey is detected:

```shell
lsusb
```

Look for a device like **“Yubico YubiKey”**. If it’s not detected, try reinserting the YubiKey or using a different USB port.

## Future Enhancements

- Add recovery options to the menu for PC recovery utilities.
- Include additional installer options for other operating systems or configurations.

## License

This project is a fork of **netboot.xyz** and is licensed under the same terms (Apache License 2.0). See the [LICENSE](https://github.com/netbootxyz/netboot.xyz/blob/master/LICENSE) file in the original repository for details.

## Contact

For issues or suggestions, please open an issue in this repository or contact the **Skyscope Sentinel** team.
