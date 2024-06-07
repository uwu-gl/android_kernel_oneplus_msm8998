# LineageOS kernel build guide

This page will guide you build out OnePlus 5/5T kernel and install on your device.

# Compile kernel

### Operating system requirements

A recent computer that runs **Linux 64-bit (x86_64)**.

In this guide, we will use Ubuntu 20.04.

### Step 1. Set up compile environment

* 1. Using apt to install necessary packages:
```bash
sudo apt update -y
sudo apt-get install -y git ccache wget automake flex lzop bison gperf build-essential zip curl zlib1g-dev libxml2-utils bzip2 libbz2-dev libbz2-1.0 libghc-bzlib-dev squashfs-tools pngcrush schedtool dpkg-dev liblz4-tool make optipng maven libssl-dev pwgen libswitch-perl policycoreutils minicom libxml-sax-base-perl libxml-simple-perl bc libc6-dev-i386 lib32ncurses5-dev libx11-dev lib32z-dev libgl1-mesa-dev xsltproc unzip device-tree-compiler python2 python3 gcc-9-aarch64-linux-gnu gcc-9-arm-linux-gnueabi gcc-aarch64-linux-gnu gcc-arm-linux-gnueabi
sudo apt clean
```




* 2. Then clone gcc from Arm Developer:
```bash
cd ~
wget "https://developer.arm.com/-/media/Files/downloads/gnu-a/9.2-2019.12/binrel/gcc-arm-9.2-2019.12-x86_64-aarch64-none-linux-gnu.tar.xz"
xz -d gcc-arm-9.2-2019.12-x86_64-aarch64-none-linux-gnu.tar.xz
tar xf gcc-arm-9.2-2019.12-x86_64-aarch64-none-linux-gnu.tar
mkdir -p ~/toolchains/gcc
cp -r gcc-arm-9.2-2019.12-x86_64-aarch64-none-linux-gnu/* ~/toolchains/gcc
rm -rf gcc-arm-9.2-2019.12-x86_64-aarch64-none-linux-gnu gcc-arm-9.2-2019.12-x86_64-aarch64-none-linux-gnu.tar.xz
```

### Step 2. Sync up kernel source

```bash
git clone https://github.com/uwu-gl/android_kernel_oneplus_msm8998 ~/android-kernel
```

### Step 3. Set up PATH
```bash
export ARCH=arm64
export SUBARCH=arm64
export PATH=~/toolchains/gcc/bin:$PATH
export CROSS_COMPILE=aarch64-none-linux-gnu-
export CROSS_COMPILE_ARM32=arm-linux-gnueabi-
```

### Step 4. Start build
* 1. Back to kernel source folder:
```bash
cd ~/android-kernel
```

* 2. Make `defconfig`:
```bash
make O=out lineage_oneplus5_defconfig
```

* 3. Compile kernel
```bash
make -j $(nproc) O=out
```

### [OPTIONAL] Step 5. Make AnyKernel3 package
```bash
git clone https://github.com/Grill-Laux/AnyKernel3 anykernel -b upstream
cp ~/android-kernel/out/arch/arm64/boot/Image.gz-dtb anykernel
zip -r anykernel3_lineage_oplus5.zip * -x .git README.md *placeholder
 ```

### [OPTIONAL] Step 6. Install AnyKernel3 package

Now, reboot your device to **TWRP** recovery mode that support to use **adb sideload** .

Then, connect your device with your PC through **USB 2.0 port**.

Make sure your PC has already installed `adb`:
```bash
sudo apt install adb -y
```

Keep your connection, click **Advanced > adb sideload** on your device, and run command below on your PC:
```bash
adb sideload path-to-your-package.zip
```
Please notice replacing `path-to-your-package.zip` to your path!

On your device, the installation should be started.

If you face any problems, please give an issue or contact me, thanks!
