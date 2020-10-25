# GPD-Win-Max-Hackintosh
Hello, everyone. This is a repository discussing how to install Mac OS Catalina (or newer version) on GPD Win Max.

Thanks to all the volunteers who participated in this project. We have achieved the minimal requirement to use Mac OS on this little machine.

However, there's still some difficult problem need to be solved. We are currently trying to solve them in our leisure time. The progress may very slow. If you are also interested, join us to solve them together.

# WinMax running Cataline 10.5.7
![image](https://github.com/kingo132/GPD-Win-Max-Hackintosh/blob/main/photos/screenshot.png)

# What's working
* All USB port works include two type-c port. (TODO: Need to test if USB3 works)
* Both two type-c ports can connect to external type-c to DP or HDMI monitors. I have tested BQNQ 4K monitor without a problem.
* Joystick emulated mouse works.
* Gigabyte ethernet port works.
* The native keyboard works.
* The Intel Iris integrated graphic card works, and the screen can be rotated 270 degrees by using ScreedResX.
# What's working but have flaws 
* Wifi can be driven by itlwm kext driver, the speed is tested at 20Mbps. However, the itlwm driver may fail to load occasionally at startup. I have checked the boot log and found nothing. The itlwm is just waiting for the hardware to response but the hardware doesn't give a response. Maybe this is a hardware conflict or the itlwm driver needs to be modified.
* The Bluetooth also stops to load occasionally. Maybe the same cause as Wifi. Need to do more tests. When Bluetooth loads successfully. It works perfectly without a problem.
* The battery information is read but can not read capacity. This is due to the _STA function in SSDT. Need to be fixed.
# What's not working and currently trying to solve
* The touchpad is not working - need to modify SSDT, and also maybe need to develop drivers
* The touch screen is not working - the same as the touchpad, however, someone has already developed a touchscreen driver for GPD P2Max, these two touchscreens is the same.
* There are glitches on the screen when entering desktop and shutting down. Tried many fixes and no avail. Maybe this is an Apple framebuffer driver problem, or display edid problem. Or maybe we need to wait for 1038ng7 to fix this problem.
* Sleep/Hibernate problem - maybe need to modify SSDT code of CPU
* System may freeze during shutdown - may be related to sleep/hibernate problem
* Thunderbolt 3 is not driven - need to modify SSDT and BIOS configuration
* HDMI port is not working - maybe it will not work because Apple does not have an HDMI port in this CPU
* iStat Menu can not read the temperature of CPU


# You may need to modify BIOS settings
Note: We are testing under BIOS version 1.11
## How to modify
* By using ru.efi. I have uploaded my BIOS dump file [winmax_bios_dump.txt](https://github.com/kingo132/GPD-Win-Max-Hackintosh/blob/main/winmax_bios_dump.txt). You can find offset in this file and modify it using ru.efi.
* Use setvar by grub. Check this [tutorial](https://github.com/Azkali/GPD-P2-MAX-Hackintosh/issues/16#issuecomment-565882180)
* Other BIOS modify tools
## The options need to be modified
* cfg-lock: Enable -> Disable
* Vt-d: Enable -> Disable
* Dvmt: 60MB -> 64MB

Other options that the default value is satisfied
* above 4g: default is enable
* csm: default is disabled
* boot mode select: default is efi

# PCI information from ubuntu
```
00:00.0 Host bridge: Intel Corporation Device 8a12 (rev 03)
00:02.0 VGA compatible controller: Intel Corporation Iris Plus Graphics G7 (rev 07)
00:04.0 Signal processing controller: Intel Corporation Device 8a03 (rev 03)
00:07.0 PCI bridge: Intel Corporation Ice Lake Thunderbolt 3 PCI Express Root Port #0 (rev 03)
00:0d.0 USB controller: Intel Corporation Ice Lake Thunderbolt 3 USB Controller (rev 03)
00:0d.2 System peripheral: Intel Corporation Ice Lake Thunderbolt 3 NHI #0 (rev 03)
00:14.0 USB controller: Intel Corporation Ice Lake-LP USB 3.1 xHCI Host Controller (rev 30)
00:14.2 RAM memory: Intel Corporation Device 34ef (rev 30)
00:15.0 Serial bus controller [0c80]: Intel Corporation Ice Lake-LP Serial IO I2C Controller #0 (rev 30)
00:15.1 Serial bus controller [0c80]: Intel Corporation Ice Lake-LP Serial IO I2C Controller #1 (rev 30)
00:16.0 Communication controller: Intel Corporation Management Engine Interface (rev 30)
00:17.0 SATA controller: Intel Corporation Ice Lake-LP SATA Controller [AHCI mode] (rev 30)
00:1c.0 PCI bridge: Intel Corporation Device 34ba (rev 30)
00:1c.6 PCI bridge: Intel Corporation Device 34be (rev 30)
00:1d.0 PCI bridge: Intel Corporation Ice Lake-LP PCI Express Root Port #9 (rev 30)
00:1e.0 Communication controller: Intel Corporation Ice Lake-LP Serial IO UART Controller #0 (rev 30)
00:1e.3 Serial bus controller [0c80]: Intel Corporation Ice Lake-LP Serial IO SPI Controller #1 (rev 30)
00:1f.0 ISA bridge: Intel Corporation Ice Lake-LP LPC Controller (rev 30)
00:1f.3 Audio device: Intel Corporation Smart Sound Technology Audio Controller (rev 30)
00:1f.4 SMBus: Intel Corporation Ice Lake-LP SMBus Controller (rev 30)
00:1f.5 Serial bus controller [0c80]: Intel Corporation Ice Lake-LP SPI Controller (rev 30)
2c:00.0 Network controller: Intel Corporation Wi-Fi 6 AX200 (rev 1a)
2d:00.0 Ethernet controller: Realtek Semiconductor Co., Ltd. RTL8111/8168/8411 PCI Express Gigabit Ethernet Controller (rev 15)
2e:00.0 Non-Volatile memory controller: Sandisk Corp Device 5009 (rev 01)
```

# Integrated display EDID from ubuntu
```
edid-decode (hex):

00 ff ff ff ff ff ff 00 31 d8 00 00 00 00 00 00 
05 16 01 03 6d 14 21 78 ea 5e c0 a4 59 4a 98 25 
20 50 54 00 00 00 45 00 01 01 01 01 01 01 01 01 
01 01 01 01 01 01 c2 1a 20 50 30 00 10 50 10 10 
32 00 d0 4d 01 00 00 1e 00 00 00 ff 00 4c 69 6e 
75 78 20 23 30 0a 20 20 20 20 00 00 00 fd 00 3b 
3d 4c 4e 07 00 0a 20 20 20 20 20 20 00 00 00 fc 
00 38 30 30 78 31 32 38 30 0a 20 20 20 20 00 ea 

----------------

EDID version: 1.3
Manufacturer: LNX Model 0 Serial Number 0
Made in week 5 of 2012
Analog display, Input voltage level: 0.7/0.7 V
Sync: Separate Composite Serration 
Maximum image size: 20 cm x 33 cm
Gamma: 2.20
DPMS levels: Standby Suspend Off
RGB color display
First detailed timing is preferred timing
Color Characteristics
  Red:   0.6416, 0.3486
  Green: 0.2919, 0.5957
  Blue:  0.1474, 0.1250
  White: 0.3125, 0.3281
Established Timings I & II: none
Standard Timings
    800x504    60.001 Hz  16:10   31.320 kHz  31.571 MHz (GTF)
Detailed mode: Clock 68.500 MHz, 208 mm x 333 mm
                800  816  832  880 ( 16  16  48)
               1280 1283 1285 1296 (  3   2  11)
               +hsync +vsync
               VertFreq: 60.062 Hz, HorFreq: 77.841 kHz
Display Product Serial Number: Linux #0
Display Range Limits
  Monitor ranges (GTF): 59-61 Hz V, 76-78 kHz H, max dotclock 70 MHz
Display Product Name: 800x1280
Checksum: 0xea
```
