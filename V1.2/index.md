<img src="/assets/img/ZuluSCSI-V1.2-2023c-TopDown-QuarterSize.jpg" alt="ZuluSCSI V1.2 PCB" width="733" height="770">

<a href="https://github.com/ZuluSCSI/ZuluSCSI-firmware/wiki/ZuluSCSI-V1.2">ZuluSCSI V1.2 specific documentation can be found here</a>

ZuluSCSI™ V1.2 is a SCSI computer storage emulation device where the device type and SCSI ID can be physically configured using DIP switches 

## Features

* [Open-source firmware](https://github.com/zuluscsi/zuluscsi-firmware), licensed under the GPLv3
* Emulates up to 7 SCSI devices simultaneously, including CD-ROM, Magneto Optical, removable (SyQuest/Jaz-style), and SCSI floppy device types
* Speaks both SCSI-1 and SCSI-2, including 10MB/sec Fast SCSI
* Up to 8.3 MB/sec. read and 3.5 MB/sec. write speeds
* SCSI Termination is DIP-switch controlled.
* Firmware upgrade simplicity; As easy as copying a .bin file file to your SD card.
* External activity LED pin header for attaching remote activity LED
* Designed to be powered via SCSI termination power, when provided by the host
* Identical dimensions and mounting holes as that of ZuluSCSI RP2040 Full Size, SCSI2SD V6, SCSI2SD V5.2/V5.1
