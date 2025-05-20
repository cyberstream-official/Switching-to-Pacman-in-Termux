# **Switching Package Managers in Termux: From APT to Pacman**

Termux can use different package managers, but APT is the default. Some people like Pacman (which is used in Arch Linux) better because of its cool features and ecosystem. This guide will show you how to switch from APT to Pacman safely and effectively by setting up a separate environment and replacing the initial setup.

# **Instruction:**

### **1. Set Up an Isolated Environment:**

Let's start by making a new folder (let's call it usr-n) to keep the Pacman environment. This way, you can test and set up Pacman without messing up your current setup.
```bash
mkdir -p $PREFIX/../usr-n && cd $_
```
> [!faq] Why usr-n?
By keeping Pacman separate, you can avoid conflicts and easily remove it if needed.

---

### **2. Download the Pacman Bootstrap:**

Download the right Pacman bootstrap file for your device's architecture (like aarch64) from the [official GitHub releases](https://github.com/termux-pacman/termux-packages/releases).
Move the downloaded file to your working directory:
```bash
mv /storage/emulated/0/Download/bootstrap-aarch64.zip .
```
> [!note]
Make sure you have the correct file name and are in the right folder before moving on.

---

### **3. Extract the Bootstrap Archive:**

Unzip the downloaded file to set up your new environment, then delete the zip file:
```bash
unzip bootstrap-aarch64.zip && rm bootstrap-aarch64.zip
```
> [!tip]
This will unpack the Pacman-based environment into usr-n, getting it ready for use.

---

### **4. Generate Symbolic Links:**

Use the SYMLINKS.txt file from the archive to create necessary symbolic links automatically:
```bash
cat SYMLINKS.txt | awk -F "←" '{system("ln -s '"'"'"$1"'"'"' '"'"'"$2"'"'"'")}'
```
> [!info] What this does
It reads each line from SYMLINKS.txt, splits it at the ← character, and creates symbolic links from the source to the destination path. This makes sure that binaries and libraries are linked correctly.

---

### **5. Enter Termux Failsafe Mode:**

> [!warning] Important: Exit Normal Session
Before making the switch, close Termux completely. Then reopen it in Failsafe Mode to avoid any issues.

• To get to Failsafe Mode:
• Long-press the Termux icon
Select Failsafe
(Only available through F-Droid or compatible installs.)

---

### **6. Replace the Default Environment:**

Once in Failsafe Mode, swap out the APT-based environment with your new Pacman setup:
```bash
cd $PREFIX/..
rm -rf usr/
mv usr-n/ usr/
```
> [!danger] Caution:
This step is permanent unless you've backed up your original setup. Make sure everything works in usr-n before making the switch.

---

### **7. Initialize the Pacman Keyring:**

To check and install packages, set up and fill the Pacman GPG keyring:
```bash
pacman-key --init
pacman-key --populate
```
> [!note]
This step is important for safe package management with Pacman.
