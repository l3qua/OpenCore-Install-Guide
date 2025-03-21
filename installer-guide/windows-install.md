# Making the installer in Windows

While you don't need a fresh install of macOS to use OpenCore, some users prefer having a fresh slate with their boot manager upgrades.

To start you'll need the following:

* 4GB USB Stick

* For USB larger than 16 GB to format in FAT32 use [Rufus method](#rufus-method)

* [macrecovery.py](https://github.com/acidanthera/OpenCorePkg/releases)
  * This will require [Python 3 installed](https://www.python.org/downloads/)

## Downloading macOS

To grab legacy installers is super easy, first grab a copy of [OpenCorePkg](https://github.com/acidanthera/OpenCorePkg/releases) and head to `/Utilities/macrecovery/`. Next, click next to the current folder path and type `cmd` to open a Command Prompt in the current directory:

![](../images/installer-guide/windows-install-md/open-cmd-current-folder.gif)

Now run one of the following depending on what version of macOS you want(Note these scripts rely on [Python 3](https://www.python.org/downloads/) support, please install if you haven't already):

```sh
# Lion (10.7):
py macrecovery.py -b Mac-2E6FAB96566FE58C -m 00000000000F25Y00 download
py macrecovery.py -b Mac-C3EC7CD22292981F -m 00000000000F0HM00 download

# Mountain Lion (10.8):
py macrecovery.py -b Mac-7DF2A3B5E5D671ED -m 00000000000F65100 download

# Mavericks (10.9):
py macrecovery.py -b Mac-F60DEB81FF30ACF6 -m 00000000000FNN100 download

# Yosemite (10.10):
py macrecovery.py -b Mac-E43C1C25D4880AD6 -m 00000000000GDVW00 download

# El Capitan (10.11):
py macrecovery.py -b Mac-FFE5EF870D7BA81A -m 00000000000GQRX00 download

# Sierra (10.12):
py macrecovery.py -b Mac-77F17D7DA9285301 -m 00000000000J0DX00 download

# High Sierra (10.13)
py macrecovery.py -b Mac-7BA5B2D9E42DDD94 -m 00000000000J80300 download
py macrecovery.py -b Mac-BE088AF8C5EB4FA2 -m 00000000000J80300 download

# Mojave (10.14)
py macrecovery.py -b Mac-7BA5B2DFE22DDD8C -m 00000000000KXPG00 download

# Catalina (10.15)
py macrecovery.py -b Mac-00BE6ED71E35EB86 -m 00000000000000000 download

# Big Sur (11)
py macrecovery.py -b Mac-42FD25EABCABB274 -m 00000000000000000 download

# Monterey (12)
py macrecovery.py -b Mac-FFE5EF870D7BA81A -m 00000000000000000 download

# Ventura (13)
py macrecovery.py -b Mac-4B682C642B45593E -m 00000000000000000 download

# Sonoma (14)
py macrecovery.py -b Mac-226CB3C6A851A671 -m 00000000000000000 download

# Latest version
# ie. Sequoia (15)
py macrecovery.py -b Mac-937A206F2EE63C01 -m 00000000000000000 download
```

* **macOS 12 and above note**: As recent macOS versions introduce changes to the USB stack, it is highly advisable that you map your USB ports (with USBToolBox) before installing macOS.
  * <span style="color:red"> CAUTION: </span> With macOS 11.3 and newer, [XhciPortLimit is broken resulting in boot loops](https://github.com/dortania/bugtracker/issues/162).
    * If you've already [mapped your USB ports](https://dortania.github.io/OpenCore-Post-Install/usb/) and disabled `XhciPortLimit`, you can boot macOS 11.3+ without issues.

This will take some time, however once you're finished you should get either BaseSystem or RecoveryImage files:

![](../images/installer-guide/windows-install-md/macrecovery-done.png)

| BaseSystem | RecoveryImage |
| :--- | :--- |
|![](../images/installer-guide/windows-install-md/basesystem-example.png) | ![](../images/installer-guide/windows-install-md/macrecovery-after.jpg) |

Now with our installer downloaded, we'll next want to format our USB.

## Making the installer

Here we'll be formatting our USB and adding macOS onto it, we have 3 options:

* [Disk Management method](#disk-management-method)
  * GUI Based, simplest way
  * Only UEFI systems are supported(ex. 2012+)
* [Rufus method](#rufus-method)
  * GUI Based, simplest way
  * For larger USB drives(16GB+)
* [diskpart method](#diskpart-method)
  * Command line based, little more work
  * Required for legacy systems(ie. non-UEFI, pre-2012)

### Disk Management method

Simply open up Disk Management, and format your USB as FAT32:

1. Right click the Start Button on your task bar and select Disk Management.
2. You should see all of your partitions and disks. On the bottom half, you'll see your devices. Find your USB.
3. You'll want to format the USB to have a FAT32 partition.

* If you have multiple partitions on the USB, right click each partition and click Delete Volume for your USB (This will remove data, make sure you have backups and only remove partitions from your USB)
  * Right click the unallocated space and create a new simple volume. Make sure it is FAT32 and at least a gigabyte or two big. Name it "EFI".
* Otherwise, right click the partition on the USB and click Format and set it to FAT32.

![](../images/installer-guide/windows-install-md/DiskManagement.jpg)

Next, go to the root of this USB drive and create a folder called `com.apple.recovery.boot`. Then move the downloaded BaseSystem or RecoveryImage files. Please ensure you copy over both the .dmg and .chunklist files to this folder:

![](../images/installer-guide/windows-install-md/com-recovery.png)

Now grab OpenCorePkg you downloaded earlier and open it:

![](../images/installer-guide/windows-install-md/base-oc-folder.png)

Here we see both IA32(32 Bit CPUs) and X64(64 Bit CPUs) folders, choose the one that's most appropriate to your hardware and open it. Next grab the EFI folder inside and place this on the root of the USB drive along side com.apple.recovery.boot. Once done it should look like this:

![](../images/installer-guide/windows-install-md/com-efi-done.png)

### Rufus method

1. Download [Rufus](https://rufus.ie/)
2. Set the BOOT selection as not bootable
3. Set File System as Large FAT32
4. Click Start
5. Delete all file autorun in USB Drive partition

![](../images/installer-guide/windows-install-md/format-usb-rufus.png)

Next, go to the root of this USB drive and create a folder called `com.apple.recovery.boot`. Then move the downloaded BaseSystem or RecoveryImage files. Please ensure you copy over both the .dmg and .chunklist files to this folder:

![](../images/installer-guide/windows-install-md/com-recovery.png)

Now grab OpenCorePkg you downloaded earlier and open it:

![](../images/installer-guide/windows-install-md/base-oc-folder.png)

Here we see both IA32(32 Bit CPUs) and X64(64 Bit CPUs) folders, choose the one that's most appropriate to your hardware and open it. Next grab the EFI folder inside and place this on the root of the USB drive along side com.apple.recovery.boot. Once done it should look like this:

![](../images/installer-guide/windows-install-md/com-efi-done.png)

### diskpart method

::: details diskpart method

Press Windows + R and enter `diskpart`.

Now run the following:

```sh
# List available disks
list disk
# Select your disk(ie. disk 1)
select disk 1
# Format the drive
clean
# Convert to GPT
# Due to an odd bug with BOOTICE and DuetPkg, MBR disks will fail to boot
convert gpt
# Create a new partition
create partition primary
# Select your partition
# Running clean ensures we only have 1 partition so it will be "partition 1"
select partition 1
# Format the drive as FAT32
format fs=fat32 quick
# Assign a drive letter(ie. Drive E, ensure it's not currently in use)
ASSIGN LETTER=E
```

Next, go to the root of this USB drive and create a folder called `com.apple.recovery.boot`. Then move the downloaded BaseSystem or RecoveryImage files. Please ensure you copy over both the .dmg and .chunklist files to this folder:

![](../images/installer-guide/windows-install-md/com-recovery.png)

Now grab OpenCorePkg you downloaded earlier and open it:

![](../images/installer-guide/windows-install-md/base-oc-folder.png)

Here we see both IA32(32 Bit CPUs) and X64(64 Bit CPUs) folders, choose the one that's most appropriate to your hardware and open it. Next grab the EFI folder inside and place this on the root of the USB drive along side com.apple.recovery.boot. Once done it should look like this:

![](../images/installer-guide/windows-install-md/com-efi-done.png)

::: details Legacy Install Setup

If your firmware does not support UEFI, see below instructions:

To start, you'll need the following:

* [7-Zip](https://www.7-zip.org)
* [BOOTICE](https://www.majorgeeks.com/files/details/bootice_64_bit.html)
* [OpenCorePkg](https://github.com/acidanthera/OpenCorePkg/releases)

Next, open up BOOTICE and ensure you've selected the right drive.

![](../images/installer-guide/windows-install-md/bootice.png)

Next, enter "Process MBR" then select "Restore MBR" and select the **boot0** file from `Utilities/LegacyBoot/` in OpenCorePkg:

| Restore MBR | Restore boot0 file |
| :--- | :--- |
| ![](../images/installer-guide/windows-install-md/restore-mbr.png) | ![](../images/installer-guide/windows-install-md/restore-mbr-file.png) |

Then head back to the main screen and select "Process PBR" then "Restore PBR". From here, choose the **boot1f32** file from `Utilities/LegacyBoot/` in OpenCorePkg:

| Restore PBR | Restore boot1f32 file |
| :--- | :--- |
| ![](../images/installer-guide/windows-install-md/restore-pbr.png) | ![](../images/installer-guide/windows-install-md/restore-pbr-file.png) |

Once this is done, head back to your USB and do 1 final thing. Grab either the **bootx64**(64 Bit CPUs) or **bootia32**(32 Bit CPUs) file from `Utilities/LegacyBoot/` and place it on the root of your drive. **Rename this file to boot** to ensure DuetPkg can properly:

![](../images/installer-guide/windows-install-md/final-boot-file.png)

:::

## Making an offline installer

This is the only way to install macOS without an internet connection (without a mac) if you can't set up one right now. For instructions on how to do it, read the README.md file in [Unplugged Repo](https://github.com/corpnewt/UnPlugged?tab=readme-ov-file)

## Now with all this done, head to [Setting up the EFI](./opencore-efi.md) to finish up your work
