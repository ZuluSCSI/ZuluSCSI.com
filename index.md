<img src="assets/img/ZuluSCSI-Blaster-Rev2025e-iso.jpg" alt="ZuluSCSI Blaster PCB">


ZuluSCSI® is a SCSI computer storage emulation platform, which speaks both SCSI-1 and SCSI-2. It uses file-based SCSI HDD & CD-ROM images. New for 2025, **ZuluSCSI Blaster** is Powered by Raspberry Pi, and leverages the new RP2350B, which includes additional I/O capabilities, beyond what was possible with the previous generations of our products. Hard drive and CD-ROM drive images are stored on a standard FAT32 or exFAT-formatted SD card, and are exposed as block devices to the operating system.

## Features of the new ZuluSCSI RP2350B

* Emulates up to 7 SCSI devices simultaneously, including CD-ROM, Magneto Optical, removable (SyQuest/Jaz-style), and SCSI floppy device types
* Speaks both SCSI-1 and SCSI-2, including 20MB/sec Fast SCSI and Ultra SCSI
* Up to **18 megabytes/second** read speeds, 10MB/sec write speeds (on ZuluSCSI Blaster-based models)
* SCSI Termination is DIP-switch controlled
* Firmware upgrade simplicity; As easy as placing a file on the SD card
* Highly configurable using a text-based ini file, ZuluSCSI.ini
* External activity LED pin header for attaching remote activity LED
* Designed to be powered via SCSI termination power when provided by the host
* Identical dimensions and mounting holes as that of SCSI2SD V5.1, V5.2, and V6, making it compatible with many existing SCSI2SD-specific mounting solutions

### Origins and License

#### Firmware origins

The open source ZuluSCSI™ firmware](https://github.com/zuluscsi/zuluscsi-firmware), licensed under the GPLv3 is derived from two sources, both under GPL 3 license:

* [SCSI2SD V6](http://www.codesrc.com/gitweb/index.cgi?p=SCSI2SD-V6.git;a=summary)
* [BlueSCSI](https://github.com/erichelgeson/BlueSCSI), which in turn is derived from [ArdSCSIno-stm32](https://github.com/ztto/ArdSCSino-stm32).

#### Hardware 
The ZuluSCSI™ Boaster , V1.0 and V1.1 hardware designs are derived from the SCSI2SD V5.1 hardware design.
The ZuluSCSI™ RP2040 Mini hardware designs are derived from the SCSI2SD V5.5 hardware design.
