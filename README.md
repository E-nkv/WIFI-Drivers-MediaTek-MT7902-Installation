# 🚀 MT7902 WiFi Driver Installation for Linux

This repo provides **simple** and **copy-paste ready** instructions for installing community drivers for the **MediaTek MT7902 Wireless Card** to enable WiFi connectivity on **Linux systems**. Currently, neither **MediaTek** nor the **Linux Kernel team** provides official support for MT7902 drivers. Since MediaTek has not released the source code or upstreamed support to the Linux kernel, users must either install community drivers or look for hardware alternatives.

# Motivation
I currently own an **Acer Aspire 3 15** laptop which comes with the **MT7902 Wireless Card (WC)**, and **it has been a frustrating process trying to make things work**. My journey went like this: 

- I had **Windows 11** and **8GB of RAM**. I realized it is impossible to have **WSL2**, **VS Code**, and a few **Chrome** tabs open simultaneously ☠️, let alone using **Docker Desktop** or a database editor (**DBeaver** or **Beekeeper Studio**), which are a **MUST** in my dev workflow. 
- I installed **Fedora Workstation**. Then I realized: "Wow. There is no WiFi 😐."
- I searched extensively for workarounds on **GitHub**; none worked, so I bought a **USB dongle**.
- The dongle broke. Not wanting to buy another, I went back to **Windows** (spoiler: **DON'T** ☠️).
- I remembered why I switched from Windows: **1-minute boot times**, a **bloated system**, and **laptop freezes** when using just VS Code and Chrome. 
- I installed **Arch Linux** (thought it would be fun, and hell yeah it was 😋), with the mindset of using my **phone's WiFi via USB** until I bought another dongle. Spoiler - **your laptop's battery won't like this.**
- I finally came across [hmtheboy154's repository **gen4-mt7902** ](https://github.com/hmtheboy154/gen4-mt7902.git), tweaked a bit the installation and compiled the drivers, and I **FINALLY HAVE WIFI** without a dongle 🙂‍↕️.

# Alternatives
1. **Use Windows:** But **PLEASE DON'T** 👺👺.
2. **Buy a WiFi Dongle:** Make sure it is **Linux-compatible**. Use [this GitHub repo guide](https://github.com/morrownr/USB-WiFi/blob/main/home/USB_WiFi_Adapters_that_are_supported_with_Linux_in-kernel_drivers.md) and double-check before buying. (I bought a **FENVI adapter** on AliExpress for **60 cents** due to a discount; the normal price is around **$6 USD**).
3. **Replace your MT7902 WC:** Swap it for an **Intel card (AX200/AX210)**. These offer superior **out-of-the-box Linux driver support**, assuming the card is not currently **soldered** to your motherboard. I intend to do this in the future. Cost: **$18 to $30 USD**.

---

## 🛠 Installation

<details>
<summary><b>🟦 Arch Linux (Pacman)</b></summary>

### YOLO One-Liner (Copy & Paste)
```bash
sudo pacman -S --noconfirm base-devel linux-headers git && git clone https://github.com/hmtheboy154/gen4-mt7902.git && cd gen4-mt7902 && sudo mkdir -p /lib/firmware/mediatek/mt7902 && sudo cp firmware/* /lib/firmware/mediatek/mt7902/ && make -f Makefile.x86 KVER=$(uname -r) LINUX_VER=$(uname -r) KERNELRELEASE=$(uname -r) && sudo mkdir -p /usr/lib/modules/$(uname -r)/kernel/drivers/net/wireless/mediatek/ && sudo cp mt7902.ko /usr/lib/modules/$(uname -r)/kernel/drivers/net/wireless/mediatek/ && sudo depmod -a && sudo modprobe mt7902 && echo "mt7902" | sudo tee /etc/modules-load.d/mt7902.conf && cd .. && rm -rf gen4-mt7902
```

### Step-by-Step Installation
```bash
# 1. Install dependencies
sudo pacman -S --noconfirm base-devel linux-headers git

# 2. Clone repository and setup firmware
git clone https://github.com/hmtheboy154/gen4-mt7902.git
cd gen4-mt7902
sudo mkdir -p /lib/firmware/mediatek/mt7902
sudo cp firmware/* /lib/firmware/mediatek/mt7902/

# 3. Build the driver
make -f Makefile.x86 KVER=$(uname -r) LINUX_VER=$(uname -r) KERNELRELEASE=$(uname -r)

# 4. Install the module
sudo mkdir -p /usr/lib/modules/$(uname -r)/kernel/drivers/net/wireless/mediatek/ 
sudo cp mt7902.ko /usr/lib/modules/$(uname -r)/kernel/drivers/net/wireless/mediatek/

# 5. Load and persist
sudo depmod -a
sudo modprobe mt7902
echo "mt7902" | sudo tee /etc/modules-load.d/mt7902.conf

# 6. Clean up source folder
cd .. && rm -rf gen4-mt7902
```
</details>

<details>
<summary><b>🟧 Ubuntu / Debian / Mint (APT)</b></summary>

### YOLO One-Liner (Copy & Paste)
```bash
sudo apt update && sudo apt install -y build-essential linux-headers-$(uname -r) git && git clone https://github.com/hmtheboy154/gen4-mt7902.git && cd gen4-mt7902 && sudo mkdir -p /lib/firmware/mediatek/mt7902 && sudo cp firmware/* /lib/firmware/mediatek/mt7902/ && make -f Makefile.x86 KVER=$(uname -r) LINUX_VER=$(uname -r) KERNELRELEASE=$(uname -r) && sudo mkdir -p /usr/lib/modules/$(uname -r)/kernel/drivers/net/wireless/mediatek/ && sudo cp mt7902.ko /usr/lib/modules/$(uname -r)/kernel/drivers/net/wireless/mediatek/ && sudo depmod -a && sudo modprobe mt7902 && echo "mt7902" | sudo tee /etc/modules-load.d/mt7902.conf && cd .. && rm -rf gen4-mt7902
```

### Step-by-Step Installation
```bash
# 1. Update and install dependencies
sudo apt update
sudo apt install -y build-essential linux-headers-$(uname -r) git

# 2. Clone repository and setup firmware
git clone https://github.com/hmtheboy154/gen4-mt7902.git
cd gen4-mt7902
sudo mkdir -p /lib/firmware/mediatek/mt7902
sudo cp firmware/* /lib/firmware/mediatek/mt7902/

# 3. Build the driver
make -f Makefile.x86 KVER=$(uname -r) LINUX_VER=$(uname -r) KERNELRELEASE=$(uname -r)

# 4. Install the module
sudo mkdir -p /usr/lib/modules/$(uname -r)/kernel/drivers/net/wireless/mediatek/ 
sudo cp mt7902.ko /usr/lib/modules/$(uname -r)/kernel/drivers/net/wireless/mediatek/

# 5. Load and persist
sudo depmod -a
sudo modprobe mt7902
echo "mt7902" | sudo tee /etc/modules-load.d/mt7902.conf

# 6. Clean up source folder
cd .. && rm -rf gen4-mt7902
```
</details>

<details>
<summary><b>🟦 Fedora (DNF)</b></summary>

### YOLO One-Liner (Copy & Paste)
```bash
sudo dnf groupinstall -y "Development Tools" && sudo dnf install -y kernel-devel kernel-headers git && git clone https://github.com/hmtheboy154/gen4-mt7902.git && cd gen4-mt7902 && sudo mkdir -p /lib/firmware/mediatek/mt7902 && sudo cp firmware/* /lib/firmware/mediatek/mt7902/ && make -f Makefile.x86 KVER=$(uname -r) LINUX_VER=$(uname -r) KERNELRELEASE=$(uname -r) && sudo mkdir -p /usr/lib/modules/$(uname -r)/kernel/drivers/net/wireless/mediatek/ && sudo cp mt7902.ko /usr/lib/modules/$(uname -r)/kernel/drivers/net/wireless/mediatek/ && sudo depmod -a && sudo modprobe mt7902 && echo "mt7902" | sudo tee /etc/modules-load.d/mt7902.conf && cd .. && rm -rf gen4-mt7902
```

### Step-by-Step Installation
```bash
# 1. Install Development Tools and Headers
sudo dnf groupinstall -y "Development Tools"
sudo dnf install -y kernel-devel kernel-headers git

# 2. Clone repository and setup firmware
git clone https://github.com/hmtheboy154/gen4-mt7902.git
cd gen4-mt7902
sudo mkdir -p /lib/firmware/mediatek/mt7902
sudo cp firmware/* /lib/firmware/mediatek/mt7902/

# 3. Build the driver
make -f Makefile.x86 KVER=$(uname -r) LINUX_VER=$(uname -r) KERNELRELEASE=$(uname -r)

# 4. Install the module
sudo mkdir -p /usr/lib/modules/$(uname -r)/kernel/drivers/net/wireless/mediatek/ 
sudo cp mt7902.ko /usr/lib/modules/$(uname -r)/kernel/drivers/net/wireless/mediatek/

# 5. Load and persist
sudo depmod -a
sudo modprobe mt7902
echo "mt7902" | sudo tee /etc/modules-load.d/mt7902.conf

# 6. Clean up source folder
cd .. && rm -rf gen4-mt7902
```
</details>

<details>
<summary><b>🟩 openSUSE (Zypper)</b></summary>

### YOLO One-Liner (Copy & Paste)
```bash
sudo zypper install -y -t pattern devel_basis && sudo zypper install -y kernel-default-devel git && git clone https://github.com/hmtheboy154/gen4-mt7902.git && cd gen4-mt7902 && sudo mkdir -p /lib/firmware/mediatek/mt7902 && sudo cp firmware/* /lib/firmware/mediatek/mt7902/ && make -f Makefile.x86 KVER=$(uname -r) LINUX_VER=$(uname -r) KERNELRELEASE=$(uname -r) && sudo mkdir -p /usr/lib/modules/$(uname -r)/kernel/drivers/net/wireless/mediatek/ && sudo cp mt7902.ko /usr/lib/modules/$(uname -r)/kernel/drivers/net/wireless/mediatek/ && sudo depmod -a && sudo modprobe mt7902 && echo "mt7902" | sudo tee /etc/modules-load.d/mt7902.conf && cd .. && rm -rf gen4-mt7902
```

### Step-by-Step Installation
```bash
# 1. Install Development Patterns and Headers
sudo zypper install -y -t pattern devel_basis
sudo zypper install -y kernel-default-devel git

# 2. Clone repository and setup firmware
git clone https://github.com/hmtheboy154/gen4-mt7902.git
cd gen4-mt7902
sudo mkdir -p /lib/firmware/mediatek/mt7902
sudo cp firmware/* /lib/firmware/mediatek/mt7902/

# 3. Build the driver
make -f Makefile.x86 KVER=$(uname -r) LINUX_VER=$(uname -r) KERNELRELEASE=$(uname -r)

# 4. Install the module
sudo mkdir -p /usr/lib/modules/$(uname -r)/kernel/drivers/net/wireless/mediatek/ 
sudo cp mt7902.ko /usr/lib/modules/$(uname -r)/kernel/drivers/net/wireless/mediatek/

# 5. Load and persist
sudo depmod -a
sudo modprobe mt7902
echo "mt7902" | sudo tee /etc/modules-load.d/mt7902.conf

# 6. Clean up source folder
cd .. && rm -rf gen4-mt7902
```
</details>

---

## ⚠️ Notes
- **Secure Boot:** If you have **Secure Boot** enabled in your BIOS, the driver might fail to load unless you sign the module. It is highly recommended to **disable Secure Boot** if you encounter "Permission Denied" errors when running `modprobe`.
- **Kernel Updates:** You will need to **re-run** the installation steps every time your Linux kernel updates.

---

## 🗑 Uninstall / Cleanup
If you wanna uninstall (and you will probably have to), whether to re-install or just cleanup entirely, I made it easier for you. 
<details>
<summary><b>🟦 Arch Linux (Pacman)</b></summary>

### YOLO Cleanup (One-Liner)
```bash
sudo modprobe -r mt7902 && sudo rm /etc/modules-load.d/mt7902.conf && sudo rm -rf /lib/firmware/mediatek/mt7902 && sudo rm /usr/lib/modules/$(uname -r)/kernel/drivers/net/wireless/mediatek/mt7902.ko && sudo depmod -a && rm -rf gen4-mt7902
```

### Step-by-Step Cleanup
```bash
# 1. Unload the driver
sudo modprobe -r mt7902

# 2. Remove the auto-load configuration
sudo rm /etc/modules-load.d/mt7902.conf

# 3. Remove the firmware files
sudo rm -rf /lib/firmware/mediatek/mt7902

# 4. Remove the compiled kernel module
sudo rm /usr/lib/modules/$(uname -r)/kernel/drivers/net/wireless/mediatek/mt7902.ko

# 5. Update module dependencies
sudo depmod -a

# 6. Remove source code folder
rm -rf gen4-mt7902
```
</details>

<details>
<summary><b>🟧 Ubuntu / Debian / Mint (APT)</b></summary>

### YOLO Cleanup (One-Liner)
```bash
sudo modprobe -r mt7902 && sudo rm /etc/modules-load.d/mt7902.conf && sudo rm -rf /lib/firmware/mediatek/mt7902 && sudo rm /usr/lib/modules/$(uname -r)/kernel/drivers/net/wireless/mediatek/mt7902.ko && sudo depmod -a && rm -rf gen4-mt7902
```

### Step-by-Step Cleanup
```bash
# 1. Unload the driver
sudo modprobe -r mt7902

# 2. Remove auto-load configuration
sudo rm /etc/modules-load.d/mt7902.conf

# 3. Remove firmware files
sudo rm -rf /lib/firmware/mediatek/mt7902

# 4. Remove compiled kernel module
sudo rm /usr/lib/modules/$(uname -r)/kernel/drivers/net/wireless/mediatek/mt7902.ko

# 5. Update module dependencies
sudo depmod -a

# 6. Remove source code folder
rm -rf gen4-mt7902
```
</details>

<details>
<summary><b>🟦 Fedora (DNF)</b></summary>

### YOLO Cleanup (One-Liner)
```bash
sudo modprobe -r mt7902 && sudo rm /etc/modules-load.d/mt7902.conf && sudo rm -rf /lib/firmware/mediatek/mt7902 && sudo rm /usr/lib/modules/$(uname -r)/kernel/drivers/net/wireless/mediatek/mt7902.ko && sudo depmod -a && rm -rf gen4-mt7902
```

### Step-by-Step Cleanup
```bash
# 1. Unload the driver
sudo modprobe -r mt7902

# 2. Remove auto-load configuration
sudo rm /etc/modules-load.d/mt7902.conf

# 3. Remove firmware files
sudo rm -rf /lib/firmware/mediatek/mt7902

# 4. Remove compiled kernel module
sudo rm /usr/lib/modules/$(uname -r)/kernel/drivers/net/wireless/mediatek/mt7902.ko

# 5. Update module dependencies
sudo depmod -a

# 6. Remove source code folder
rm -rf gen4-mt7902
```
</details>

<details>
<summary><b>🟩 openSUSE (Zypper)</b></summary>

### YOLO Cleanup (One-Liner)
```bash
sudo modprobe -r mt7902 && sudo rm /etc/modules-load.d/mt7902.conf && sudo rm -rf /lib/firmware/mediatek/mt7902 && sudo rm /usr/lib/modules/$(uname -r)/kernel/drivers/net/wireless/mediatek/mt7902.ko && sudo depmod -a && rm -rf gen4-mt7902
```

### Step-by-Step Cleanup
```bash
# 1. Unload the driver
sudo modprobe -r mt7902

# 2. Remove auto-load configuration
sudo rm /etc/modules-load.d/mt7902.conf

# 3. Remove firmware files
sudo rm -rf /lib/firmware/mediatek/mt7902

# 4. Remove compiled kernel module
sudo rm /usr/lib/modules/$(uname -r)/kernel/drivers/net/wireless/mediatek/mt7902.ko

# 5. Update module dependencies
sudo depmod -a

# 6. Remove source code folder
rm -rf gen4-mt7902
```
</details>

---

## ⚠️ Disclaimer & Troubleshooting

Since this is a **community driver** and not officially baked into the Linux kernel, keep these tips in mind to stay connected:

- **Kernel Updates:** You will need to **recompile and reload** the driver whenever your **Linux kernel** gets a version update (usually when you see `linux`, `linux-headers`, or `kernel-devel` in your update list). If WiFi disappears after an update, just run the installation script again.
- **Sleep Issues:** The driver sometimes gets **"lazy"** after a prolonged sleep (suspend). If your WiFi doesn't wake up:
  1. Try toggling the WiFi **Off and On** in your system settings.
  2. If that fails, **Reboot** your system.
  3. If it's still being stubborn, run the **Cleanup** script followed by the **Installation** script (YOLO or Step-by-Step) to give it a fresh start. 

Basically, keep this repo handy—it’s your best friend until you finally buy that **Intel card**! 🫡

## Final Words
If this helped, then leave a **star** and share it with your friend(s) that still use **Windows** because their laptops dont have drivers for the **WiFi WC**. If you do this, not only will you be a good friend, but you will also be the **COOL friend** that flexes hard on Linux 💪🤣. Joking aside, I really hope it saves you at least a bit of the frustration I went through just to get my WiFi working. 