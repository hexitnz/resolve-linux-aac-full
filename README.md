# DaVinci Resolve Linux AAC Fix (Decoder & Encoder)

> ### ⚠️ DEPRECATED: Please use this repository for the AAC fix
> **Note:** I no longer recommend using my older, standalone encoder plugin. This repository provides the complete, modern fix for both importing (decoding) and exporting (encoding) AAC audio on DaVinci Resolve for Linux. 

## What is this?
DaVinci Resolve Studio on Linux has historically lacked native AAC support. This repository provides pre-compiled plugins that fix this:
1. **AAC Decoder (Import):** Allows you to import MP4/MOV/MKV files with AAC audio, generate waveforms, and play them back.
2. **AAC Encoder (Export):** Allows you to export AAC audio (FDK-AAC) directly from the Deliver page.

This is a compiled build based on the `resolve-21.1.1` branch from the [AquaroTorres/dvcp-aac](https://github.com/AquaroTorres/dvcp-aac) project. 

## Compatibility
* **Important Requirement:** This plugin requires **DaVinci Resolve Studio** (the paid version). It will **not work** on the free version of DaVinci Resolve, as the free version does not support loading custom IOPlugins.
* **DaVinci Resolve:** Tested and working on **Resolve Studio 21.2.6** (Build 6). It should also work on 21.1.1. *Because these plugins hook into Resolve's memory, they are highly version-specific. Future Resolve updates may require new signatures.*
* **Linux Distributions:** These binaries were compiled inside a **Rocky Linux 8** container. This means they require `GLIBC 2.28` or newer. They will work on almost all modern Linux distributions, including:
  * Linux Mint 20+ / Ubuntu 20.04+
  * Debian 10+
  * Fedora 29+
  * Arch Linux, openSUSE, etc.
* **Updates:** I will try and keep this updated as new Resolve versions are released and tested but no promises!
* **Tested Environment for import export MP4 H264/AAC:**
  * **OS:** Linux Mint 22.2, Kubuntu 26.04 LTS
  * **Resolve:** DaVinci Resolve Studio 21.2.6 (Linux)
  * **Players:** VLC 3.0.20, Haruna media player KDE.

## Installation

You can install this using the automated script (recommended) or manually copying the files. 

*Note: Both methods require root permissions (`sudo`) because you are writing to the `/opt/resolve` system directory.*

### Method 1: Automated Script (Recommended)
1. Download the `.zip` file from the **Releases** section and extract it.
2. Open a terminal in the extracted folder.
3. **Fully quit DaVinci Resolve.**
4. Run the installer:
   ```bash
   sudo python3 install.py install --root /opt/resolve
   ```
   *(This script will automatically back up your old plugins, verify the signatures match your Resolve version, and safely install the new ones).*
5. Relaunch Resolve.

### Method 2: Manual Installation
1. Download the `.zip` file from the **Releases** section and extract it.
2. **Fully quit DaVinci Resolve.**
3. Copy the two extracted `.dvcp.bundle` folders into Resolve's plugin directory:
   ```bash
   sudo cp -r aac_decoder_probe.dvcp.bundle /opt/resolve/IOPlugins/
   sudo cp -r aac_fdk_plugin.dvcp.bundle /opt/resolve/IOPlugins/
   ```
4. Relaunch Resolve.

## How to verify it is working
If you want to confirm the decoder is successfully hooked into Resolve, launch Resolve from the terminal with the diagnostic flag:
```bash
RESOLVE_AAC_LOG=1 /opt/resolve/bin/resolve
```
Look at the terminal output as Resolve starts. You should see:
`AAC bridge active (memory only); application files unchanged.`

If you see `AAC bridge inactive`, it means your specific Resolve build has different binary signatures and the plugin failed safely (it will not crash Resolve, it just won't work). 

## Uninstalling / Rolling Back
If you used the automated script, you can safely undo the installation at any time (remember to use `sudo`):
```bash
sudo python3 install.py uninstall --root /opt/resolve
```
This will remove the new plugins and automatically restore any previous plugins you had installed.

## Credits & Acknowledgments
This project would not be possible without the incredible work of the open-source community:
* **[GloriousEggroll](https://github.com/GloriousEggroll/dvcp-aac)** - For creating the original `dvcp-aac` project, the in-memory bridge, and the FDK-AAC encoder plugin.
* **[AquaroTorres](https://github.com/AquaroTorres/dvcp-aac)** - For creating the `resolve-21.1.1` branch, which updated the binary signatures and made the bridge work on Resolve 21.1 and 21.2. 
