# 🚀 MT7902 WiFi Driver Installation for Linux

This repository provides instructions and drivers for the MediaTek MT7902 Wireless Card. Follow the instructions for your specific distribution below.

---

## 🛠 Choose Your Distribution

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