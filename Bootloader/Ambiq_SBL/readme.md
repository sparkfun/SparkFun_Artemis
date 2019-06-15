Artemis Bootloader
==========================

The Artemis module is loaded with two bootloaders: the SBL and ABL. The Ambiq Secure Boot Loader (SBL) resides from 0x00 to 0xC000. This bootloader is physically part of the IC and is configured using the info0 registers. At power up, if the SBL is not activated, it jumps to 0xC000. The SparkFun Artemis Bootloader (ABL) resides at 0xC000 and will broadcast a character at POR. If the correct response is received, the ABL will negotiate a bootload speed, then jump to user code starting at 0x10000. The ABL times out after 50ms.

This directory contains the various binaries that are programmed onto each Artemis module:

* info0_artemis_47_115200.bin - Sets the SBL to 115200bps with boot pin on 47
* Artemis_Bootloader_v10.bin - the ABL, starts at 0xC000
* example1_blink.ino.bin - An example sketch to blink the LED. This shows code has been loaded but will be overwritten with new sketches.

For more information on the Artemis Bootloader see the Artemis Bootloader sketch in the SparkFun_BL directory in this repo.

The SBL is configured based on the documents from Ambiq. See \AmbiqSuite-Rel2.x.x\docs\secure_bootloader\ for how to modify the SBL/info0 image. 

* Run the bootloader at 921600bps: --u0 0x1C299c0
* Currently pin 47 is used for BOOTLOAD: --gpio 0x2f
* Bootloader to check if BOOTLOAD pin is high (not low): --gpiolvl 1
* Trim timeout to 2500ms: --wTO 2500

This was used for Artemis v10 module that used pin 47 for boot:

    create_info0.py --valid 1 info0_artemis_47 --pl 1 --u0 0x1C200c0 --u1 0xFFFF3031 --u2 0x2 --u3 0x0 --u4 0x0 --u5 0x0 --main 0xC000 --gpio 0x2f --version 0 --wTO 2500 --gpiolvl 1

We load a conservative 115200bps image into the Ambiq SBL section so that future updates can be applied. The SparkFun Artemis Bootloader is a variable bootloader capable of operating from 9600bps to 921600bps.
