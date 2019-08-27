While doing the following YOCTO build:

http://variwiki.com/index.php?title=Yocto_Build_Release&release=RELEASE_MORTY_V1

Actually, using the latest available tag: thud-fslc-4.14.78-mx6ul-v1.1

From https://github.com/varigit/variscite-bsp-platform/tags

I've got the following error, while compiling for both images below:
bitbake -k fsl-image-gui
bitbake -k fsl-image-qt5

(the environment setup is the following: MACHINE=imx6qdlsabreauto DISTRO=fslc-x11 source setup-environment build)

Seems, that the local file: file://variscite-bt.conf is somehow missing?!

I can add this file artificially:

https://github.com/varigit/meta-variscite-imx/blob/Krogoth-imx-4.1.15-var01/recipes-connectivity/bluez5/files/imx6ul-var-dart/variscite-bt.conf

The KEY question is: why this file is missing???
_______

log follows: bluez5-5.50-r0 do_fetch: Failed to fetch variscite-bt.conf

https://github.com/ZoranStojsavljevic/imx6-sabre-automotive-bsp/blob/master/Issues/Fetch_failed.log
