# EFI for ASUS TUF 504 GD

Configurations working with macOS Catalina 10.15.x4

<br/>

# Prerequisites

- 16GB+ USB Flash Drive – Catalina installer is slightly larger than 8GB and so a 16GB or larger flash drive is required.
- Computer running macOS – Access to a macOS is needed to create a flash drive installer of Catalina. If you don’t have an existing hackintosh or a Mac the alternative is to install macOS with a virtual machine on windows or borrow a Mac from friend or family.

# Step 1: Download - get a copy of macOS Catalina

To begin the setup of a Hackintosh we first need a clean copy of Catalina, which can be downloaded from the App Store or [macOS catalina 10.15.6](http://google.com.vn)

<br/>

# Step 2: Installer – Create the macOS Catalina flash drive

**Part 1: Formatting the USB Drive**

To get a working Catalina installer onto a USB flash drive it must first be formatted into a scheme that allows for an EFI Partition.<br/>
In order to create a bootable macOS installer the USB flash drive must be formatted as Mac OS Extended (Journaled) which will add a second hidden EFI partition needed to create a boot loader.

1. **Insert** a USB Flash Drive.
2. Open **Disk Utility**.
3. Select the Flash Drive on the left column.
4. Click **Erase**.
5. Set the following settings:
   - Name: **Hackintosher**.
   - Format: **Mac OS Extended (Journaled)**.
   - Scheme: **GUID Partition Map**.
6. Click **Erase**.
7. Click **Done** upon finish.

**Part 2: Transferring the Catalina Installer**

Afterwards the the Catalina Installer downloaded from the Mac App Store is ready to be placed on the USB drive using Terminal.

1. Verify **Install macOS Catalina** is in the /Applications folder.
2. Open **Terminal** through spotlight or launchpad.
3. Paste the following into Terminal entering your password if prompted:<br />

```bash
sudo /Applications/Install\ macOS\ Catalina.app/Contents/Resources/createinstallmedia --volume /Volumes/Hackintosher /Applications/Install\ macOS\ Catalina.app --nointeraction
```

4. Do not close Terminal, the installer will transfer over slowly and can take awhile especially if you are using USB 2.0.

**Part 3: Catalina EFI Partition & Drop Files**

1. **Mount EFI Partition:**
   1. Download Clover Configurator.
   2. Open Clover Configurator.
   3. Select Mount EFI under TOOLS.
   4. Click Mount Partition for Install macOS….
   5. Click Open Partition.
2. **Drop my EFI Folder in:**
   1. Delete the folder called EFI in the partition you mounted (If it exists).
   2. Copy EFI folder.
   3. Paste the EFI folder into your mounted EFI partition or /Volumes/EFI.

# Step 3: BIOS – Recommended Settings

Restart your computer and press the ```F2``` key to enter BIOS when computer posts the splash screen logo.

You’re motherboard may not have all of these settings and that’s okay:

**Change the following settings:**

- Virtualization : **Enabled**
- VT-d : **Disabled**
- XHCI Hand-Off : **Enabled**
- Legacy USB Support: **Auto/Enabled**
- IO SerialPort : **Disabled**
- Network Stack : **Disabled**
- XMP Profile : **Auto / Profile 1/Enabled**
- **UEFI Booting** set to **Enabled** and set **Priority** over Legacy
- Secure Boot : **Disabled**
- Fast Boot : **Disabled**
- OS Type: **Other OS**
- Wake on LAN : **Disabled**

**Setting Boot Device:**

Enter BIOS and set Boot → Boot Option #1: UEFI : XXX (For example I used a SanDisk flash drive so I would select UEFI: Sandisk, Partition 1) This is easier if you don’t want to spam F8 on startup every time to boot macOS.

Press F10 to Save and **Exit the BIOS**

# Step 3: Install – macOS Catalina Hackintosh Installation

[Step-by-step walkthrough of installing macOS Catalina](https://hackintosher.com/guides/mac-os-catalina-hackintosh-clover-walkthrough-10-15-install/)

# Step 4: Post-Install macOS Catalina Hackintosh

Install [HeliPort](https://github.com/OpenIntelWireless/HeliPort/releases) to be Intel WiFi Client

Now that you have macOS up and running here are some post install steps to help you.

- Test audio output if its not working you may need to change something called an audio layout-id which is a single number in your ```config.plist```
- All system files were moved to a read-only partition meaning /L/E or /S/L/E can’t be edited by you when adding or removing kexts.
    - To get around this after Catalina is installed these folders need to be mounted with write privileges through the Terminal app with ```sudo mount -uw /```
- You should also be able to access the internet since I included kexts for four Ethernet chipsetes, there are four of them in ```EFI/Clover/kexts/Other``` you may delete the ones you don’t need.
- ```darkwake=0``` is set as a bootflag in config.plist by default. ```darkwake=8``` might be better for waking up an ASRock or MSI motherboard.
If you have issues with restarting when trying to shutdown try changing ```FixShutdown``` in ```config.plist``` under ```Acpi > Fixes```
