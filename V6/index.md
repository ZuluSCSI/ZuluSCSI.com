<img src="/assets/img/ZuluSCSI_V6.4-Rev2024a.jpg" alt="ZuluSCSI V6.4 PCB" width="733" height="770">


ZuluSCSI™ V6.4 is a SCSI computer storage emulation platform which is a descendent of SCSI2SD V6. Unlike ZuluSCSI V1 and ZuluSCSI RP2040, ZuluSCSI V6.4 uses a client-side configuration utility, ZuluSCI-V6-util, to configure SCSI devices, identical to that of SCSI2SD V6. 

## Features

* [Open-source firmware](https://github.com/zuluscsi/zuluscsi-firmware), licensed under the GPLv3
* Emulates up to 7 SCSI devices simultaneously, including CD-ROM, Magneto Optical, removable (SyQuest/Jaz-style), and SCSI floppy device types
* Speaks both SCSI-1 and SCSI-2, including 10MB/sec Fast SCSI
* Up to 9.5 megabytes/second read AND write speeds
* SCSI Termination is software-controlled, via ZuluSCSI-V6-util.exe
* Firmware upgrade simplicity; As easy as copying a .uf2 file to the ZuluSCSI via USB.
* External activity LED pin header for attaching remote activity LED
* Designed to be powered via SCSI termination power, when provided by the host
* Identical dimensions and mounting holes as that of SCSI2SD V6, V5.2, ZuluSCSI V1.1, and ZuluSCSI RP2040 Full Size

### Origins and License

#### Firmware origins

The [ZuluSCSI™ V6.4 firmware](https://github.com/rabbitholecomputing.com/ZuluSCSI-V6-firmware) is derived from the original [SCSI2SD V6](http://www.codesrc.com/gitweb/index.cgi?p=SCSI2SD-V6.git;a=summary) firmware.

