#ubuntu1804-UEFI环境下安装VirtualBox.md

错误描述：

----------------------------------------------------------
Kernel driver not installed (rc=-1908)

The VirtualBox Linux kernel driver is either not loaded or not set up correctly. Please try setting it up again by executing

'/sbin/vboxconfig'

as root.

If your system has EFI Secure Boot enabled you may also need to sign the kernel modules (vboxdrv, vboxnetflt, vboxnetadp, vboxpci) before you can load them. Please see your Linux system's documentation for more information.

where: suplibOsInit what: 3 VERR_VM_DRIVER_NOT_INSTALLED (-1908) - The support driver is not installed. On linux, open returned ENOENT. 

----------------------------------------------------------


1、问题

在一台新计算机上安装VirtualBox，启动虚拟机时出现“Kernel driver not installed (rc=-1908)”错误。按照错误提示执行“sudo modprobe vboxdrv”无法解决问题。降级安装老版本的VirtualBox问题依旧。最后发现这个问题是与计算机使用UEFI导致的。

 

2、问题产生的原因

首先说明一下虚拟机安装环境：

  - 主机：使用UEFI

  - 主机的操作系统：Ubuntu 16.04（uname -a：4.15.0-42-generic）

  - VirtualBox：v5.1.38_Ubuntu r122592


UEFI[1]定义了计算机固件与操作系统之间的接口，是传统BIOS的替代者。UEFI的功能之一是提供所谓的“Secure Boot”功能。该功能只允许通过数字签名认证的操作系统或驱动被加载。VirtualBox所需的模块“vboxdrv”没有进行数字签名，导致该模块未能被加载，进而导致VirtualBox运行失败。

 

3、解决方法一

在计算机固件配置中关闭“Secure Boot”功能，VirtualBox就可以正常运行。但是，一旦重新启动“Secure Boot”，问题又会出现。

 

4、解决方法二

为xboxdvr创建数字签名，并将数字签名添加到UEFI中。具体操作步骤如下：

 

1）创建数字签名

在命令行中执行以下命令
	
openssl req -new -x509 -newkey rsa:2048 -keyout modules.priv -outform DER -out modules.der -nodes -days 36500 -subj "/CN=KEYNAME/"   

执行这个命令，会在当前路径下创建一对数字签名文件FILENAME.priv和FILENAME.der。你可以将FILENAME和KEYNAME设置为任何你喜欢的名字。

 

2）检查vboxdrv的位置

在命令行中执行以下命令
	
modinfo vboxdrv

返回如下执行结果（显示内容可能会因为操作系统和硬件的不同而略有不同）
	
filename:       /lib/modules/4.15.0-42-generic/updates/dkms/vboxdrv.ko
version:        5.1.38_Ubuntu r122592 (0x002a0000)
license:        GPL
description:    Oracle VM VirtualBox Support Driver
author:         Oracle Corporation
srcversion:     6598048D64CD6300853C314
depends:       
retpoline:      Y
name:           vboxdrv
vermagic:       4.15.0-42-generic SMP mod_unload
parm:           force_async_tsc:force the asynchronous TSC mode (int)

 

3）对vboxdrv进行数字签名

在命令行中执行一下命令

sudo /usr/src/linux-headers-4.15.0-42-generic/scripts/sign-file sha256 ./FILENAME.priv ./FILENAME.der /lib/modules/4.15.0-42-generic/updates/dkms/vboxdrv.ko

sudo /usr/src/linux-headers-5.4.0-48-generic/scripts/sign-file sha256 ./FILENAME.priv ./FILENAME.der /lib/modules/5.4.0-48-generic/updates/dkms/vboxdrv.ko


 这里，

 - linux-headers-4.15.0-42-generic：应当与“uname -a”命令返回的结果相匹配。

 - ./FILENAME.priv”、./FILENAME.der：应当与步骤1）中生产的数字签名文件的路径相匹配。这上述例子中，我们假设一对数字签名文件处于命令行当前路径下。

 - /lib/modules/4.15.0-42-generic/updates/dkms/vboxdrv.ko：应当与步骤2）返回的“filename”保持一致。

 

4）将公钥添加至MOK

在命令行中执行以下命令，将vboxdrv的公钥添加到UEFI的MOK（Module Owned Keys）。执行命令过程中，要求用户输入密码，该密码用于步骤5。
1
	
sudo mokutil --import FILENAME.der

 

5）登记公钥

重启计算机，UEFI会检测到MOK中新添加了公钥，并提示用户是否登记新的公钥。用户根据屏幕提示执行即可（选择“Enroll key from disk”？2019/5/9补充，这句话可能写得不准，如果不对选择屏幕上其它选项）。

 

完成上述步骤后，VirtualBox中的虚拟机就可以正常运行了。

 

注意：升级内核后，这一问题会再次出现，需要重新对vboxdrv添加数字签名。



vagrant box add \
https://mirrors.tuna.tsinghua.edu.cn/ubuntu-cloud-images/bionic/current/bionic-server-cloudimg-amd64-vagrant.box \
--name ubuntu/bionic


sudo apt install virtualbox-guest-dkms virtualbox-guest-x11 linux-headers-$(uname -r)

The following packages have unmet dependencies:
 virtualbox-guest-x11 : Depends: xorg-video-abi-23
                        Depends: xserver-xorg-core (>= 2:1.18.99.901)
E: Unable to correct problems, you have held broken packages.

sovled:

sudo apt install -y xorg-video-abi-23
\

You might want to run 'apt --fix-broken install' to correct these.
The following packages have unmet dependencies:
 virtualbox-6.0 : Depends: libqt5x11extras5 (>= 5.6.0) but it is not going to be installed


执行以下命令安装vboxdrv模块即可解决：

sudo modprobe vboxdrv




Kernel driver not installed (rc=-1908)

The VirtualBox Linux kernel driver is either not loaded or not set up correctly. Please try setting it up again by executing

'/sbin/vboxconfig'

as root.

If your system has EFI Secure Boot enabled you may also need to sign the kernel modules (vboxdrv, vboxnetflt, vboxnetadp, vboxpci) before you can load them. Please see your Linux system's documentation for more information.

where: suplibOsInit what: 3 VERR_VM_DRIVER_NOT_INSTALLED (-1908) - The support driver is not installed. On linux, open returned ENOENT. 

检查驱动

ls -la /lib/modules/$(uname -r)/misc




解决方案：

https://askubuntu.com/questions/1008681/virtualbox-not-starting-after-kernel-upgrade


sudo apt-get install ppa-purge
sudo ppa-purge ppa:ubuntu-toolchain-r/test
#Select gcc version 5 using update-alternatives manually
sudo update-alternatives --config gcc



sudo apt-get purge linux-headers-4.4.0-116 linux-headers-4.4.0-116-generic linux-image-4.4.0-116-generic linux-image-extra-4.4.0-116-generic linux-signed-image-4.4.0-116-generic



9

I was facing the same problem. After kernel upgrade my gcc version was showing as 5.4.1. Downgrading this version to 5.4.0 helped me to have retpoline for vboxdrv kernel module.

Following steps from this link helped me solve my issue:

sudo apt-get install ppa-purge
sudo ppa-purge ppa:ubuntu-toolchain-r/test
#Select gcc version 5 using update-alternatives manually
sudo update-alternatives --config gcc

After these steps gcc --version should be (Ubuntu 5.4.0-6ubuntu1~16.04.9) 5.4.0 20160609

Then purge all the new linux headers (4.4.0-116)

sudo apt-get purge linux-headers-4.4.0-116 linux-headers-4.4.0-116-generic linux-image-4.4.0-116-generic linux-image-extra-4.4.0-116-generic linux-signed-image-4.4.0-116-generic

Again install them

sudo apt-get install linux-generic linux-signed-generic

Then re-install virtualbox, I installed latest virtualbox-5.2 this time, but default 5.0 version of virtualbox should also work fine.

sudo apt-get purge virtualbox-dkms virtualbox virtualbox-qt
sudo apt-get install virtualbox-5.2

And, we have retpoline support in latest module

anirudh@AHDRMD34579:~$ modinfo vboxdrv 
filename:       /lib/modules/4.4.0-116-generic/misc/vboxdrv.ko
version:        5.2.6 r120293 (0x00290000)
license:        GPL
description:    Oracle VM VirtualBox Support Driver
author:         Oracle Corporation
srcversion:     4880B21EFF1B605D6402982
depends:        
vermagic:       4.4.0-116-generic SMP mod_unload modversions retpoline 
parm:           force_async_tsc:force the asynchronous TSC mode (int)

