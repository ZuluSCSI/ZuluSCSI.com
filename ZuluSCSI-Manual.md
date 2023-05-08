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

* [Connectors](#connectors)

  * [SD Card Slot](#connectors-sd-card-slot)
  * [Internal SCSI Header](#connectors-internal-scsi-header)
  * [External SCSI Port](#connectors-external-scsi-port)
  * [Floppy (Berg) Connector](#connectors-floppy-berg-connector)
  * [USB Port](#connectors-usb-port)
  * [LED Header](#connectors-led-header)

## <a id="introduction-to-ZuluSCSI"></a> Introduction to ZuluSCSI

### <a id="introduction-overview"></a> Overview

ZuluSCSI is a SCSI hard drive emulator. Physical disk devices are represented by images stored on a SD card. 

### <a id="introduction-history"></a> ZuluSCSI History & Lineage

In response to ongoing global electronics component shortages, ZuluSCSI development began in November, 2021 by Rabbit Hole Computing. ZuluSCSI is different from other SD card SCSI emulation devices and projects. Unlike with SCSI2SD, no configuration utility is used or necessary. All necessary ZuluSCSI documentation is here.

### <a id="introduction-models"></a> ZuluSCSI Models

| Device version             | Description                                                  |
| :------------------------- | :----------------------------------------------------------- |
| ZuluSCSI RP2040            | A full-sized ZuluSCSI with a 50 pin internal SCSI connector and switch-controlled termination. based on the RP2040 microcontroller. |
| ZuluSCSI RP2040 Mini       | External DB25M-based device in a plastic case, with permanently enabled termination.  Uses a microSD card for storage.  |
| ZuluSCSI RP2040 Compact    | A half-sized ZuluSCSI, with permanently enabled termination.  Uses a microSD card socket for storage.  |
| ZuluSCSI RP2040 Compact Homebrew | A kit-based half-sized ZuluSCSI, with jumper-configurable termination.  Uses a microSD card socket for storage.  |
| ZuluSCSI Mini V1.0         | External DB25M-based device in a plastic case, with permanently enabled termination.  Uses a microSD card for storage.  |
| ZuluSCSI v1.1              | A full-sized ZuluSCSI with a 50 pin internal SCSI connector and switch-controlled termination |
| ZuluSCSI V1.1 2.5" Laptop/PowerBook | A variation of the ZuluSCSI Mini for Apple Macintosh PowerBook laptops. It has a 50 pin internal connector, fixed, permanently enabled SCSI termination, and uses a microSD card for storage. |

### <a id="introduction-features"></a> Features

| Hardware Version/Edition                                 | RP2040    |V1.0       | V1.1      | V1.1 Laptop & RP2040 Laptop | V1.0 Mini & RP2040 Mini (DB25) | RP2040 Compact | RP2040 Compact Homebrew |
| ------------------------------------------------------------ | --------- | --------- | --------- | -------------- |-------------------------|-------------------------|-------------------------|
| DIP-switch/jumper-configurable SCSI termination              | Yes       | Yes       | Yes       | Yes            | Always On      | Yes            | Yes            |
| LED pin header location for _optional_ external LED pin header | Yes     | Yes       | Yes       | Yes            | No             | Yes            ||Yes            |
| SD Card Type                                                 | Full Size | Full Size | Full Size | MicroSD        | MicroSD        | MicroSD        | MicroSD        |
| Initiator Mode                                               | Yes     | Not supported | Not supported | Not supported | Not supported  | Not supported | Yes        |


All ZuluSCSI devices are compatible with a wide range of SCSI enabled computers and devices, and are capable of emulating a range of different devices, including hard drives, CD-ROMs, SCSI floppy, and removable media. All models can operate solely on power from the SCSI connector in most instances, when SCSI termination power is provided. 

## <a id="configuration"></a> ZuluSCSI Configuration

Configure ZuluSCSI using files on a FAT32 or exFAT-formatted SD or microSD card.

ZuluSCSI searches the SD card for files with a specific filename structure, for files ending in `.img`, `.hda`, or `.iso` format.

ZuluSCSI searches for files named `HDn.img` or `HDn.hda`, where 'n' is a unique SCSI ID (0-7). Note: SCSI ID 7 is often reserved for the controller. Each file with a unique SCSI ID will be presented to the host controller as a SCSI drive.

ZuluSCSI can be configured in more detail via an optional text file named zuluscsi.ini. The [example file](https://github.com/ZuluSCSI/ZuluSCSI-firmware/blob/main/zuluscsi.ini) shows how to configure ZuluSCSI via the `zuluscsi.ini` file.

At initialization, ZuluSCSI logs details and detected image files to `zululog.txt`

### <a id="firmware-update"></a> Updating firmware

Copy the new firmware file named `zuluSCSIxxx.bin` (where xxx is the version) to the root directory of an SD card, then apply power to the ZuluSCSI. The ZuluSCSI will self update. For approximately one second, the LED will flash rapidly.

When the update is complete, ZuluSCSI will delete the firmware file from the SD card. It will automatically reboot, using the new firmware.

### <a id="dip-switches"></a> Configuration switch settings

ZuluSCSI RP2040 and V1.1 include a four-position DIP switch. By default, TERM (termination) is on, and all others are off.

### TERM

Termination should remain on if ZuluSCSI is the last physical device on the SCSI bus.

### BOOT

The BOOT/BOOTLOADER DIP switch is primarily meant for firmware recovery, and must not be enabled for normal operation.
When enabled, on blue ZuluSCSI V1.x boards, the DFU bootloader is executed instead of the normal ZuluSCSI firmware. 
When enabled, on red ZuluSCSI RP2040-based boards, the ROM-based UF2 bootloader is executed instead of the normal ZuluSCSI firmware. 


### <a id="troubleshooting"></a>Troubleshooting

The LED indicator normally flashes to indicate disk activity.

Upon initial power-on, the activity LED also reports following status conditions:

- 1 fast blink on boot: Firmware successfully loaded, and at least one image file loaded successfully
- 3 fast blinks: No images found on SD card, and then automatically falls back to raw passthrough mode.
- 5 fast blinks: SD card not detected
- Continuous morse pattern: firmware crashed, morse code indicates crash location

If a crash occurrs, the firmware will also attempt to save information to a separate `zuluerr.txt` log file, on the root of the SD card.

No LED activity: verify the 'BOOT' configuration switch is OFF.

## <a id="using-the-ZuluSCSI"></a> Using your ZuluSCSI

## <a id="SD-card-requirements"></a> SD card requirements

 ZuluSCSI firmware requires a MBR/DOS-partitioned SD card, which must be labeled SDHC or SDXC, and at least one FAT32 or exFAT-formatted SD partition. With blue ZuluSCSI V1.x boards, Older SD cards manufactured prior to around 2008, generally ~4GB and smaller, may not be detected by the SDIO interface of the microcontroller. This is a hardware/silicon limitation. ZuluSCSI has no upper limit on the size of SD cards it is compatible with, and is known to work with large 256GB and 400GB SD cards which are currently commercially available. SD cards partitioned with GPT (GUID Partition Map) can not be read by ZuluSCSI, and the ZuluSCSI will not detect such SD cards, resulting in the LED flashing five times.

### <a id="using-quickstart"></a> Quickstart guide

These steps get your ZuluSCSI up and running quickly, emulatiing a single SCSI hard drive.

Create or copy a single (or up to six), valid disk image file to the root directory of an MBR/DOS-partitioned FAT32 or exFAT-formatted SD card of any capacity. Name the image "HDx.img" or "HDx.hda", where 'x' is a unique SCSI ID, between 0 and 6 (ID 7 is often the controller's ID).

Image files named "CDx.iso" or "CDx.img" will automatically be configured by the ZuluSCSI firmware as a removable SCSI CD-ROM drive, with a standard 2048-byte sector size.  

To use your ZuluSCSI with the original, un-patched **Apple HD SC Setup** or **Drive Setup** formatting utility included with earlier Macintosh operating systems, you must enable Apple quirks mode. 

With blue-colored full-sized ZuluSCSI V1.1, you can switch the SW1 DIP switch (closest to the SD card socket) ON. This switch changes the SCSI product & manufacturer IDs to values that Apple's HD SC Setup utility has whitelisted as supported. 
With red-colored ZuluSCSI RP2040-based boards, you must set the "System=Mac" _or_ "System=MacPlus" parameter in the [SCSI] section of your zuluscsi.ini file:

    [SCSI]
    System=Mac

If the SW1 switch on a ZuluSCSI V1.x is not in the on position (or, alternatively, Quirks=1 or System=Mac set in zuluscsi.ini), HD SC Setup will not recognise the ZuluSCSI as a suitable/usable SCSI hard drive due to its [limitations](#limitations-formatting). With SW1 off, you would need to use a third-party hard drive formatting utility, such as **Hard Disk Toolkit**, **Silverlining** or **Lido**. There is also a patched version of the **Apple HD SC Setup** utility available with the Apple-imposed limitations removed.

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

## <a id="connectors"></a> Connectors

### <a id="connectors-sd-card-slot"></a> SD Card Slot

The full-size version of ZuluSCSI has a full-size SD card slot. The compact, laptop, and mini versions of ZuluSCSI have MicroSD card slots. All versions support SD, SDHC, and SDXC cards.

### <a id="connectors-internal-scsi-header"></a> Internal SCSI Header

The full-size, compact, and laptop versions of ZuluSCSI have a 50-pin internal SCSI connector. The internal SCSI connector is typically connected by a ribbon cable to a corresponding pin header on the motherboard.

### <a id="connectors-external-scsi-port"></a> External SCSI Port

The mini version of ZuluSCSI has a DB-25 male external SCSI connector. This can be connected directly to a DB-25 female port, as found on the back of vintage Macs from the Macintosh Plus onward.

The full-size version of ZuluSCSI has an unpopulated footprint for a DB-25 female connector. If soldered on, the ZuluSCSI can be connected to the external SCSI port of vintage Macs with a straight-through DB-25 cable.

### <a id="connectors-floppy-berg-connector"></a> Floppy (Berg) Connector

The full-size and compact versions of ZuluSCI have unpopulated footprints for a Floppy-style Berg connector. This can be used to supply +5V power to the ZuluSCSI if the host machine does not provide termination power. The +12V power line is ignored. It is safe (but unnecessary) to supply +5V power through this connector while the ZuluSCSI is receiving power from another source.

### <a id="connectors-usb-port"></a> USB Port

All versions of ZuluSCSI have a Micro-USB Type-B port. This can be used to supply +5V power to the ZuluSCSI if the host machine does not provide termination power. It is safe (but unnecessary) to supply +5V power through this connector while the ZuluSCSI is receiving power from another source.

In addition, the USB port can be used as an alternate method to flash the ZuluSCSI with updated firmware. See the [firmware repository](https://github.com/ZuluSCSI/ZuluSCSI-firmware) for details.

Other than power and firmware updates, no additional functionality is currently provided through the USB port. It would be technically feasible to implement support for USB Mass Storage, but the ZuluSCSI is limited to USB “Full Speed”, and most modern machines would be better served by a High Speed or Super Speed card reader.

### <a id="connectors-led-header"></a> LED Header

The full-size, compact, and laptop versions of ZuluSCSI have an unpopulated footprint for a pin header (J304). A LED can be connected to that header as an external activity LED. The cathode (negative) end of the LED (the shorter leg) should be connected to the position marked “C”.

<!--
### <a id="using-advanced"></a> Advanced options

[Not yet written]
<!--

## <a id="limitations"></a> Limitations

At present, ZuluSCSI 
-->
