# 🚀 MT7902 WiFi Driver Installation for Linux

This repo provides **simple** and **copy-paste ready** instructions for installing community drivers for the **MediaTek MT7902 Wireless Card** to enable WiFi connectivity on some **Linux systems**. Currently, neither **MediaTek** nor the **Linux Kernel team** provides official support for MT7902 drivers. Since MediaTek has not released the source code or upstreamed support to the Linux kernel, users must either install community drivers or look for hardware alternatives.

# Motivation

I currently own an **Acer Aspire 3 15** laptop which comes with the **MT7902 Wireless Card (WC)**, and **it has been a frustrating process trying to make things work**. My journey went like this:

- I had **Windows 11** and realized it is impossible to have **WSL2**, **VS Code**, and **Chrome** open simultaneously on 8GB of RAM ☠️. I even tried downgrading to Windows 10, since it isnt as heavy and bloated as Windows 11 (especially the React UI of the system plus the telemetry and general system bloat). Win10's performance slighly increased, but it was still nothing near being enough for my development needs.
- I installed **Fedora Workstation 43**. Then I realized: "Wow. There is no WiFi 😐."
- After extensive searching, I found that newer kernels (6.13+) and experimental distros like **Fedora 43** have removed the legacy "Wireless Extensions" (WEXT) that this driver needs to compile.
- I installed **Arch Linux** and finally got it working using [hmtheboy154's repository](https://github.com/hmtheboy154/gen4-mt7902.git).
- I finally have WiFi without a dongle, but I've documented exactly which OSs will work and which will fail to save you the headache. I also tried the script on some Distros for accuracy (and just testing out their differences). Specifically, I have tested on: Ubuntu, Pop!\_OS, Fedora, Zorin, Linux Mint, and Arch Linux. See below for which worked:

# 🚦 Compatibility Matrix

| OS Status    | Distributions                                                                                                                                    | Why?                                                                                                 |
| :----------- | :----------------------------------------------------------------------------------------------------------------------------------------------- | :--------------------------------------------------------------------------------------------------- |
| **✅ WORKS** | **Arch Linux**, **Zorin OS 17.x**, **Pop!\_OS 22.04**                                                                                            | These distros use kernels that still include the `WEXT` API needed by the driver.                    |
| **❌ FAILS** | **Fedora**, **Ubuntu**, **Zorin OS 18**, and probably all other distros without the Legacy WEXT api (appearing on kernel versions 6.13 or lower) | These versions have stripped the legacy wireless code from the kernel. The driver will fail to link. |

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
<summary><b>🟣 Zorin OS | Pop!_OS (APT)</b></summary>

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

---

## ⚠️ Notes & Troubleshooting

- **Secure Boot:** You **MUST DISABLE SECURE BOOT** in your BIOS. Zorin (and probably other OS'es ) will block the driver from loading if Secure Boot is on because the driver is not digitally signed.
- In case you aren't aware of, at least as of now Bluetooth support for the MT7902 on linux is almost NULL. There are currently community efforts around it ([repo here](https://github.com/OnlineLearningTutorials/mt7902_temp)) which didn't work on my arch distro. So you should know you probably won't have bluetooth with the MT7902 on linux. This repo is also working on a WiFi community driver, so you may want to keep an eye on it every now and then.

## 🗑 Cleanup

To remove the driver entirely from any of the supported systems:

```bash
sudo modprobe -r mt7902 && sudo rm /etc/modules-load.d/mt7902.conf && sudo rm -rf /lib/firmware/mediatek/mt7902 && sudo rm /usr/lib/modules/$(uname -r)/kernel/drivers/net/wireless/mediatek/mt7902.ko && sudo depmod -a
```

## 🚨 Kernel Disclaimer

If you update your system and WiFi disappears, it means your kernel updated. You **must** re-run the installation script for your specific distro. If your distro updates to a kernel version that removes `wireless_send_event` (like Fedora 43), this driver will stop working entirely until a community patch is released.

## Final Words

If this helped, leave a **star**! Keep this repo handy—it’s your best friend until you finally swap that card or get a dongle! 🫡.

##Edit 2: The script has been working perfectly for me (on archlinux) this whole time, and every now and then after updating the system (`sudo pacman -Syu`) i lose WiFi connectivity, share connection from my phone via USB, re-run it (taking about 2 minutes) and WiFi is ready 🤓.
