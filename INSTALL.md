# OrangePiPcPlusArmbian
Orange PI Pc Plus Armbian install

## Prepare the installation

### Get Armbian image

[Orange Pi Pc+ page on Armbian][1]


fetch Armbian Bionic with mainline kernel 4.19.20 image

### Check sdCard

using [f3][2] tool tocheck we have a real sd card

### Extract archive

```shell
7z -e Armbian_5.75_Orangepipcplus_Ubuntu_bionic_next_4.19.20.7z
```
you will have a file named like: **Armbian_5.75_Orangepipcplus_Ubuntu_bionic_next_4.19.20.img**


Nb: when you extract a .xz file for Amlogic s905
```shell
unxz Armbian_5.76_Aml-s905_Ubuntu_bionic_dev_4.19.21_desktop_20190304.img.xz
```
the file name is now **Armbian_5.76_Aml-s905_Ubuntu_bionic_dev_4.19.21_desktop_20190304.img**


### Check card reader device

```shell
dmesg
```

```
mmc0: new ultra high speed SDR50 SDHC card at address aaaa
mmcblk0: mmc0:aaaa SL16G 14.8 GiB 
mmcblk0: p1
FAT-fs (mmcblk0p1): Volume was not properly unmounted. Some data may be corrupt. Please run fsck.
mmc0: card aaaa removed
mmc0: new ultra high speed SDR50 SDHC card at address aaaa
mmcblk0: mmc0:aaaa SL16G 14.8 GiB
```

The device could be check with tab completion
```shell
ls /dev/mmcblk0
```

### Allow use of dmesg by user

```
dmesg: read kernel buffer failed: Operation not permitted
```
or in french
```
dmesg: échec de lecture du tampon de noyau: Opération non permise
```

```shell
sudo sysctl kernel.dmesg_restrict=0
```
cf: [stackexchange][3]

### write image on sd card

```shell
dd if=Armbian_5.75_Orangepipcplus_Ubuntu_bionic_next_4.19.20.img  of=/dev/mmcblk0  bs=4M status=progress
```

```
947912704 bytes (948 MB, 904 MiB) copied, 73.3012 s, 12.9 MB/s
227+0 records in
227+0 records out
952107008 bytes (952 MB, 908 MiB) copied, 89.5046 s, 10.6 MB/s
```
1mn20 to write image


Compared to image for Aml-s905:
```
2617245696 bytes (2.6 GB, 2.4 GiB) copied, 220.07 s, 11.9 MB/s  
625+0 records in
625+0 records out
2621440000 bytes (2.6 GB, 2.4 GiB) copied, 239.383 s, 11.0 MB/s 
```
Near 4mn to write image

Image should be ready ! :-)

## First start

### Serial connection
We use a USB serial (FTDI), OPi has dedicated pinouts near power jack.

FTDI <---> OPi
GND  <---> GND
RX   <---> TX
TX   <---> RX

Plug the serial adapter, check dev in dmesg, here it is /dev/ttyUSB0,
then using (GNU) screen :

```shell
screen /dev/ttyUSB0 115200
```
Nb: to exit of screen **Ctrl** + **a** 
then **Ctrl** + **k**
and validate with **y**

### Boot Armbian

We had no output on serial, first mistake was buring wrong image ...
Then nothing but OPi with a screen on HDMI start with Android (chinese)
We notice that it don't boot on sd card, because the sd card was absent ...
Once sd card inserted Armbian start and we have access to serial 0o


### First login
login as root with password 1234
```
Ubuntu 18.04.1 LTS orangepipcplus ttyS0

orangepipcplus login: root
Password: 
You are required to change your password immediately (root enforced)
Changing password for root.
(current) UNIX password: 

Authentication token manipulation error

Ubuntu 18.04.1 LTS orangepipcplus ttyS0

orangepipcplus login: root
Password: 
You are required to change your password immediately (root enforced)
Changing password for root.
(current) UNIX password: 
Enter new UNIX password: 
Retype new UNIX password: 
You must choose a longer password
Enter new UNIX password: 
Retype new UNIX password:
```

[1]: https://www.armbian.com/orange-pi-pc-plus/
[2]: http://oss.digirati.com.br/f3/
[3]: https://unix.stackexchange.com/questions/390184/dmesg-read-kernel-buffer-failed-permission-denied
