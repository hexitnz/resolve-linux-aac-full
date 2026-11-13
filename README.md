# DaVinci Resolve Linux AAC Fix (21.1.1 / 21.2.0)

This repository provides pre-compiled AAC decoder and encoder plugins for **DaVinci Resolve Studio on Linux x86-64**. 

If Resolve is failing to import AAC audio, or if you want to export using FDK-AAC, these plugins fix the issue. 

This is a compiled build based on the `resolve-21.1.1` branch from the [AquaroTorres/dvcp-aac](https://github.com/AquaroTorres/dvcp-aac) project, which I have personally tested and verified to work perfectly on **Resolve Studio 21.2.0.0006**.

### Installation Instructions

1. **Fully quit DaVinci Resolve.**
2. Download the two `.dvcp.bundle` folders from this repository.
3. Copy them into Resolve's plugin directory (you will likely need root/sudo permissions for this):
   ```bash
   sudo cp -r aac_decoder_probe.dvcp.bundle /opt/resolve/IOPlugins/
   sudo cp -r aac_fdk_plugin.dvcp.bundle /opt/resolve/IOPlugins/

4.Relaunch Resolve.

5. You can verify the decoder is working by running Resolve from the terminal with RESOLVE_AAC_LOG=1 resolve. If it says AAC bridge active (memory only), it is working!

Note: These plugins hook into Resolve's memory and do not modify your actual Resolve application files. Because they rely on matching specific machine code, they are version-specific. They are tested on 21.1.1 and 21.2.0.0006. If you upgrade Resolve in the future, they may stop working until the signatures are updated.
