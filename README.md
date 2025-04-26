# Custom SysRescue ISO with Autorun

This repository contains a customized [SystemRescue](https://www.system-rescue.org/) ISO setup with autorun functionality and custom YAML configuration for automated networking and system diagnostics.

---

## 📁 Repository Contents

- `sysrescue_custom.iso`  
  > The customized SystemRescue ISO (tracked with Git LFS).

- `.gitattributes`  
  > Configuration to track large files (like `.iso`) using Git LFS.

- `sample-dir/`  
  > Contains files used for autorun and system customization.

---

## 🔧 sample-dir Details

### `autorun/autorun0`

A bash script executed automatically at boot via SystemRescue's autorun feature.

#### Script Actions:
- Logs start time and IP configuration
- Runs `ipmitool lan print 1` to gather IPMI LAN settings
- Saves all output to `/var/log/autorun0.log`

```bash
#!/bin/bash
echo "=== Starting autorun0 ===" > /var/log/autorun0.log
date >> /var/log/autorun0.log
ip a >> /var/log/autorun0.log
ipmitool lan print 1 >> /var/log/autorun0.log 2>&1
echo "=== Done ===" >> /var/log/autorun0.log





🚀 How It Works
Boot using sysrescue_custom.iso

Autorun picks up 100-default.yaml and autorun0

System comes up with DHCP networking

autorun0 script logs diagnostic info to /var/log/autorun0.log

📌 Notes
Ensure ipmitool is available in the ISO environment.

Customize autorun0 to include additional diagnostics as needed.

Use copytoram: true in 100-default.yaml if you want to run from RAM.



✅ 1. How to Rebuild the ISO
If you're customizing the ISO manually, add instructions like:


```bash
mkisofs -o sysrescue_custom.iso -b isolinux/isolinux.bin -c isolinux/boot.cat \
  -no-emul-boot -boot-load-size 4 -boot-info-table -R -J -v -T ./iso-root/


Replace ./iso-root/ with the path where your modified ISO files are stored
---

### ✅ **2. How to Test with QEMU**

If you're using QEMU/KVM for testing (as I mentioned before, then note that ipmi-tools needs a hardware env to run):

```markdown
## 🧪 Testing in QEMU

You can test the ISO locally:

```bash
qemu-system-x86_64 -boot d -cdrom sysrescue_custom.iso -m 1024


## 🤝 Contributions

Feel free to fork this repo and adapt the autorun logic for your own automation or diagnostic workflows.
