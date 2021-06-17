# NIBE AirSite GreenMaster PLC

Version 0.0 / 2021-03-06 / AW

[toc]

## PLC components

Components are used to divide the PLC application in several parts that can be activated depending on the site. The NIBE AirSite software is divided in one component for each unit (product) plus some basic components

- GM-BASE - Basic functionality such as alarms and temperature control
- GM-IO - Input and outputs on the PLC
- GM-VENT - Basic ventilation common to several products
- GM-XXL - Functionality for XXL units
- GM-A - Functionality for GMA units
- GM-OPT-XXX - Customer specific optional functions such as Chalmers (GM-OPT-CTH)

Components can be activated in the web interface or directly in the PLC. When activated the functions will execute in the PLC and the corresponding parameters will be included in the parameter list in the web interface or a separate menu, see `Parameter access`.

### GM-BASE

#### Alarms

Fire alarm

Fire alarm supply air

Fire damper alarm

External fire alarm (smoke detector)

Fuse alarm

Service alarm smoke detectors

Freeze protection alarm

Temperature control deviation alarm

Reset alarms

### GM-IO

Physical analog and digital inputs and outputs.

### GM-VENT-T

#### Alarms

Supply fan deviation alarm

Supply fan alarm

Supply filter alarm

### GM-VENT-F

#### Alarms

Extract fan deviation alarm

Extract fan alarm

Extract filter alarm

### GM-XXL

#### Alarms

Heating pump alarm

Cooling pump alarm

Heat exchanger efficiency alarm

Defrost alarm

#### Parameters

Enable heating pump alarm

Enable cooling pump alarm

### GM-A

#### Alarms

Heating pump alarm

Cooling pump alarm

Heat exchanger efficiency alarm

Heat exchanger rotation alarm

Defrost alarm

#### Parameters

Enable heating pump alarm

Enable cooling pump alarm

### GM-C-F

#### Alarms

### GM-C-T

#### Alarms

### GM-OPT-CTH

#### Parameters

Heating pump running

## CX8190

### Flash images

https://download.beckhoff.com/download/software/embPC-Control/CX81xx/CX81xx

Note the firmware update procedure in https://download.beckhoff.com/download/software/embPC-Control/CX81xx/CX81xx/_History/readme_if_device_does_not_start.txt

### Remote display

#### CERHOST

Download the CERHOST application from beckhoff.com.

Currently found at https://infosys.beckhoff.com/content/1033/cx8190_hw/Resources/zip/5047075211.zip

Start cerhost.exe and use File/Connect to connect to the device by IP and empty password (default).

![image-20201030001747416](C:\work\AirSite\airsite-web\readme-assets\image-20201030001747416.png)

and the remote device screen will be displayed:

<img src="C:\work\AirSite\airsite-web\readme-assets\image-20201030001324321.png" alt="image-20201030001324321" style="zoom:50%;" />			

#### Open File Manager

Start / Run and enter a `/` or some other path such as `"\Hard Disk"` (citation marks required when there is a space in the path.

#### Open Registry Editor

Start / Run and enter `regedit` .

#### Open Command Prompt

Start / Run and enter `cmd` .

#### Configure CX8190 firewall

Open port 80 (tcp) for HTTP access in addition to HTTPS at port 443 (open by default

Open ports 20 and 21 (tcp) for FTP access.

### SD-card

#### Linux

List available disks: sudo fdisk -l

Read SD-card to image file: sudo dd if=/dev/sdd of=~/cx8190.img

Check if any mounts before writing to SD card: sudo mount | grep sdd

Remove mount: sudo umount /dev/sdd1

Write image file to SD card: sudo dd if=~/cx8190.img of=/dev/sdd

#### Mac

https://www.raspberrypi.org/documentation/installation/installing-images/mac.md

#### Windows

Save and restore image files: PassMark imageUSB (free)

Expand partitions: EaseUS Partition Master Free

NOTE: Some tools that can expand partition size changes from FAT16 to FAT32. This may not work in the CX8190.

### KL6041 Modbus RTU configuration

Select the PLC Box in TwinCAT:

![image-20201030151409687](C:\work\AirSite\gm\readme-assets\image-20201030151409687.png)

Right-click and select "Register Access..."

In the Register Access dialog select `Terminal ModBus RTU (KL6041)`

<img src="C:\work\AirSite\gm\readme-assets\image-20201030151228317.png" alt="image-20201030151228317" style="zoom:80%;" />

- Set register 32 to dec 7 (baudrate 19200)
- Set register 33 to dec 4 (8 bits, even parity)
- Set register 34 to  dec 385 (half duplex mode)
- Click the Write button to save the changes to the KL6041



### CX8190 config web page

https://<ip>/config

or (with https disabled)

http://<ip>/config

User/password may be:

1. webguest/1
2. webguest/webguest
3. Administrator/1

### Install TcAdsWebService

Copy the files in C:\TwinCAT\AdsApi\TcAdsWebService\V100\ce\ARMV4 from a computer with TwinCAT 3.1 installed to the CX8190 flash at "Hard Disk\www\TcAdsWebService".

Verify that it works by browsing to http://192.168.2.225/TcAdsWebService/TcAdsWebService.dll or https://192.168.2.225/TcAdsWebService/TcAdsWebService.dll when using HTTPS. The following should be displayed:

![image-20210129185206065](C:\work\AirSite\gm\README-images\image-20210129185206065.png)

### Modbus TCP

#### Install Windows driver

Download TF6250 from beckhoff.com.

![image-20201010162148202](C:\work\AirSite\airsite-web\readme-assets\image-20201010162148202.png)

#### Install Windows CE driver in CX8190

![image-20201010162948876](C:\work\AirSite\airsite-web\readme-assets\image-20201010162948876.png)

FTP to some folder in CX8190.

Use cerhost and "run \" to open a file manager. Double-click the cab-file.  Install at default location. Reboot device.

#### Modbus TCP tools

- modpoll command line utility

- modscan32 windows client

- modsim32 windows server

#### Add TwinCAT 3 license

![image-20201010163315387](C:\work\AirSite\airsite-web\readme-assets\image-20201010163315387.png)

Generate trial licenses or get valid license.

### FTP

FileZilla setup:

![image-20210119234531701](C:\work\AirSite\gm\README-images\image-20210119234531701.png)



![image-20210119234622665](C:\work\AirSite\gm\README-images\image-20210119234622665.png)



![image-20210119234721920](C:\work\AirSite\gm\README-images\image-20210119234721920.png)



Open ports 4000 to 4100 in your PC firewall.

### HTTP or HTTPS

HTTPS is active by default. Open port 80 in the CX8190 firewall to use HTTP.

### CX8190 Firmware

https://download.beckhoff.com/download/software/embPC-Control/CX81xx/CX81xx