make ARCH=arm CROSS_COMPILE=arm-linux-gnueabihf- bcm2711_defconfig

make ARCH=arm menuconfig

make -j 12 ARCH=arm CROSS_COMPILE=arm-linux-gnueabihf- zImage modules dtbs

mkdir -p mnt/{boot,rootfs}

sudo mount /dev/sdf1 mnt/boot

sudo mount /dev/sdf2 mnt/rootfs

sudo make ARCH=arm CROSS_COMPILE=arm-linux-gnueabihf- INSTALL_MOD_PATH=mnt/rootfs modules_install

sudo cp arch/arm/boot/zImage mnt/boot/$KERNEL.img

sudo cp arch/arm/boot/dts/*.dtb mnt/boot/

sudo cp arch/arm/boot/dts/overlays/*.dtb* mnt/boot/overlays/

sudo cp arch/arm/boot/dts/overlays/README mnt/boot/overlays/

sudo umount mnt/*
