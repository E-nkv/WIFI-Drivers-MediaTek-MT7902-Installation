# 🚀 MT7902 WiFi Driver Installation for Linux

This repo provides **simple** and **copy-paste ready** instructions for installing community drivers for the **MediaTek MT7902 Wireless Card** to enable WiFi connectivity on **Linux systems**. Currently, neither **MediaTek** nor the **Linux Kernel team** provides official support for MT7902 drivers. Since MediaTek has not released the source code or upstreamed support to the Linux kernel, users must either install community drivers or look for hardware alternatives.

# Motivation
I currently own an **Acer Aspire 3 15** laptop which comes with the **MT7902 Wireless Card (WC)**, and **it has been a frustrating process trying to make things work**. My journey went like this: 

- I had **Windows 11** and realized it is impossible to have **WSL2**, **VS Code**, and **Chrome** open simultaneously on 8GB of RAM ☠️.
- I installed **Fedora Workstation (Rawhide/43)**. Then I realized: "Wow. There is no WiFi 😐."
- After extensive searching, I found that newer kernels (6.13+) and experimental distros like **Fedora 43** have removed the legacy "Wireless Extensions" (WEXT) that this driver needs to compile.
- I installed **Arch Linux** and finally got it working using [hmtheboy154's repository](https://github.com/hmtheboy154/gen4-mt7902.git).
- I finally have WiFi without a dongle, but I've documented exactly which OSs will work and which will fail to save you the headache.

# 🚦 Compatibility Matrix

| OS Status | Distributions | Why? |
| :--- | :--- | :--- |
| **✅ WORKS** | **Arch Linux**, **Zorin OS 17.x**, **Linux Mint 22.x**, **Pop!_OS 22.04** | These distros use kernels that still include the `WEXT` API needed by the driver. |
| **⚠️ PARTIAL** | **Fedora 41 (Stable)** | Works, but requires **Secure Boot** to be disabled and may require manual tweaks. |
| **❌ FAILS** | **Fedora 42/43 (Rawhide)**, **Ubuntu (Non-LTS)** | These versions have stripped the legacy wireless code from the kernel. The driver will fail to link. |

# Alternatives
1. **Buy a WiFi Dongle:** Make sure it is **Linux-compatible**. 
2. **Replace your MT7902 WC:** Swap it for an **Intel card (AX200/AX210)**. This is the best long-term fix. Cost: **$18 to $30 USD**.

---

## 🛠 Installation

<details>
<summary><b>🟦 Arch Linux (Pacman)</b></summary>

### YOLO One-Liner (Copy & Paste)
```bash
sudo pacman -S --noconfirm base-devel linux-headers git && git clone https://github.com/hmtheboy154/gen4-mt7902.git && cd gen4-mt7902 && sudo mkdir -p /lib/firmware/mediatek/mt7902 && sudo cp firmware/* /lib/firmware/mediatek/mt7902/ && make -f Makefile.x86 KVER=$(uname -r) LINUX_VER=$(uname -r) KERNELRELEASE=$(uname -r) && sudo mkdir -p /usr/lib/modules/$(uname -r)/kernel/drivers/net/wireless/mediatek/ && sudo cp mt7902.ko /usr/lib/modules/$(uname -r)/kernel/drivers/net/wireless/mediatek/ && sudo depmod -a && sudo modprobe mt7902 && echo "mt7902" | sudo tee /etc/modules-load.d/mt7902.conf && cd .. && rm -rf gen4-mt7902
```
</details>

<details>
<summary><b>🟣 Zorin OS / Linux Mint / Pop!_OS (APT)</b></summary>

### YOLO One-Liner (Copy & Paste)
```bash
sudo apt update && sudo apt install -y build-essential linux-headers-$(uname -r) git && git clone https://github.com/hmtheboy154/gen4-mt7902.git && cd gen4-mt7902 && sudo mkdir -p /lib/firmware/mediatek/mt7902 && sudo cp firmware/* /lib/firmware/mediatek/mt7902/ && make -f Makefile.x86 KVER=$(uname -r) && sudo mkdir -p /usr/lib/modules/$(uname -r)/kernel/drivers/net/wireless/mediatek/ && sudo cp mt7902.ko /usr/lib/modules/$(uname -r)/kernel/drivers/net/wireless/mediatek/ && sudo depmod -a && sudo modprobe mt7902 && echo "mt7902" | sudo tee /etc/modules-load.d/mt7902.conf && cd .. && rm -rf gen4-mt7902
```

### Step-by-Step Installation
```bash
# 1. Install dependencies
sudo apt update && sudo apt install -y build-essential linux-headers-$(uname -r) git

# 2. Clone and Setup Firmware
git clone https://github.com/hmtheboy154/gen4-mt7902.git
cd gen4-mt7902
sudo mkdir -p /lib/firmware/mediatek/mt7902 && sudo cp firmware/* /lib/firmware/mediatek/mt7902/

# 3. Build & Install
make -f Makefile.x86 KVER=$(uname -r)
sudo mkdir -p /usr/lib/modules/$(uname -r)/kernel/drivers/net/wireless/mediatek/
sudo cp mt7902.ko /usr/lib/modules/$(uname -r)/kernel/drivers/net/wireless/mediatek/

# 4. Load
sudo depmod -a && sudo modprobe mt7902
echo "mt7902" | sudo tee /etc/modules-load.d/mt7902.conf
```
</details>

<details>
<summary><b>🟦 Fedora 41 ONLY (DNF)</b></summary>

> **Note:** Do NOT use Fedora 42 or 43. This driver is currently incompatible with those kernels. Ensure **Secure Boot** is OFF.

### YOLO One-Liner (Copy & Paste)
```bash
sudo dnf install -y kernel-devel kernel-headers git gcc make elfutils-libelf-devel && git clone https://github.com/hmtheboy154/gen4-mt7902.git && cd gen4-mt7902 && sudo mkdir -p /lib/firmware/mediatek/mt7902 && sudo cp firmware/* /lib/firmware/mediatek/mt7902/ && make -f Makefile.x86 KVER=$(uname -r) && sudo mkdir -p /usr/lib/modules/$(uname -r)/kernel/drivers/net/wireless/mediatek/ && sudo cp mt7902.ko /usr/lib/modules/$(uname -r)/kernel/drivers/net/wireless/mediatek/ && sudo depmod -a && sudo modprobe mt7902 && echo "mt7902" | sudo tee /etc/modules-load.d/mt7902.conf && cd .. && rm -rf gen4-mt7902
```
</details>

---

## ⚠️ Notes & Troubleshooting
- **Secure Boot:** You **MUST DISABLE SECURE BOOT** in your BIOS. Fedora and Zorin will block the driver from loading if Secure Boot is on because the driver is not digitally signed.
- **Wired Headphones:** If you are on Arch and your headphones don't work, install the firmware: `sudo pacman -S sof-firmware alsa-ucm-conf`. On Mint/Zorin/Pop, this works out of the box.
- **Bluetooth:** This driver primarily enables WiFi. Bluetooth on the MT7902 is extremely unstable on Linux and may not work even with the driver installed.

## 🗑 Cleanup
To remove the driver entirely from any of the supported systems:
```bash
sudo modprobe -r mt7902 && sudo rm /etc/modules-load.d/mt7902.conf && sudo rm -rf /lib/firmware/mediatek/mt7902 && sudo rm /usr/lib/modules/$(uname -r)/kernel/drivers/net/wireless/mediatek/mt7902.ko && sudo depmod -a
```

## 🚨 Kernel Disclaimer
If you update your system and WiFi disappears, it means your kernel updated. You **must** re-run the installation script for your specific distro. If your distro updates to a kernel version that removes `wireless_send_event` (like Fedora 43), this driver will stop working entirely until a community patch is released.

## Final Words
If this helped, leave a **star**! Keep this repo handy—it’s your best friend until you finally swap that card for an **Intel AX210**! 🫡