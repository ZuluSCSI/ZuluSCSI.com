# About ZuluSCSI firmware

The ZuluSCSI firmware looks for file names which adhere to a simple but powerful naming convention, and presents them as the drives to the SCSI host. Each image file represents a SCSI drive.

As with SCSI2SD, ZuluSCSI is designed to power itself solely from SCSI termination power, with no separate power source needed, when the host device/computer provides SCSI termination power. Nearly all desktop Macintosh computers do, as well as the vast majority of other desktop systems with SCSI interfaces.

ZuluSCSI V1.1 includes an optional DB25 pin header for direct installation of an external SCSI connector, in addition to the Single-Ended 50 pin IDC connector. The ZuluSCSI V1.1 shares the exact same dimensions and mounting holes as that of SCSI2SD V5.1, V5.2, and V6, and is therefore compatible with many existing SCSI2SD-V5.1, V5.2, and V6 mounting solutions.

Maximum read speeds with ZuluSCSI V1.1 can be up to 3.9 megabytes/second, and maximum write speeds of ~2.1 megabytes/second are currently implemented. Throughput rates can be affected by particularly slow (eg. 8MHz 68000) CPUs. SCSI controllers can be, and often are, bottlenecks. Since ZuluSCSI uses the same SCSI command handling code as SCSI2SD V6, compatibility with nearly all devices that speak both SCSI-1 and SCSI-2 is extremely high.

Termination is DIP-switch controlled, and firmware updates are as simple as placing a file on a FAT32 or exFAT-formatted SD card, inserting it in the SD card slot, and applying power to the ZuluSCSI. A firmware recovery mechanism is also provided, as a back-up.
