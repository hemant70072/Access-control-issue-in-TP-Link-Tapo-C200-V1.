# Hacking your friend's Router: Access control issue in TP-Link Tapo C200 V1. allowing attackers to gain root access, sensitive information or execute arbitrary command

The TP-Link Tapo C200 V1 has a vulnerability that can be exploited to gain the root shell access of the device which can further be used to gain sensitive information, firmware extraction or execute arbitrary commands.

The steps provided below can be helpful to know the process for finding this vulnerability.

Firmware-extraction-and-root-shell-access-of-Tapo C200 V1-camera


Exploiting the UART shell to get access of the root shell and dumping the content of the flash chip (firmware) in the SD card 


Getting the **U-Boot** shell :
1. Find the UART pins of the device
2. Connect the UART pins to the USB-TTL converter and connect it to the Host PC 
3. Access the data on UART using software like **putty** for Windows or **screen** for Linux
4. Create the putty or screen session by entering the correct baud rate
5. Power on the device and stop the booting of the device by pressing the key when the boot log shows the text **"stop the autobooting in 3s"*

**NOTE:**: The key to stop the autoboot should be known (generally the boot log shows the key to stop the booting)


6. Upon stopping the boot process, the bootloader shell will be accessible


Getting the root shell :


1. Once the U-Boot shell is accessible, change the path of the **init** environment variable to **init=/bin/sh** using **setenve** command (There are many commands available in this shell which can be used to overwrite the memory)
2. Now run the content of **bootcmd** as a command in the shell
3. Upon running the bootcmd the device will boot normally and give the root shell

Extracting the Firmware from the device


1. Now insert the SD card in the device and check whether the SD card is detected using the command **cat /proc/partitions** (SD card will detected as **mmcblk0**)
2. Find the mtd block partitions of the flash chip using the command **cat /proc/mtd**
3. Finally copy the content of each mtd block in the SD card using the command **cat /dev/mtdblock0 >> mmcblk0/flashdump.bin**



**NOTE:** Run the above command for each mtdblock and the complete content of the flash chip is now dumped to the SD card and can be used for further exploitation
