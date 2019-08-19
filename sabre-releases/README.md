### YOCTO Release: thud-fslc-4.14.78-mx6ul-v1.1

Please, for HOST OS use the following: Ubuntu 18.04, since it ideally matches given YOCTO repos, presented here:

http://releases.ubuntu.com/18.04/

### verification done on the following 

Also, the following Debian Stretch release was used for the verification:

$ hostnamectl

	Static hostname: <pc_name>
	Icon name: computer-desktop
	Chassis: desktop
	Machine ID: ---
	Boot ID: ---
	Operating System: Debian GNU/Linux 9 (stretch)
	Kernel: Linux 4.9.0-8-amd64
	Architecture: x86-64

$ gcc --version

	gcc (Debian 6.3.0-18+deb9u1) 6.3.0 20170516
	Copyright (C) 2016 Free Software Foundation, Inc.
	This is free software; see the source for copying conditions.  There is NO
	warranty; not even for MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.

### YOCTO thud logs:
```
z00449ct@pc228:~/projects/i.MX6/i.MX6-yocto/build$ bitbake -k core-image-minimal
NOTE: Your conf/bblayers.conf has been automatically updated.
WARNING: /home/z00449ct/projects/i.MX6/i.MX6-yocto/sources/meta-variscite-fslc/recipes-support/swupdate/var-image-swu.bb: Unable to get checksum for var-image-swu SRC_URI entry sw-description: file could not be found
WARNING: /home/z00449ct/projects/i.MX6/i.MX6-yocto/sources/meta-variscite-fslc/recipes-bsp/u-boot/u-boot-fw-utils.bb: Unable to get checksum for u-boot-fw-utils-cross SRC_URI entry fw_env.config: file could not be found
WARNING: /home/z00449ct/projects/i.MX6/i.MX6-yocto/sources/meta-variscite-fslc/recipes-bsp/u-boot/u-boot-fw-utils.bb: Unable to get checksum for u-boot-fw-utils SRC_URI entry fw_env.config: file could not be found
WARNING: /home/z00449ct/projects/i.MX6/i.MX6-yocto/sources/meta-swupdate/recipes-support/swupdate/swupdate_git.bb: Unable to get checksum for swupdate SRC_URI entry swupdate.cfg: file could not be found
WARNING: /home/z00449ct/projects/i.MX6/i.MX6-yocto/sources/meta-swupdate/recipes-support/swupdate/swupdate_git.bb: Unable to get checksum for swupdate SRC_URI entry swupdate.default: file could not be found
WARNING: /home/z00449ct/projects/i.MX6/i.MX6-yocto/sources/meta-swupdate/recipes-support/swupdate/swupdate_2018.11.bb: Unable to get checksum for swupdate SRC_URI entry swupdate.cfg: file could not be found
WARNING: /home/z00449ct/projects/i.MX6/i.MX6-yocto/sources/meta-swupdate/recipes-support/swupdate/swupdate_2018.11.bb: Unable to get checksum for swupdate SRC_URI entry swupdate.default: file could not be found
WARNING: /home/z00449ct/projects/i.MX6/i.MX6-yocto/sources/poky/meta/recipes-connectivity/bluez5/bluez5_5.50.bb: Unable to get checksum for bluez5 SRC_URI entry variscite-bt.conf: file could not be found
Parsing recipes: 100% |######################################################################################################################| Time: 0:02:41
Parsing of 2571 .bb files complete (0 cached, 2571 parsed). 3567 targets, 227 skipped, 0 masked, 0 errors.
NOTE: Resolving any missing task queue dependencies

Build Configuration:
BB_VERSION           = "1.40.0"
BUILD_SYS            = "x86_64-linux"
NATIVELSBSTRING      = "debian-9"
TARGET_SYS           = "arm-fslc-linux-gnueabi"
MACHINE              = "imx6qdlsabreauto"
DISTRO               = "fslc-x11"
DISTRO_VERSION       = "2.6"
TUNE_FEATURES        = "arm armv7a vfp thumb neon callconvention-hard"
TARGET_FPU           = "hard"
meta                 
meta-poky            = "HEAD:50f33d3bfebcbfb1538d932fb487cfd789872026"
meta-oe              
meta-multimedia      = "HEAD:4cd3a39f22a2712bfa8fc657d09fe2c7765a4005"
meta-freescale       = "HEAD:46fcbab00f7e01ded4609c09be89161783426f41"
meta-freescale-3rdparty = "HEAD:c4b5ac6b20e4245ce0630e9197313aaef999a331"
meta-freescale-distro = "HEAD:4a244af3993ae662624c6f615464e6806cc719a2"
meta-browser         = "HEAD:75640e14e325479c076b6272b646be7a239c18aa"
meta-gnome           
meta-networking      
meta-python          = "HEAD:4cd3a39f22a2712bfa8fc657d09fe2c7765a4005"
meta-qt5             = "HEAD:0630018c0033c91fddda62a49f59a82ba6ec6850"
meta-swupdate        = "HEAD:66af6e7e019b07b48facfd68be3c4ab2094502a4"
meta-variscite-fslc  = "HEAD:ff12ab716e1c3f47e723a377f00aa01fa3e5b955"

NOTE: Fetching uninative binary shim from http://downloads.yoctoproject.org/releases/uninative/2.4/x86_64-nativesdk-libc.tar.bz2;sha256sum=06f91685b782f2ccfedf3070b3ba0fe4a5ba2f0766dad5c9d1642dccf95accd0
Initialising tasks: 100% |###################################################################################################################| Time: 0:00:01
Sstate summary: Wanted 781 Found 0 Missed 781 Current 0 (0% match, 0% complete)
NOTE: Executing SetScene Tasks
NOTE: Executing RunQueue Tasks
WARNING: linux-fslc-imx-4.9-1.0.x+gitAUTOINC+953c6e30c9-r0 do_fetch: Failed to fetch URL git://github.com/Freescale/linux-fslc.git;branch=4.9-1.0.x-imx, attempting MIRRORS if available
WARNING: u-boot-fslc-v2018.11+gitAUTOINC+6e25ce6f3c-r0 do_fetch: Failed to fetch URL git://github.com/Freescale/u-boot-fslc.git;branch=2018.11+fslc, attempting MIRRORS if available
NOTE: Tasks Summary: Attempted 2561 tasks of which 5 didn't need to be rerun and all succeeded.
NOTE: Writing buildhistory

Summary: There were 10 WARNING messages shown.
```
### Extra packages installed in the build

Systemd package:

	DISTRO_FEATURES_append = " systemd"
	VIRTUAL-RUNTIME_init_manager = "systemd"
	DISTRO_FEATURES_BACKFILL_CONSIDERED = "sysvinit"
	VIRTUAL-RUNTIME_initscripts = ""
	IMX_DEFAULT_DISTRO_FEATURES_append = " systemd"

### Some potential problems

Problem: error with the default locale setting in Yocto, which is POSIX. The problem: no idea how to change the locale setting to en_US.utf-8 in Yocto?

Solution (for Debian and Ubuntu): 

The following commands should be run:

	$ sudo apt-get install locales
	$ sudo dpkg-reconfigure locales
	$ sudo locale-gen en_US.UTF-8
	$ sudo update-locale LC_ALL=en_US.UTF-8 LANG=en_US.UTF-8
	$ export LANG=en_US.UTF-8

Additional packages might be required/to be installed:

	$ sudo apt-get install dos2unix chrpath textinfo
