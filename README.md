# Dfu-util - Device Firmware Upgrade Utilities

Dfu-util is a host side implementation of the DFU 1.0 and DFU 1.1 [1]
specification of the USB forum.

DFU is intended to download and upload firmware to devices connected over
USB. It ranges from small devices like micro-controller boards up to mobile
phones and single-board computers. dfu-util is able to download firmware
to devices as well as to upload firmware from them.

dfu-util has been tested with Openmoko Neo1973 and Freerunner and many
other devices.

The official website is: http://dfu-util.sourceforge.net

See the INSTALLATION section for build/install instructions.

DFU 1.1 specification: https://www.usb.org/sites/default/files/DFU_1.1.pdf

## INSTALLATION

Generate Build system:
````shell
cmake -DCMAKE_BUILD_TYPE=Release -DCMAKE_INSTALL_PREFIX=... ./ -B cmake-build-rel
````
The `-DCMAKE_INSTALL_PREFIX=...` parameter can be omitted to install into default location.

Build executables:
```shell
cmake --build cmake-build-rel -j3
```

Install executables and manual pages (use `sudo` if installing into system directory):
```shell
cmake --build cmake-build-rel --target install
```

## List of supported software and hardware products:

Software user (bootloader, etc):
- Sam7DFU: http://git.osmocom.org/openpcd/
- U-boot: http://www.denx.de/wiki/U-Boot/
- Barebox: http://www.barebox.org/
- Leaflabs: https://www.leaflabs.com/maple
- Blackmagic: https://github.com/blacksphere/blackmagic/wiki

Products using DFU:
- OpenPCD (sam7dfu)
- Openmoko Neo 1973 and Freerunner (u-boot with DFU patches)
- Leaflabs Maple
- ATUSB from Qi Hardware
- STM32F105/7, STM32F2/F3/F4 in System Bootloader
- Black Magic Probe debugger
- NXP LPC31xx/LPC43XX, e.g. LPC-Link and LPC-Link2, need binaries
  with LPC prefix and encoding (LPC-Link)

## Tested Devices
qi-hardware-atusb:
- Qi Hardware ben-wpan
- DFU implementation:
  http://projects.qi-hardware.com/index.php/p/ben-wpan/source/tree/master/atusb/fw/usb
- Tester: Stefan Schmidt

openpcd:
- OpenPCD RFID reader
- DFU implementation: SAM7DFU
  http://www.openpcd.org/Sam7dfu, git://git.gnumonks.org/openpcd.git
- Tester: Stefan Schmidt

simtrace:
- Sysmocom SimTrace
- DFU implementation: SAM7DFU
  http://www.openpcd.org/Sam7dfu, git://git.gnumonks.org/openpcd.git
- Tester: Stefan Schmidt

openmoko-freerunner:
- Openmoko Freerunner
- DFU implementation: Old U-Boot
  http://git.openmoko.org/?p=u-boot.git;a=shortlog;h=refs/heads/mokopatches
- Tester: Stefan Schmidt

openmoko-neo1973:
- Openmoko Neo1073
- DFU implementation: Old U-Boot
  http://git.openmoko.org/?p=u-boot.git;a=shortlog;h=refs/heads/mokopatches
- Tester: Stefan Schmidt

tdk-bluetooth:
- TDK Corp. Bluetooth Adapter
- DFU implementation: closed soure
- Only upload has been tested
- Tester: Stefan Schmidt

stm32f107:
- STM32 microcontrollers with built-in (ROM) DFU loader
- DFU implementation: Closed source but probably similar to the one
  in their USB device libraries. Some relevant application notes:
  http://www.st.com -> AN3156 and AN2606
- Tested by Uwe Bonnes

stm32f4discovery:
- STM32 microcontroller board with built-in (ROM) DFU loader
- DFU implementation: Closed source, probably similar to stm32f107.
- Tested by Joe Rothweiler

dso-nano:
- DSO Nano pocket oscilloscope
- DFU implementation: Based on ST Microelectronics USB FS Library 1.0
  http://dsonano.googlecode.com/files/DS0201_OpenSource.rar
- Tester: Tormod Volden

opc-20:
- Custom devices based on STM32F1xx
- DFU implementation: ST Microelectronics USB FS Device Library 3.1.0
  http://www.st.com -> um0424.zip
- Tester: Tormod Volden

lpc-link, lpclink2:
- NXP LPCXpresso debug adapters
- Proprietary DFU implementation, uses special download files with
  LPC prefix and encoding of the target firmware code
- Tested by Uwe Bonnes

Adding the lsusb output and a download log of your device here helps
us to avoid regressions for hardware we cannot test while working on
the code. To extract the lsusb output use this command:
```shell
sudo lsusb -v -d $USBID > $DEVICE.lsusb
```
Prepare a description snippet as above, and send it to us. A log
(copy-paste of the command window) of a firmware download is also
nice, please use the double verbose option -v -v and include the
command line in the log file.


## Authors ordered by first contribution

Harald Welte  
Werner Almesberger  
Michael Lauer  
Jim Huang  
Stefan Schmidt  
Daniel Willmann  
Mike Frysinger  
Uwe Hermann  
C. Scott Ananian  
Tormod Volden  
Bernard Blackham  
Holger Freyther  
Marc Singer  
James Perkins  
Tommi Keisala  
Pascal Schweizer  
Bradley Scott  
Uwe Bonnes  
Andrey Smirnov  
Jussi Timperi  
Hans Petter Selasky  
Bo Shen  
Henrique de Almeida Mendonca  
Bernd Krumboeck  
Dennis Meier  
Veli-Pekka Peltola  
Dave Hylands  
Michael Grzeschik  
Paul Fertser  
Dirk Castelijns  
Gordon McNab  
Stefan Zehl  
Timo Poikola  
Michael Everitt  
David G. Turner  
Geoffrey Hausheer  
Thilo Cestonaro  
Alex Mastro  
Colin Parker  
Xavier Domont  
Ievgenii Meshcheriakov  
Aleks Chakin  
Niels Skou Olsen  
Jörg Riechardt  
Thomas Hebb  
Jérôme Hamm  
Hendry Kaak  