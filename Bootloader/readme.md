Artemis Bootloader
==========================

The Artemis module is loaded with two bootloaders: the ASB and SVL. The Ambiq Secure Boot Loader (ASB) resides from 0x00 to 0xC000. This bootloader is physically part of the IC and is configured using the info0 registers. At power up, if the ASB is not activated, it jumps to 0xC000. The SparkFun Variable Bootloader (SVL) resides at 0xC000 and will wait for an incoming serial character. If a character is received, the baud rate will be auto-detected, the SVL will load new code, then jump to the new user code starting at 0x10000. The SVL times out after 50ms.

This directory contains the various binaries that are programmed onto each Artemis module:

* info0_artemis_47_115200.bin - Sets the Ambiq Secure Bootloader (ASB) to 115200bps with boot pin on 47
* artemis_svl.bin - the SparkFun Variable Loader (SVL), starts at 0xC000
* example1_blink.ino.bin - An example sketch to blink the LED. This shows code has been loaded but will be overwritten with new sketches.

The code for the bootloader can be found in the [Apollo3 BSP](https://github.com/sparkfun/SparkFun_Apollo3_AmbiqSuite_BSPs/tree/master/common/examples/artemis_svl).

The bootloader for the Artemis can be upgraded via Arduino using [these instructions](https://learn.sparkfun.com/tutorials/designing-with-the-sparkfun-artemis/all#troubleshooting). It can also be upgraded using the [Artemis Firmware Upload GUI](https://github.com/sparkfun/Artemis-Firmware-Upload-GUI).

The ASB is configured based on the documents from Ambiq (Ambiq calls it the secure bootloader or SBL). See \AmbiqSuite-Rel2.x.x\docs\secure_bootloader\ for how to modify the info0 image. 

* Run the bootloader at 921600bps: --u0 0x1C299c0
* Currently pin 47 is used for BOOTLOAD: --gpio 0x2f
* Bootloader to check if BOOTLOAD pin is high (not low): --gpiolvl 1
* Trim timeout to 2500ms: --wTO 2500

This was used for Artemis v10 module that used pin 47 for boot:

    create_info0.py --valid 1 info0_artemis_47 --pl 1 --u0 0x1C200c0 --u1 0xFFFF3031 --u2 0x2 --u3 0x0 --u4 0x0 --u5 0x0 --main 0xC000 --gpio 0x2f --version 0 --wTO 2500 --gpiolvl 1

We load a conservative 115200bps image into the Ambiq ASB section so that future updates can be applied. The SparkFun Variable Loader is a variable bootloader capable of operating from 115200bps to 921600bps.
