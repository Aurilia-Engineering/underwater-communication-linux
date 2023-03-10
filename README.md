# Kernel Compilation for CM4 Modules

## Steps on RaspberryPi

```bash
FIRMWARE_HASH=$(zgrep "* firmware as of" /usr/share/doc/raspberrypi-bootloader/changelog.Debian.gz | head -1 | awk '{ print $5 }')
```

```bash
KERNEL_HASH=$(wget https://raw.github.com/raspberrypi/firmware/$FIRMWARE_HASH/extra/git_hash -O -)
```

Get GIT hash of the running kernel for cross compiling

```bash
echo KERNEL_HASH
```

## Steps on cross compiling host

```bash
KERNEL=kernel8
```

```bash
make ARCH=arm64 CROSS_COMPILE=aarch64-linux-gnu- bcm2711_defconfig
```

```bash
make ARCH=arm64 menuconfig
```

```bash
make -j 12 ARCH=arm64 CROSS_COMPILE=aarch64-linux-gnu- Image modules dtbs
```

```bash
mkdir -p mnt/{boot,rootfs}
```

```bash
sudo mount /dev/sdf1 mnt/boot
```

```bash
sudo mount /dev/sdf2 mnt/rootfs
```

```bash
sudo make ARCH=arm64 CROSS_COMPILE=aarch64-linux-gnu- INSTALL_MOD_PATH=mnt/rootfs modules_install
```

```bash
sudo cp arch/arm64/boot/Image mnt/boot/$KERNEL.img
```

```bash
sudo cp arch/arm64/boot/dts/*.dtb mnt/boot/
```

```bash
sudo cp arch/arm64/boot/dts/overlays/*.dtb* mnt/boot/overlays/
```

```bash
sudo cp arch/arm64/boot/dts/overlays/README mnt/boot/overlays/
```

```bash
sudo umount mnt/*
```
