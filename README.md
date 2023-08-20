# OneCloud PiKVM

## Prepare

- step1. install [Armbian_OneCloud_jammy.img.xz](https://github.com/hzyitc/armbian-onecloud/releases/download/ci-20230706-173248-UTC/Armbian_23.08.0-trunk_Onecloud_jammy_edge_6.4.2.burn.img.xz) by @[hzyitc](https://github.com/hzyitc/armbian-onecloud)
- step2. install ```sudo apt install -y nginx tesseract-ocr tesseract-ocr-eng janus libevent-dev libgpiod-dev```
- step3. install [fruity-PiKVM_armhf.deb](https://github.com/jacobbar/fruity-pikvm/releases/download/debfile/fruity-pikvm_0.2_armhf.deb) by @[jacobbar](https://github.com/jacobbar/fruity-pikvm)
- step4. modify ```/usr/bin/kvmd-fix``` append ```echo device > /sys/class/usb_role/c9040000.usb-role-switch/role```

## How to use

Now, you got a PiKVM only price with 6$(RMB 40￥): OneCloud(3$/20￥)+MS2019(3$/20￥)

- step1. Plug ```USB MS2019``` in the usb port ```next``` to the enternet port, and then use hdmi cable connect to your target machine.
- step2. Plug ```USB Dual Male Cable``` in the usb-otg port ```away``` from the enternet port, and then connect to your target machine.
- step3. boot the OneCloud and enjoy :)

WARNING: OTG on OneCloud not support hot plug!!!

# Fruity PiKVM

This is an experimental script in an attempt to port PiKVM installation to other SBCs such as OrangePi, BananaPi, MangoPi etc..
it is based on the original raspberry pikvm software which can be found [HERE](https://pikvm.org/)


This script was tested on OrangePi Zero 2, a working OS image for OrangePi Zero 2 can be downloaded [HERE](https://github.com/jacobbar/fruity-pikvm/releases/download/os-images/Orangepizero2_2.2.2_ubuntu_jammy_server_linux5.13.0.zip)

At the moment this script supports architecture arm64/aarch64 and armhf/armv7l, and should work with any debian based distribution such as Ubuntu, Debian, Armbian etc...

## Installation
Install git, clone and run the script with the following code

```bash
sudo apt install -y git
git clone http://github.com/jacobbar/fruity-pikvm
cd fruity-pikvm
sudo ./install.sh
```
## MSD KVMD Patch
Befor applying the patch, you would need to resize your installation partition on the SD card using GParted and create an additional partition for the msd, finally you need to add a mount entry of the new partition to /etc/fstab where `/dev/mmcblk1p2` matches the name of the new partition you created

```
/dev/mmcblk1p2 /var/lib/kvmd/msd  ext4  nodev,nosuid,noexec,ro,errors=remount-ro,data=journal,X-kvmd.otgmsd-root=/var/lib/kvmd/msd,X-kvmd.otgmsd-user=kvmd  0 0
```

Applying the MSD KVMD patch
```
sudo ./msd-patch.sh
```
to enable msd after applying the patch you would need to comment out the msd override configuration in file /etc/kvmd/override.yaml
