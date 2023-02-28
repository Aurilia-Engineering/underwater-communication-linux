make ARCH=arm CROSS_COMPILE=arm-linux-gnueabihf- bcm2711_defconfig
make ARCH=arm menuconfig
make -j 12 ARCH=arm CROSS_COMPILE=arm-linux-gnueabihf- zImage modules dtbs
