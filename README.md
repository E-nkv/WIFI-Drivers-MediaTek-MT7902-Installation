# 🚀 MT7902 WiFi Driver Installation for Linux

This repo provides instructions for installing community drivers for the MediaTek MT7902 Wireless Card, to allow WiFi connectivity on Linux systems, considering that MediaTek nor the Linux Kernel team don't provide support for official MT7902 linux drivers. The reason for this is that MediaTek has not released the source code or upstreamed support to the Linux kernel, so to have WiFi we must either install the community drivers, or go for the alternatives.

# Motivation
I currently have an Acer Aspire 3 15 laptop, which has the MT7902 Wireless Card (WC), and it has been a frustrating process trying to make things work. It pretty much went like this: 
- I have Windows11 and 8gb of RAM. I realise that it's plain impossible to have WSL2, VS-Code, and a few Chrome tabs opened simultaneously, let alone additionally using Docker Desktop or some DB editor (Dbeaver or Beekeeper Studio), which are a MUST in my dev workflow. 
- I install Linux (Fedora Workstation 43). Then I suddenly realise "Wow. There is no WiFi."
- I search a lot, from trying different workarounds I found on github. None worked. So I bought a dongle.
- The dongle broke, awesome. So, not wanting to buy another dongle, I went back to Windows. (spoiler: DON'T DO THAT).
- I realised why I had switched from Windows in the first place. 1 minute of booting time, bloated system, and more importantly - laptop freezes when I literally use VSCode and Chrome with ONE tab. 
- I install Linux again (specifically ArchLinux. just thought it would be fun, and hell yes it was 😋), with the mindset of using my phone's WiFi via USB until I buy another dongle. (spoiler: your laptop's battery will tell you - "Bro what u doin??")
- I come accross [https://github.com/hmtheboy154/gen4-mt7902.git](gen4-mt7902 from hmtheboy154 on github), install it, and I FINALLY HAVE WIFI without a dongle.


# Alternatives
1. Use Windows. But PLEASE DON'T.
2. Buy a Wifi Dongle. Make sure it's linux-compatible. guide yourself by [https://github.com/morrownr/USB-WiFi/blob/main/home/USB_WiFi_Adapters_that_are_supported_with_Linux_in-kernel_drivers.md](this github repo) and double-check with a bit of prompting before buying. Cost - anywhere from 1 to 20 bucks. (i bought my FENVI adapter on Aliexpress for literally 60 cents due to a discount).
3. Replace your MT-7902 WC (Wireless Card) for an Intel one (Intel AX200/AX210/AX211), since they offer better out-of-the-box linux driver support, assuming its not currently soldiered to your laptop. Prompt a bit to find more about this. I intend on doing this on the future. Cost - from 18 to 30 bucks.


---

## 🛠 Installation

Choose Your Distro
<details>
<summary><b>🟦 Arch Linux (Pacman)</b></summary>

### YOLO One-Liner (Copy & Paste)
```bash
sudo pacman -S --noconfirm base-devel linux-headers git && git clone https://github.com/hmtheboy154/gen4-mt7902.git && cd gen4-mt7902 && sudo mkdir -p /lib/firmware/mediatek/mt7902 && sudo cp firmware/* /lib/firmware/mediatek/mt7902/ && make -f Makefile.x86 KVER=$(uname -r) LINUX_VER=$(uname -r) KERNELRELEASE=$(uname -r) && sudo mkdir -p /usr/lib/modules/$(uname -r)/kernel/drivers/net/wireless/mediatek/ && sudo cp mt7902.ko /usr/lib/modules/$(uname -r)/kernel/drivers/net/wireless/mediatek/ && sudo depmod -a && sudo modprobe mt7902 && echo "mt7902" | sudo tee /etc/modules-load.d/mt7902.conf
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
```
</details>

<details>
<summary><b>🟧 Ubuntu / Debian / Mint (APT)</b></summary>

### YOLO One-Liner (Copy & Paste)
```bash
sudo apt update && sudo apt install -y build-essential linux-headers-$(uname -r) git && git clone https://github.com/hmtheboy154/gen4-mt7902.git && cd gen4-mt7902 && sudo mkdir -p /lib/firmware/mediatek/mt7902 && sudo cp firmware/* /lib/firmware/mediatek/mt7902/ && make -f Makefile.x86 KVER=$(uname -r) LINUX_VER=$(uname -r) KERNELRELEASE=$(uname -r) && sudo mkdir -p /usr/lib/modules/$(uname -r)/kernel/drivers/net/wireless/mediatek/ && sudo cp mt7902.ko /usr/lib/modules/$(uname -r)/kernel/drivers/net/wireless/mediatek/ && sudo depmod -a && sudo modprobe mt7902 && echo "mt7902" | sudo tee /etc/modules-load.d/mt7902.conf
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
```
</details>

<details>
<summary><b>🟦 Fedora (DNF)</b></summary>

### YOLO One-Liner (Copy & Paste)
```bash
sudo dnf groupinstall -y "Development Tools" && sudo dnf install -y kernel-devel kernel-headers git && git clone https://github.com/hmtheboy154/gen4-mt7902.git && cd gen4-mt7902 && sudo mkdir -p /lib/firmware/mediatek/mt7902 && sudo cp firmware/* /lib/firmware/mediatek/mt7902/ && make -f Makefile.x86 KVER=$(uname -r) LINUX_VER=$(uname -r) KERNELRELEASE=$(uname -r) && sudo mkdir -p /usr/lib/modules/$(uname -r)/kernel/drivers/net/wireless/mediatek/ && sudo cp mt7902.ko /usr/lib/modules/$(uname -r)/kernel/drivers/net/wireless/mediatek/ && sudo depmod -a && sudo modprobe mt7902 && echo "mt7902" | sudo tee /etc/modules-load.d/mt7902.conf
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
```
</details>

<details>
<summary><b>🟩 openSUSE (Zypper)</b></summary>

### YOLO One-Liner (Copy & Paste)
```bash
sudo zypper install -y -t pattern devel_basis && sudo zypper install -y kernel-default-devel git && git clone https://github.com/hmtheboy154/gen4-mt7902.git && cd gen4-mt7902 && sudo mkdir -p /lib/firmware/mediatek/mt7902 && sudo cp firmware/* /lib/firmware/mediatek/mt7902/ && make -f Makefile.x86 KVER=$(uname -r) LINUX_VER=$(uname -r) KERNELRELEASE=$(uname -r) && sudo mkdir -p /usr/lib/modules/$(uname -r)/kernel/drivers/net/wireless/mediatek/ && sudo cp mt7902.ko /usr/lib/modules/$(uname -r)/kernel/drivers/net/wireless/mediatek/ && sudo depmod -a && sudo modprobe mt7902 && echo "mt7902" | sudo tee /etc/modules-load.d/mt7902.conf
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
```
</details>

---

## ⚠️ Notes
- **Secure Boot:** If you have Secure Boot enabled in your BIOS, the driver might fail to load unless you sign the module. It is recommended to **disable Secure Boot** if you encounter "Permission Denied" errors when running `modprobe`.
- **Kernel Updates:** You will need to re-run the `make` and `cp` steps every time your Linux kernel updates.

---

## 🗑 Uninstall / Cleanup

If you need to remove the driver and all associated files, use the instructions below for your distribution.

<details>
<summary><b>🟦 Arch Linux (Pacman)</b></summary>

### YOLO Cleanup (One-Liner)
```bash
sudo modprobe -r mt7902 && sudo rm /etc/modules-load.d/mt7902.conf && sudo rm -rf /lib/firmware/mediatek/mt7902 && sudo rm /usr/lib/modules/$(uname -r)/kernel/drivers/net/wireless/mediatek/mt7902.ko && sudo depmod -a
```

### Step-by-Step Cleanup
```bash
# 1. Unload the driver from the kernel
sudo modprobe -r mt7902

# 2. Remove the auto-load configuration
sudo rm /etc/modules-load.d/mt7902.conf

# 3. Remove the firmware files
sudo rm -rf /lib/firmware/mediatek/mt7902

# 4. Remove the compiled kernel module
sudo rm /usr/lib/modules/$(uname -r)/kernel/drivers/net/wireless/mediatek/mt7902.ko

# 5. Update module dependencies
sudo depmod -a
```
</details>

<details>
<summary><b>🟧 Ubuntu / Debian / Mint (APT)</b></summary>

### YOLO Cleanup (One-Liner)
```bash
sudo modprobe -r mt7902 && sudo rm /etc/modules-load.d/mt7902.conf && sudo rm -rf /lib/firmware/mediatek/mt7902 && sudo rm /usr/lib/modules/$(uname -r)/kernel/drivers/net/wireless/mediatek/mt7902.ko && sudo depmod -a && sudo rm -rf ~/gen4-mt7902
```

### Step-by-Step Cleanup
```bash
# 1. Unload the driver from the kernel
sudo modprobe -r mt7902

# 2. Remove the auto-load configuration
sudo rm /etc/modules-load.d/mt7902.conf

# 3. Remove the firmware files
sudo rm -rf /lib/firmware/mediatek/mt7902

# 4. Remove the compiled kernel module
sudo rm /usr/lib/modules/$(uname -r)/kernel/drivers/net/wireless/mediatek/mt7902.ko

# 5. Update module dependencies
sudo depmod -a
```
</details>

<details>
<summary><b>🟦 Fedora (DNF)</b></summary>

### YOLO Cleanup (One-Liner)
```bash
sudo modprobe -r mt7902 && sudo rm /etc/modules-load.d/mt7902.conf && sudo rm -rf /lib/firmware/mediatek/mt7902 && sudo rm /usr/lib/modules/$(uname -r)/kernel/drivers/net/wireless/mediatek/mt7902.ko && sudo depmod -a
```

### Step-by-Step Cleanup
```bash
# 1. Unload the driver from the kernel
sudo modprobe -r mt7902

# 2. Remove the auto-load configuration
sudo rm /etc/modules-load.d/mt7902.conf

# 3. Remove the firmware files
sudo rm -rf /lib/firmware/mediatek/mt7902

# 4. Remove the compiled kernel module
sudo rm /usr/lib/modules/$(uname -r)/kernel/drivers/net/wireless/mediatek/mt7902.ko

# 5. Update module dependencies
sudo depmod -a
```
</details>

<details>
<summary><b>🟩 openSUSE (Zypper)</b></summary>

### YOLO Cleanup (One-Liner)
```bash
sudo modprobe -r mt7902 && sudo rm /etc/modules-load.d/mt7902.conf && sudo rm -rf /lib/firmware/mediatek/mt7902 && sudo rm /usr/lib/modules/$(uname -r)/kernel/drivers/net/wireless/mediatek/mt7902.ko && sudo depmod -a
```

### Step-by-Step Cleanup
```bash
# 1. Unload the driver from the kernel
sudo modprobe -r mt7902

# 2. Remove the auto-load configuration
sudo rm /etc/modules-load.d/mt7902.conf

# 3. Remove the firmware files
sudo rm -rf /lib/firmware/mediatek/mt7902

# 4. Remove the compiled kernel module
sudo rm /usr/lib/modules/$(uname -r)/kernel/drivers/net/wireless/mediatek/mt7902.ko

# 5. Update module dependencies
sudo depmod -a
```
</details>