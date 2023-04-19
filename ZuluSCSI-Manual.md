# ZuluSCSI Manual

## Table of contents

* [Introduction to ZuluSCSI](#introduction-to-ZuluSCSI)

  * [Overview](#introduction-overview)
  * [ZuluSCSI History](#introduction-history)
  * [ZuluSCSI Models](#introduction-models)
  * [Features](#introduction-features)

* [Using ZuluSCSI](#using-the-ZuluSCSI)

  * [SD card requirements](#SD-card-requirements)
  * [Quickstart Guide](#using-quickstart)
  * [Understanding SCSI](#using-scsi) 
  * [Installation](#using-installation)
  * [General Use](#using-general)
  * [Understanding File Systems](#using-formats)

## <a id="introduction-to-ZuluSCSI"></a> Introduction to ZuluSCSI

### <a id="introduction-overview"></a> Overview

ZuluSCSI is a SCSI hard drive emulator. Physical disk devices are represented by images stored on a SD card. 

### <a id="introduction-history"></a> ZuluSCSI History & Lineage

In response to ongoing global electronics component shortages, ZuluSCSI development began in November, 2021 by Rabbit Hole Computing. ZuluSCSI is different from other SD card SCSI emulation devices and projects. Unlike with SCSI2SD, no configuration utility is used or necessary. All necessary ZuluSCSI documentation is here.

### <a id="introduction-models"></a> ZuluSCSI Models

| Device version             | Description                                                  |
| :------------------------- | :----------------------------------------------------------- |
| ZuluSCSI RP2040            | A full-sized ZuluSCSI with a 50 pin internal SCSI connector and switch-controlled termination. based on the RP2040 microcontroller. |
| ZuluSCSI v1.1              | A full-sized ZuluSCSI with a 50 pin internal SCSI connector and switch-controlled termination |
| ZuluSCSI Mini              | Has a compact form factor, a plastic case, a DB25 pin external SCSI connector, permanently enabled termination and uses a micro SD card for storage.  |
| ZuluSCSI V1.1 2.5" Laptop/PowerBook | A variation of the ZuluSCSI Mini for Apple Macintosh PowerBook laptops. It has a 50 pin internal connector, fixed, permanently enabled SCSI termination, and uses a microSD card for storage. |

### <a id="introduction-features"></a> Features

| Hardware Version/Edition                                     | RP2040    |V1.0       | V1.1      | V1.1 PowerBook | V1.0 Mini & RP2040 Mini |
| ------------------------------------------------------------ | --------- | --------- | --------- | -------------- |-------------------------|
| DIP-switch configurable termination                          | Yes       | Yes       | Yes       | Yes            | Always On      |
| LED pin header location for _optional_ external LED pin header | Yes       | Yes       | Yes       | Yes            | No             |
| SD Card Type                                                 | Full Size | Full Size | Full Size | MicroSD        | MicroSD        |

All ZuluSCSI devices are compatible with a wide range of SCSI enabled computers and devices, and are capable of emulating a range of different devices, including hard drives, CD-ROMs, SCSI floppy, and removable media. All models can operate solely on power from the SCSI connector in most instances, when SCSI termination power is provided. 

## <a id="configuration"></a> ZuluSCSI Configuration

Configure ZuluSCSI using files on a FAT32 or exFAT-formatted SD or microSD card.

ZuluSCSI searches the SD card for files with a specific filename structure, for files ending in `.img`, `.hda`, or `.iso` format.

ZuluSCSI searches for files named `HDn.img` or `HDn.hda`, where 'n' is a unique SCSI ID (0-7). Note: SCSI ID 7 is often reserved for the controller. Each file with a unique SCSI ID will be presented to the host controller as a SCSI drive.

ZuluSCSI can be configured in more detail via an optional text file named zuluscsi.ini. The [example file](https://github.com/ZuluSCSI/ZuluSCSI-firmware/blob/main/zuluscsi.ini) shows how to configure ZuluSCSI via the `zuluscsi.ini` file.

At initialization, ZuluSCSI logs details and configured files to `zululog.txt`

### <a id="firmware-update"></a> Updating firmware

Copy the new firmware file named `zuluSCSIxxx.bin` (where xxx is the version) to the root directory of an SD card, then apply power to the ZuluSCSI. The ZuluSCSI will self update. For approximately two seconds, the LED will flash rapidly.

When the update is complete, ZuluSCSI will delete the firmware file from the SD card. It will automatically reboot, using the new firmware.

### <a id="dip-switches"></a> Configuration switch settings

ZuluSCSI RP2040 and V1.1 include a four-position DIP switch. By default, TERM (termination) is on, and all others are off.

### TERM

Termination should remain on if ZuluSCSI is the last physical device on the SCSI bus.

### BOOT

When enabled, the DFU bootloader is executed instead of the normal ZuluSCSI firmware. This is for firmware recovery, and should not be enabled for normal operation.

### <a id="troubleshooting"></a>Troubleshooting

The LED indicator normally flashes to indicate disk activity.

It also reports following status conditions:

- 1 fast blink on boot: Firmware successfully loaded, and at least one image file loaded successfully
- 3 fast blinks: No images found on SD card
- 5 fast blinks: SD card not detected
- Continuous morse pattern: firmware crashed, morse code indicates crash location

If a crash occurrs, the firmware will also attempt to save information to a separate `zuluerr.txt` log file, on the root of the SD card.

No LED activity: verify the 'BOOT' configuration switch is OFF.

## <a id="using-the-ZuluSCSI"></a> Using the ZuluSCSI

## <a id="SD-card-requirements"></a> SD card requirements

The ZuluSCSI firmware requires a MBR/DOS-partitioned SD card, and at least one FAT32 or exFAT-formatted SD partition, which must be labeled SDHC or SDXC. Older SD cards manufactured prior to around 2008, generally ~4GB and smaller, may not be detected by the SDIO interface of the microcontroller. This is a hardware/silicon limitation. ZuluSCSI has no upper limit on the size of SD cards it is compatible with, and is known to work with large 256GB and 400GB SD cards which are currently commercially available. SD cards partitioned with GPT (GUID Partition Map) can not be read by ZuluSCSI, and the ZuluSCSI will not detect such SD cards, resulting in the LED flashing five times.

### <a id="using-quickstart"></a> Quickstart guide

These steps get your ZuluSCSI up and running quickly, emulatiing a single SCSI hard drive.

Create or copy a single (or up to six), valid disk image file to the root directory of an MBR/DOS-partitioned FAT32 or exFAT-formatted SD card of any capacity. Name the image "HDx.img" or "HDx.hda", where 'x' is a unique SCSI ID, between 0 and 6 (ID 7 is often the controller's ID).

Image files named "CDx.iso" or "CDx.img" will automatically be configured by the ZuluSCSI firmware as a removable SCSI CD-ROM drive, with a standard 2048-byte sector size.  

To use the ZuluSCSI with the original, un-patched **Apple HD SC Setup** formatting utility included with earlier Macintosh operating systems, set configuration switch SW1 (closest to the SD card socket) ON. This switch changes the SCSI product & manufacturer IDs to values that Apple's HD SC Setup utility has whitelisted as supported. If the SW1 switch is not in the on position, HD SC Setup will not recognise the ZuluSCSI as a suitable/usable SCSI hard drive due to its [limitations](#limitations-formatting). With SW1 off, you would need to use a third-party hard drive formatting utility, such as **Hard Disk Toolkit**, **Silverlining** or **Lido**. There is also a patched version of the **Apple HD SC Setup** utility available with the Apple-imposed limitations removed.

Copy a file to the SD card's root directory, named `HD5.img`, `HD5.hda`, or `CD5.img`, replacing the 5 with your desired SCSI ID. Multiple SCSI devices can be emulated by naming different files with different IDs.

Always power down your vintage device. Connect the ZuluSCSI to the device's 50-pin SCSI cable, or external 25 pin connector. ZuluSCSI can often take power from the SCSI bus, so the floppy-style BERG power connecter can be left disconnected. 

Power on your vintage device and start using your ZuluSCSI!

[//]: # For more information about transferring files to and from the SD card, or using pre-configured SD cards, read the [transferring data](#using-transferring) section.

### <a id="using-scsi"></a> Understanding SCSI

The original SCSI standard allows a maximum of 7 devices to be connected to the host. Strictly speaking there is allowance for 8, but the host counts as one device, leaving only 7 IDs available.

Each device must be assigned a unique ID ranging from 0 to 7. Multiple devices can be connected via daisy chain. Termination is required at either end of the SCSI bus (not the lowest and highest SCSI ID). Termination is also required for a single device SCSI chain. Termination absorbs the electrical signal reflections.

Early SCSI devices are terminated with resistor packs, long, thin multi-pin yellow or black components near the 50-pin connector. If socketed, these can be removed, or otherwise switched out of the circuit to disable termination.

An incorrectly configured SCSI chain will not operate, or may operate intermittently. It is critical to get SCSI IDs and termination correct.

A SCSI inspector, such as SCSI Probe, will report if a SCSI chain is properly terminated.

The SCSI interface has been improved throughout the years. SCSI-1, the first generation, supports a 5 MB/s theoretical maximum transfer rate. Later, SCSI-2 improved reliability and increased speeds to 10 MB/s. The Macintosh II series used SCSI-1. The Power Macintosh Series used SCSI-2 for its internal SCSI bus.

### <a id="using-installation"></a> Installation

SCSI devices should only be connected or disconnected while the host device is powered off. Power down your device before installation.

These general guides, along with well documented sites describing SCSI configuration, should assist you in configuring your SCSI device chain. We cannot provide specific instructions for each host or device.

If your device does not provide SCSI termination power (such as the Apple Macintosh IIsi) you will need to provide power to the ZuluSCSI. This can be done either with the ZuluSCSI power conector, or by powering the ZuluSCSI from the USB connector.

Be sure to secure the ZuluSCSI properly to a suitable drive mounting bay using the ZuluSCSI mounting bracket. Only after the ZuluSCSI is connected and securely mounted, and your host device is safe to use, should you power on your host device.

### <a id="using-formats"></a> Understanding filesystems

When formatting mass storage devices, such as floppy discs and hard drives, it is done using a specific file system. Different operating systems use different file systems, such as NTFS for Windows and APFS for Macintosh. Other common formats are FAT, exFAT, HFS and HFS+.

The ZuluSCSI allows the use of modern SD storage media in place of a traditional mechanical SCSI drive. When you install a ZuluSCSI in a vintage computer, the computer does not know that it's operating with a modern, solid state drive. When formatting an SD card using a vintage computer, the card will be formatted _using the vintage file system._ In doing so, the SD card may no longer be accessible in modern computers, if the file system is no longer supported.

For example, vintage Macintosh computers used HFS or Hierarchical File System for mass storage devices. This file system was used from 1985 until it was replaced by HFS+ (or HFS Extended) in 1998, with the introduction of Mac OS 8.1. If you use a ZuluSCSI on a vintage Mac and format the SD card with HFS, you may not be able to access the card's contents using a modern Macintosh. For more information, see [Limitations](#limitations).

<!--
### <a id="using-advanced"></a> Advanced options

[Not yet written]
<!--

## <a id="limitations"></a> Limitations

At present, ZuluSCSI 
-->
