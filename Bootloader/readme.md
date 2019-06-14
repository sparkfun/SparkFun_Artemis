Artemis Bootloader
==========================

This is created based on the documents from Ambiq. See \AmbiqSuite-Rel2.x.x\docs\secure_bootloader\ for how to create your own image. 

* Run the bootloader at 921600bps: --u0 0xE1000
* Currently pin 47 is used for BOOTLOAD: --gpio 0x2f
* Bootloader to check if BOOTLOAD pin is high (not low): --gpiolvl 1
* Trim timeout to 250ms: --wTO 2500

This was used for Artemis v10 module that used pin 47 for boot:

    create_info0.py --valid 1 info0_artemis_47 --pl 1 --u0 0xE1000c0 --u1 0xFFFF3031 --u2 0x2 --u3 0x0 --u4 0x0 --u5 0x0 --main 0xC000 --gpio 0x2f --version 0 --wTO 2500 --gpiolvl 1

Create 115200bps bootloader:

    create_info0.py --valid 1 info0_artemis_47 --pl 1 --u0 0x1C200c0 --u1 0xFFFF3031 --u2 0x2 --u3 0x0 --u4 0x0 --u5 0x0 --main 0xC000 --gpio 0x2f --version 0 --wTO 2500 --gpiolvl 1

We have pre-compiled the fastest possible bootloader (921600bps) as well as the standard 115200bps. To program new bootloader: call program_xxxx.bat from command line with JLINK attached and Artemis powered from separate 3V supply.

Note: We have experienced issues when the timeout was set too short (for example 250ms). Bootloading would work at 921600 but fail at 115200. We're not sure how the SBL works but by increasing timeout to 2.5s bootloading works reliably at 115200bps.