# Pine64 Configurations

This configuration package series configures Buildroot to produce a BSP image for the
PinePhone, PineBook, PineCube, and other Pine64 based devices.

There are specific configurations for each board, see "Board Compatibility."

References:

 - https://linux-sunxi.org/Pine_Pinebook
 - https://linux-sunxi.org/PinePhone
 - https://xnux.eu/devices/pine64-pinephone.html

# Board Compatibility

There are specific packages tuned to each model:

| **Board**       | **Config Package** |
| --------------- | -----------------  |
| [H64]           | [pine64/h64]       |
| [PineBook]      | [pine64/book]      |
| [PinePhone]     | [pine64/phone]     |
| [RockPro64]     | [pine64/rockpro64] |

[H64]: https://www.pine64.org/pine-h64-ver-b/
[PineBook]: https://www.pine64.org/pinebook-pro/
[PinePhone]: https://www.pine64.org/pinephone/
[RockPro64]: https://www.pine64.org/rockpro64/
[pine64/h64]: ./h64
[pine64/book]: ./book
[pine64/phone]: ./phone
[pine64/rockpro64]: ./rockpro64

# Flashing

Skiff is easiest installed to a SD card. A tool can be used to flash the OS to
the internal EMMC once booted to the SD card. The Pine64 system will boot from the
SD card if it is present and contains u-boot.

These commands require root and may need to be run with `sudo bash`.

```
export SKIFF_WORKSPACE=myworkspace
export PINE64_SD=/dev/sdx # make sure this is correct - i.e. /dev/sdb
make cmd/pine64/common/format
make cmd/pine64/common/install
```

The "format" command creates the partition layout and installs u-boot. This only
needs to be run once. The "install" command copies the latest Image, dtb, boot
script, initramfs, and modules image to the boot and rootfs partitions. The root
system can be updated without touching the "persist" partition by running
"install" again whenever necessary.





# Updating ModemFirmware

`ssh core@192.168.xxx.xxx`

### step 1) Unlock ADB access

It's possible to access the Linux side of the modem via adb, or reboot the modem to fastboot mode and boot your own kernel, The modem is rooted by default, and you can install and run your own software inside the modem. It's possible to communicate between A64 and the modem's ARM CPU via USB serial port (ttyGS0 on modem side and ttyUSB1 on A64).

#### 1.1) Get adb key
Add the file linked below to your home dir (It doesn't really matter what dir). The original file is located here: https://xnux.eu/devices/feature/qadbkey-unlock.c .

Compile with `$ gcc -o qadbkey-unlock qadbkey-unlock.c -lcrypt`
Execute with `$ ./qadbkey-unlock AT+QADBKEY?`

#### 1.2) Install adb for gentoo

ADB stands for Android Debug Bridge, and it is a part of the Android Software Development Kit (SDK). It can be installed with
```
$ sudo emerge --ask --autounmask=y --autounmask-write dev-util/android-sdk-update-manager
$ sudo emerge --ask dev-util/android-sdk-update-manager
```

Emerge may ask you to install "sys-libs/ncurses-compat", it may even tell you, that no ebuilds could be found to satisfy this dependency. You can run: `sudo emerge --ask sys-libs/ncurses-compat` to merge it into your system.

Emerge may ask you to install "dev-java/swt". You can do that by
```
$ sudo emerge --ask --autounmask=y --autounmask-write dev-java/swt
$ sudo emerge --ask dev-java/swt
```

While figuring this out, I ran into a problem with installing "dev-java/swt". It stated:
```
ERROR: dev-java/swt-4.10-r2::gentoo failed (unpack phase):
 *   Unable to extract distfile

```

After that, redo the step to install "dev-util/android-sdk-update-manager"




### 1.2) Install adb for gentoo

ADB stands for Android Debug Bridge, and it is a part of the Android Software Development Kit (SDK). It has to be build from source, because the version that is provided in the gentoo repo is amd64 only. You can clone this repo from github:
```
$ git clone https://github.com/qhuyduong/arm_adb.git
```

Next, you'll have to merge automake into your system, this can be done with emerge:
```
$ sudo emerge --ask automake
```

After that, you'll have to merge libtool into your system. This is installed with:
```
$ sudo emerge --ask libtool
```
#### Installing slightly older version of OpenSSL

If you then try to configure and make abd now, you'll get into some trouble with the newest OpenSSL installation on your device. The adb that we want to install is not compatible with the newest OpenSSL version, so we will get ourselfs a slightly older version of OpenSSL. This incompatability is due to some changes in OpenSSL's visibility of struct members. You can clone it from `$ git clone https://github.com/qhuyduong/openssl-1.0.2l.git`.

then:
```
$ cd openssl-1.0.2l/
$ ./Configure --prefix=/tmp/openssl os/compiler:gcc
$ make && make install
```
# TODO, see if version can or needs to be removed afterwords

### Troubleshooting for adb

If you encounter "WARNING: 'aclocal-1.xx' is missing on your system", just run `$ autoreconf -i --force` before configuring.


Now your system should be ready to build adb from source. 


 









[v3] build: include sys/sysmacros.h for major() and minor ...[v3] build: include sys/sysmacros.h for major() and minor ...[v3] build: include sys/sysmacros.h for major() and minor ...[v3] build: include sys/sysmacros.h for major() and minor ...[v3] build: include sys/sysmacros.h for major() and minor

make fork and add the change
or just make a pull req in openssl repo

# successfully build adb for pinephone!




















#CIVEMESOMESPACEATOM
