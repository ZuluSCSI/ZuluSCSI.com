The correct, default jumper configuration for a newly-assembled ZuluSCSI Compact Homebrew should be:

* TERM_EN (Termination Enable) jumper must be ON, unless the ZuluSCSI is NOT at the end of a SCSI chain. If no other devices are on the SCSI bus besides your ZuluSCSI, TERM_EN must be jumpered ON.
* Initiator jumper must be OFF. If the Initiator jumper is closed/on, the ZuluSCSI will not act as a hard drive, but will instead act as a SCSI initiator, looking for SCSI devices on the bus. If you want the ZuluSCSI to act as a standard hard drive, the initiator jumper MUST be left open/off.
