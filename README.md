# How-to-Update-the-BIOS-of-the-HP-ProLiant-ML110-G7-to-2019.04.04

This guide is for anyone stuck trying to update the BIOS on the ProLiant ML110 Gen7 — a notoriously stubborn machine whose firmware updates are pay-walled on HPE’s official site.
Fortunately, the correct BIOS package still exists on an HPE-hosted repository, and the update can be installed using Red Hat Enterprise Linux 7.9.

This is a recreation of the full working process.

-------------------

Requirements:

-USB flash drive (8 GB or larger)

-Red Hat Enterprise Linux 7.9 DVD ISO (This version is required — newer versions break the .scexe installer).

-Rufus (to flash the ISO)

-Spare SATA drive for temporary Linux installation (optional if you have a PS/2 keyboard that works with the Live boot menu).

-Internet connection on the server (Ethernet or Android USB tethering both work).

-The BIOS update file (BIOS Download Link HPE official mirror):

https://p1lg507061.it.hpe.com/SDR/repo/fwpp-gen8/gen8.1_hotfix16/

-Download the file: firmware-system-j01-2019.04.04-1.1.i386.rpm

-------------------

Step 1 — Prepare the USB

-Open Rufus on Windows.

-Select the Red Hat 7.9 ISO.

-Flash it to the USB (default settings are fine).

-------------------

Step 2 — Reset BIOS

-Boot the ML110 G7 and press F9 to enter BIOS.

-Load Default Settings.

-Save & exit.

-------------------

Step 3 — Adjust BIOS Settings

-Enter BIOS again (F9) and:

-Set SATA Controller → AHCI

-Set Boot Order:

-1st: USB

-2nd: Hard Drive

-Save changes and power off the system.

-------------------

Step 4 — Connect Drives & Boot Installer

-Connect a spare SATA HDD/SSD for Linux installation.

-Plug the USB with RHEL 7.9.

-Power on.

**Important note**

On many ML110 G7 units, USB keyboards do not work in the Red Hat boot menu, so you cannot select “Try Live System.”
If this happens, the installer auto-starts after 60 seconds.

-------------------

Step 5 — Install Red Hat 7.9

-Choose the installation option with GUI.

-Create a user and check “Make this user administrator.”

-Complete the install.

**The system may not reboot automatically.**

-If it hangs:

-Unplug power,

-remove the USB stick,

-plug power back in,

-boot into your new Linux installation.

-------------------

Step 6 — Get Internet & Download BIOS

**Option A: Ethernet**

-Connect cable → should auto-connect.

**Option B: Android USB tethering**

-Connect phone via USB.

-Enable USB tethering.

-Click the network icon (top right of Linux desktop) and select the tethered connection.

-Open the browser and download the BIOS package:

https://p1lg507061.it.hpe.com/SDR/repo/fwpp-gen8/gen8.1_hotfix16/firmware-system-j01-2019.04.04-1.1.i386.rpm

-------------------

Step 7 — Extract and Run the Firmware Installer

-Inside the Downloads folder:

-Open Terminal Here (right-click → “Open in Terminal”)

-Extract the RPM:

sudo rpm2cpio firmware-system-j01-2019.04.04-1.1.i386.rpm | cpio -idmv

-Locate the .scexe firmware file:

sudo find . -type f -name "CP039722.scexe"

-Copy the file path from previous command and run installer like this:

sudo ./<File_Path>/CP039722.scexe

-You will see the update process in the terminal. It will ask if you want to proceed, type "y". When complete, reboot the system.

-------------------

Step 8 — Confirm BIOS Update

-Restart → press F9 → check System Information.

-You should now see: System ROM: J01 2019.04.04

-After verifying, reset BIOS to defaults again (recommended by HP after ROM flashes).

Done!

Your ProLiant ML110 Gen7 is now running the latest BIOS (2019.04.04), bypassing HPE’s paywall and using only officially-hosted files.
