**ZuluSCSI FAQ**

>What is the difference between 1.1 and 2040?

The microcontroller itself is the difference. The V1.x ZuluSCSI designs are based on a Chinese-manufactured STM32 clone, the GigaDevice GD32F205. This microcontroller is not readily available through any standard electronics distributors in the United States, and can only be special-ordered. It also costs more to buy, and the lead times are 6-8 weeks. The ZuluSCSI RP2040 is designed around the RP2040 microcontroller, a dual-core 133MHz Cortex M0+ MCU, which was designed by the Raspberry Pi Foundation, and released publicly in January of 2021.

Immediately after we finished the design of the ZuluSCSI V1.1 in January of this year, we began design of the ZuluSCSI RP2040. Back then, we had no way of knowing how it would perform, and (incorrectly, as it turned out) we assumed it would not perform as speedily as ZuluSCSI V1.1. ZuluSCSI RP2040 therefore began life as an experimental, back-burner "Plan B" project. We didn't even know if it would see the light of day, as a commercial product, back then.

At the beginning of May, we received our first ZuluSCSI RP2040 engineering samples, and porting of the ZuluSCSI firmware itself began in earnest. A week later, we had the ZuluSCSI RP2040 minimally working as a SCSI device, albeit relatively slowly, around one megabyte per second, with no hardware acceleration.

By the end of August, we got the read performance slightly above that of ZuluSCSI V1.1 in asynchronous SCSI mode, but had not yet implemented synchronous SCSI support. By mid-september, we got synchronous SCSI support implemented in the ZuluSCSI RP2040, and the read performance is nothing short of astonishing. With a sufficiently fast bus and SCSI controller, the ZuluSCSI RP2040 can deliver data from the SD card to the SCSI bus at around 9.5 MB/sec. There were many bugs encountered along the way, and surely some remain, but we've made huge progress in addressing most of them in the last month+

It's important to point out that the SCSI controllers in most of the early Macintosh computers do not support synchronous SCSI, at the silicon level, and even the models that do mostly only support synchronous mode at 5 megabytes/sec, and not the faster 10 megabytes/second synchronous SCSI speed. In those cases, the bottleneck is the integrated SCSI controller in the Macintosh itself, not the ZuluSCSI.

As of late OCtober 2022, we're still working on improving write performance with ZuluSCSI RP2040, which is currently in the 2-3 megabyte/second range, depending on transfer size, and those improvements will be released when they're sufficiently tested and ready. There's no fixed timetable.
