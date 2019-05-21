Artemis Bootloader
==========================

This is created based on the documents from Ambiq. See \AmbiqSuite-Rel2.x.x\docs\secure_bootloader\ for how to create your own image. 

* Run the bootloader at 921600bps: --u0 0xE1000
* Currently pin 47 is used for BOOTLOAD: --gpio 0x2f
* Bootloader to check if BOOTLOAD pin is high (not low): --gpiolvl 1
* Trim timeout to 250ms: --wTO 250

This was used for Artemis v10 module that used pin 47 for boot:

    create_info0.py --valid 1 info0_artemis_47 --pl 1 --u0 0xE1000c0 --u1 0xFFFF3031 --u2 0x2 --u3 0x0 --u4 0x0 --u5 0x0 --main 0xC000 --gpio 0x2f --version 0 --wTO 250 --gpiolvl 1

To program new bootloader: call program_info0.bat from command line (make sure jlink-prog-info0.txt is pointing at the bin you want) with JLINK attached and Artemis powered from separate 3V supply.