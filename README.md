# sabre-automotive-bsp
This repository is related to Freescale (now NXP) sabre automotive referent development platform

## imx6 Sabre automotive evaluation board setup

http://events17.linuxfoundation.org/sites/events/files/slides/AGLAMM_How%20we%20Run%20AGL%20on%20i.MX%20processors_tkobayashi_25FEB16%20rev.D.pdf

## Get u-boot

The latest u-boot version might not work out of the box.

    git clone http://git.denx.de/u-boot.git u-boot-i.mx6 OR
    git clone git://git.denx.de/u-boot.git u-boot-i.mx6
    cd u-boot-i.mx6
    git checkout -b <branch-name> # Take the current HEAD (the commit checked
    # out) and create a new branch called <branch-name>

## u-boot Build

Build u-boot using an ARM cross compiler, e.g. Fedora 29 gcc-arm-linux-gnu.x86_64:

To make .config file, the following command is required:

    ARCH=arm make mx6sabreauto_defconfig

## Installing arm cross compiler on the host (the host used is Fedora 30 distro)

To install on Fedora 30 gcc-arm-linux-gnu.x86_64 (which is not native x86_64 compiler), use the following command:

    sudo dnf install gcc-arm-linux-gnu.x86_64

## Actual u-boot compilation:

    Fedora:
    ARCH=arm CROSS_COMPILE=arm-linux-gnu- make -j8

    Debian:
    ARCH=arm CROSS_COMPILE=arm-none-eabi- make -j8

## Place u-boot.imx or SPL and u-boot.img on SD card

In Sabre board case, u-boot.imx will not be generated as this board supports SPL. Two files are generated: SPL and u-boot.img.

In order to flash the SD card!

SPL should reside at offset 1024B of the SD card. To put it there, do:

    $ sudo dd if=SPL of=/dev/sdb bs=1k seek=1; sync
    (Note - the SD card node may vary, so adjust this as needed).

    - Flash the u-boot.img image into the SD card:
    sudo dd if=u-boot.img of=/dev/sdb bs=1k seek=69; sync

U-boot should reside at offset 1024B of your SD card. To put it there, do:

    $ dd if=u-boot.imx of=/dev/<your-sd-card> bs=1k seek=1
    $ sync

Your SD card device is typically something in /dev/sd<X> or /dev/mmcblk<X>. Note that you need write permissions on the SD card for the command to succeed, so you might need to su - as root, or use sudo, or do a chmod a+w as root on the SD card device node to grant permissions to users.

## The actual log from the u-boot Sabre Automotive booting:

    U-Boot 2019.10-rc1-00132-gfef408679b (Aug 09 2019 - 17:09:06 +0200)

    CPU:   Freescale i.MX6Q rev1.2 996 MHz (running at 792 MHz)
    CPU:   Automotive temperature grade (-40C to 125C) at 20C
    Reset cause: POR
    Model: Freescale i.MX6 Quad SABRE Automotive Board
    Board: MX6Q-Sabreauto revD
    I2C:   ready
    DRAM:  2 GiB
    PMIC:  PFUZE100 ID=0x10
    wait_for_sr_state: Arbitration lost sr=33 cr=98 state=202
    wait_for_sr_state: failed sr=23 cr=88 state=2000
    i2c_imx_stop:trigger stop failed
    i2c_init_transfer: failed for chip 0x8 retry=0
    wait_for_sr_state: failed sr=21 cr=88 state=2000
    wait_for_sr_state: failed sr=21 cr=88 state=2000
    i2c_imx_stop:trigger stop failed
    i2c_init_transfer: failed for chip 0x8 retry=1
    wait_for_sr_state: Arbitration lost sr=33 cr=98 state=202
    wait_for_sr_state: failed sr=23 cr=88 state=2000
    i2c_imx_stop:trigger stop failed
    i2c_init_transfer: failed for chip 0x8 retry=0
    wait_for_sr_state: failed sr=21 cr=88 state=2000
    wait_for_sr_state: failed sr=21 cr=88 state=2000
    i2c_imx_stop:trigger stop failed
    i2c_init_transfer: failed for chip 0x8 retry=1
    NAND:  0 MiB
    MMC:   FSL_SDHC: 0
    Loading Environment from MMC... *** Warning - bad CRC, using default environment

    In:    serial
    Out:   serial
    Err:   serial
    Net:   FEC [PRIME]
    Hit any key to stop autoboot:  0 
    =>

## Partitioning SD card

Here, two parttion are create: /dev/sdX1 for kernel, and /dev/sdx2 for rootfs

Gived example where /dev/sdX is /dev/sdb

    echo "Create primary partition 1 for kernel"
    echo -e "n\np\n1\n\n+256M\nw\n"  | fdisk /dev/sdb

    echo "Create primary partition 2 for rootfs"
    echo -e "n\np\n2\n\n+8192M\nw\n"  | fdisk /dev/sdb

    echo "Formatting primary partition sdb1 for kernel"
    mkfs.vfat -F 32 /dev/sdb1

    echo "Formatting primary partition sdb2 for rootfs"
    mkfs.ext4 /dev/sdb2 -F

## Making kernel using kernel.org vanilla kernel 4.20.1

The kernel 4.20.1 source code is located @ the following location:

https://mirrors.edge.kernel.org/pub/linux/kernel/v4.x/

The file to be downloaded is the following: linux-4.20.1.tar.xz

The command to unpack the designated kernel is:

    tar -xvf inux-4.20.1.tar.xz
    cd linux-4.20.1/

To build the kernel, the following should be done:

Make proper .config:

    ARCH=arm make imx_v6_v7_defconfig

Compile the kernel itself:

    ARCH=arm CROSS_COMPILE=arm-linux-gnu- make -j8

The kernel itself to be used is in the directory: .../linux-4.20.1/arch/arm/boot/ and it is called zImage:

    .../linux-4.20.1/arch/arm/boot/zImage

The .dtb file to be used is in the directory: .../linux-4.20.1/arch/arm/boot/dts/ and it is called imx6q-sabreauto.dtb

    .../linux-4.20.1/arch/arm/boot/dts/imx6q-sabreauto.dtb

The location on SD card both components should be placed is /dev/sdX1 mounted to some directory (example: /tmp/sdX1 (where the SD card itself is: /dev/sdX).

Assuming X=b, it looks like:

    mount /dev/sdb1 /tmp/sdb1

The following will happed after booting u-boot, and after that kernel 4.20.1 from the SD card:

    Starting kernel ...

    [    0.000000] Booting Linux on physical CPU 0x0
    [    0.000000] Linux version 4.20.1 (vuser@fedora30-ssd) (gcc version 9.1.1 20190503 (Red Hat Cross 9.1.1-1) (GCC)) #1 SMP
    Sun Aug 11 09:02:22 CEST 2019
    [    0.000000] CPU: ARMv7 Processor [412fc09a] revision 10 (ARMv7), cr=10c5387d
    [    0.000000] CPU: PIPT / VIPT nonaliasing data cache, VIPT aliasing instruction cache
    [    0.000000] OF: fdt: Machine model: Freescale i.MX6 Quad SABRE Automotive Board
    [    0.000000] Memory policy: Data cache writealloc
    [    0.000000] cma: Reserved 64 MiB at 0x8c000000
    [    0.000000] random: get_random_bytes called from start_kernel+0x8c/0x488 with crng_init=0
    [    0.000000] percpu: Embedded 18 pages/cpu @(ptrval) s41896 r8192 d23640 u73728
    [    0.000000] Built 1 zonelists, mobility grouping on.  Total pages: 522752
    [    0.000000] Kernel command line: console=ttymxc3,115200 root=PARTUUID=9468c8e5-02 rootwait rw
    [    0.000000] Dentry cache hash table entries: 131072 (order: 7, 524288 bytes)
    [    0.000000] Inode-cache hash table entries: 65536 (order: 6, 262144 bytes)
    [    0.000000] Memory: 1989060K/2097152K available (11264K kernel code, 895K rwdata, 3724K rodata, 1024K init, 7619K bss,
    42556K reserved, 65536K cma-reserved, 1245184K highmem)
    [    0.000000] Virtual kernel memory layout:
    [    0.000000]     vector  : 0xffff0000 - 0xffff1000   (   4 kB)
    [    0.000000]     fixmap  : 0xffc00000 - 0xfff00000   (3072 kB)
    [    0.000000]     vmalloc : 0xf0800000 - 0xff800000   ( 240 MB)
    [    0.000000]     lowmem  : 0xc0000000 - 0xf0000000   ( 768 MB)
    [    0.000000]     pkmap   : 0xbfe00000 - 0xc0000000   (   2 MB)
    [    0.000000]     modules : 0xbf000000 - 0xbfe00000   (  14 MB)
    [    0.000000]       .text : 0x(ptrval) - 0x(ptrval)   (12256 kB)
    [    0.000000]       .init : 0x(ptrval) - 0x(ptrval)   (1024 kB)
    [    0.000000]       .data : 0x(ptrval) - 0x(ptrval)   ( 896 kB)
    [    0.000000]        .bss : 0x(ptrval) - 0x(ptrval)   (7620 kB)
    [    0.000000] SLUB: HWalign=64, Order=0-3, MinObjects=0, CPUs=4, Nodes=1
    [    0.000000] Running RCU self tests
    [    0.000000] rcu: Hierarchical RCU implementation.
    [    0.000000] rcu: 	RCU event tracing is enabled.
    [    0.000000] rcu: 	RCU lockdep checking is enabled.
    [    0.000000] rcu: RCU calculated value of scheduler-enlistment delay is 10 jiffies.
    [    0.000000] NR_IRQS: 16, nr_irqs: 16, preallocated irqs: 16
    [    0.000000] L2C-310 errata 752271 769419 enabled
    [    0.000000] L2C-310 enabling early BRESP for Cortex-A9
    [    0.000000] L2C-310 full line of zeros enabled for Cortex-A9
    [    0.000000] L2C-310 ID prefetch enabled, offset 16 lines
    [    0.000000] L2C-310 dynamic clock gating enabled, standby mode enabled
    [    0.000000] L2C-310 cache controller enabled, 16 ways, 1024 kB
    [    0.000000] L2C-310: CACHE_ID 0x410000c7, AUX_CTRL 0x76470001
    [    0.000000] Switching to timer-based delay loop, resolution 333ns
    [snap] Booting continues!  

## Making rootfs (using YOCTO sumo distribution):

http://variwiki.com/index.php?title=Yocto_Build_Release&release=RELEASE_MORTY_V1

## Specific annexes/addendums to the YOCTO documentation above:

The tags are also listed in https://github.com/varigit/variscite-bsp-platform/tags

To specify a latest release/tag (up to date), the following is done:

    repo init -u https://github.com/varigit/variscite-bsp-platform.git -b refs/tags/thud-fslc-4.14.78-mx6ul-v1.1

To understand how to setup the environment, please, do the following:

    source setup-environment

After evaluating the documentation, the following is done to setup the proper environment:

    MACHINE=imx6qdlsabreauto DISTRO=fslc-x11 source setup-environment build
