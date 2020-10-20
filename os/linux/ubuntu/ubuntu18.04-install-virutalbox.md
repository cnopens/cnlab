#ubuntu18.04-install-virutalbox.md


Cannot run a VirtualBox image on Ubuntu 18.04.4 LTS due to “Kernel driver not installed (rc=-1908)” not installed error
Asked 3 months ago
Active 3 months ago
Viewed 231 times
0

I am aware that there are multiple versions of this question here. So I am going to try and provide a comprehensive detail of the issue I am facing and what I tried so far.
Problem
Whenever I try to start an image that I added I get the following error

 Kernel driver not installed (rc=-1908)  

The error is the same whether I run VirtualBox as sudo or not.
Suggested Fixes
This is a list of everything I tried to do to fix this issue.
Most of these suggestions can be found in multiple sources online (either on StackExchange forums or on different sites).
Obviously none of these suggestions have worked for me so far.

Suggestion 1: Don't use Apt version of VirtualBox
One suggestion that I found here (and on other sources) is to uninstall the apt version VirtualBox and install the package on the official site.

I did that and I even installed "VirtualBox 6.1.10 Oracle VM VirtualBox Extension Pack" as some articles suggested. However this was to no avail.
Suggestion 2: Disable Secure Boot
My secure boot is already disabled. I needed to disable it when I was working on having a dual boot of Windows and Ubuntu. I am mentioning this in the sake of full disclose, not sure if it's relevant though.



Suggestion 3: Install virtualbox-dkms (and linux-headers-generic)
This was suggested by many articles and stackoverflow users. This answer combines all commands needed to install dkms and linux headers. While this specific answer was not really upvoted, similar answers elsewhere seemed to be acceptable. Still this did not fix the issue for me. (P.S: Edit 2 has a list of these commands and its output)

Do you have any suggestions? Is there something I am missing?
Edit 1: Clarification

First: "did not work" and "did not fix" in this context mean I still get the same error when I try to run the image. Below is the full detailed error message I got

Kernel driver not installed (rc=-1908)

The VirtualBox Linux kernel driver is either not loaded or not set up correctly. Please reinstall virtualbox-dkms package and load the kernel module by executing

'modprobe vboxdrv'

as root.

If your system has EFI Secure Boot enabled you may also need to sign the kernel modules (vboxdrv, vboxnetflt, vboxnetadp, vboxpci) before you can load them. Please see your Linux system's documentation for more information.

where: suplibOsInit what: 3 VERR_VM_DRIVER_NOT_INSTALLED (-1908) - The support driver is not installed. On Linux, open returned ENOENT.

Second: When running virtual box through the terminal I get the same error above in addition to the warning below in the terminal

WARNING: The character device /dev/vboxdrv does not exist.
     Please install the virtualbox-dkms package and the appropriate
     headers, most likely linux-headers-generic.

     You will not be able to start VMs until this problem is fixed.

(P.S I have already installed the virtualbox-dkms package as show in Edit 2).
Edit 2: Commands of suggestion 2 and their output

1. Command1: sudo apt-get install virtualbox-dkms

Reading package lists... Done
Building dependency tree       
Reading state information... Done virtualbox-dkms is already the newest version (5.2.34-dfsg-0~ubuntu18.04.1).
The following packages were automatically installed and are no longer required:
  kbuild libsdl-ttf2.0-0
  module-assistant
Use 'sudo apt autoremove' to remove them.
0 upgraded, 0 newly installed, 0 to remove and 6 not upgraded.

2. Command 2: sudo dpkg-reconfigure virtualbox-dkms

------------------------------
Deleting module version: 5.2.34
completely from the DKMS tree.
------------------------------
Done.
Loading new virtualbox-5.2.34 DKMS files...
Building for 5.3.0-59-generic
Building initial module for 5.3.0-59-generic
Error! Bad return status for module build on kernel: 5.3.0-59-generic (x86_64)
Consult /var/lib/dkms/virtualbox/5.2.34/build/make.log for more information.
Job for virtualbox.service failed because the control process exited with error code.
See "systemctl status virtualbox.service" and "journalctl -xe" for details.
invoke-rc.d: initscript virtualbox, action "restart" failed.
● virtualbox.service - LSB: VirtualBox Linux kernel module
   Loaded: loaded (/etc/init.d/virtualbox; generated)
   Active: failed (Result: exit-code) since Thu 2020-06-25 12:03:38 IST; 4ms ago
     Docs: man:systemd-sysv-generator(8)
  Process: 16118 ExecStart=/etc/init.d/virtualbox start (code=exited, status=1/FAILURE)

Jun 25 12:03:38 shouman-XPS-15-7590 systemd[1]: Starting LSB: VirtualBox Linux kernel module...
Jun 25 12:03:38 shouman-XPS-15-7590 virtualbox[16118]:  * Loading VirtualBox kernel modules...
Jun 25 12:03:38 shouman-XPS-15-7590 virtualbox[16118]:  * No suitable module for running kernel found
Jun 25 12:03:38 shouman-XPS-15-7590 virtualbox[16118]:    ...fail!
Jun 25 12:03:38 shouman-XPS-15-7590 systemd[1]: virtualbox.service: Control process exited, code=exited status=1
Jun 25 12:03:38 shouman-XPS-15-7590 systemd[1]: virtualbox.service: Failed with result 'exit-code'.
Jun 25 12:03:38 shouman-XPS-15-7590 systemd[1]: Failed to start LSB: VirtualBox Linux kernel module.

This command failed and directed me to a log file. Below is the information in the log file:

DKMS make.log for virtualbox-5.2.34 for kernel 5.3.0-59-generic (x86_64)
Thu Jun 25 12:03:36 IST 2020
make: Entering directory '/usr/src/linux-headers-5.3.0-59-generic'
  CC [M]  /var/lib/dkms/virtualbox/5.2.34/build/vboxdrv/linux/SUPDrv-linux.o
  CC [M]  /var/lib/dkms/virtualbox/5.2.34/build/vboxdrv/SUPDrv.o
  CC [M]  /var/lib/dkms/virtualbox/5.2.34/build/vboxdrv/SUPDrvGip.o
gcc: error: unrecognized command line option ‘-fstack-protector-strong’
scripts/Makefile.build:288: recipe for target '/var/lib/dkms/virtualbox/5.2.34/build/vboxdrv/linux/SUPDrv-linux.o' failed
make[2]: *** [/var/lib/dkms/virtualbox/5.2.34/build/vboxdrv/linux/SUPDrv-linux.o] Error 1
make[2]: *** Waiting for unfinished jobs....
gcc: error: unrecognized command line option ‘-fstack-protector-strong’
scripts/Makefile.build:288: recipe for target '/var/lib/dkms/virtualbox/5.2.34/build/vboxdrv/SUPDrv.o' failed
make[2]: *** [/var/lib/dkms/virtualbox/5.2.34/build/vboxdrv/SUPDrv.o] Error 1
gcc: error: unrecognized command line option ‘-fstack-protector-strong’
scripts/Makefile.build:288: recipe for target '/var/lib/dkms/virtualbox/5.2.34/build/vboxdrv/SUPDrvGip.o' failed
make[2]: *** [/var/lib/dkms/virtualbox/5.2.34/build/vboxdrv/SUPDrvGip.o] Error 1
  CC [M]  /var/lib/dkms/virtualbox/5.2.34/build/vboxdrv/SUPDrvSem.o
  CC [M]  /var/lib/dkms/virtualbox/5.2.34/build/vboxdrv/SUPDrvTracer.o
gcc: error: unrecognized command line option ‘-fstack-protector-strong’
scripts/Makefile.build:288: recipe for target '/var/lib/dkms/virtualbox/5.2.34/build/vboxdrv/SUPDrvSem.o' failed
make[2]: *** [/var/lib/dkms/virtualbox/5.2.34/build/vboxdrv/SUPDrvSem.o] Error 1
gcc: error: unrecognized command line option ‘-fstack-protector-strong’
scripts/Makefile.build:288: recipe for target '/var/lib/dkms/virtualbox/5.2.34/build/vboxdrv/SUPDrvTracer.o' failed
make[2]: *** [/var/lib/dkms/virtualbox/5.2.34/build/vboxdrv/SUPDrvTracer.o] Error 1
scripts/Makefile.build:519: recipe for target '/var/lib/dkms/virtualbox/5.2.34/build/vboxdrv' failed
make[1]: *** [/var/lib/dkms/virtualbox/5.2.34/build/vboxdrv] Error 2
Makefile:1656: recipe for target '_module_/var/lib/dkms/virtualbox/5.2.34/build' failed
make: *** [_module_/var/lib/dkms/virtualbox/5.2.34/build] Error 2
make: Leaving directory '/usr/src/linux-headers-5.3.0-59-generic'

3. command 3: sudo dpkg-reconfigure virtualbox

vboxweb.service is a disabled or a static unit not running, not starting it.
Job for virtualbox.service failed because the control process exited with error code.
See "systemctl status virtualbox.service" and "journalctl -xe" for details.
invoke-rc.d: initscript virtualbox, action "restart" failed.
● virtualbox.service - LSB: VirtualBox Linux kernel module
   Loaded: loaded (/etc/init.d/virtualbox; generated)
   Active: failed (Result: exit-code) since Thu 2020-06-25 12:09:08 IST; 4ms ago
     Docs: man:systemd-sysv-generator(8)
  Process: 16596 ExecStart=/etc/init.d/virtualbox start (code=exited, status=1/FAILURE)

Jun 25 12:09:08 shouman-XPS-15-7590 systemd[1]: Starting LSB: VirtualBox Linux kernel module...
Jun 25 12:09:08 shouman-XPS-15-7590 virtualbox[16596]:  * Loading VirtualBox kernel modules...
Jun 25 12:09:08 shouman-XPS-15-7590 virtualbox[16596]:  * No suitable module for running kernel found
Jun 25 12:09:08 shouman-XPS-15-7590 virtualbox[16596]:    ...fail!
Jun 25 12:09:08 shouman-XPS-15-7590 systemd[1]: virtualbox.service: Control process exited, code=exited status=1
Jun 25 12:09:08 shouman-XPS-15-7590 systemd[1]: virtualbox.service: Failed with result 'exit-code'.
Jun 25 12:09:08 shouman-XPS-15-7590 systemd[1]: Failed to start LSB: VirtualBox Linux kernel module.

4. Command 4: sudo apt-get install linux-headers-generic

Reading package lists... Done
Building dependency tree       
Reading state information... Done
The following packages were automatically installed and are no longer required:
  kbuild libsdl-ttf2.0-0 linux-headers-4.15.0-106 linux-headers-4.15.0-106-generic module-assistant
Use 'sudo apt autoremove' to remove them.
The following additional packages will be installed:
  linux-headers-4.15.0-108 linux-headers-4.15.0-108-generic
The following NEW packages will be installed:
  linux-headers-4.15.0-108 linux-headers-4.15.0-108-generic
The following packages will be upgraded:
  linux-headers-generic
1 upgraded, 2 newly installed, 0 to remove and 5 not upgraded.
Need to get 12.0 MB of archives.
After this operation, 89.3 MB of additional disk space will be used.
Do you want to continue? [Y/n] y
Get:1 http://qa.archive.ubuntu.com/ubuntu bionic-updates/main amd64 linux-headers-4.15.0-108 all 4.15.0-108.109 [10.9 MB]
Get:2 http://qa.archive.ubuntu.com/ubuntu bionic-updates/main amd64 linux-headers-4.15.0-108-generic amd64 4.15.0-108.109 [1,097 kB]
Get:3 http://qa.archive.ubuntu.com/ubuntu bionic-updates/main amd64 linux-headers-generic amd64 4.15.0.108.96 [2,392 B]
Fetched 12.0 MB in 2s (6,355 kB/s)                
Selecting previously unselected package linux-headers-4.15.0-108.
(Reading database ... 251776 files and directories currently installed.)
Preparing to unpack .../linux-headers-4.15.0-108_4.15.0-108.109_all.deb ...
Unpacking linux-headers-4.15.0-108 (4.15.0-108.109) ...
Selecting previously unselected package linux-headers-4.15.0-108-generic.
Preparing to unpack .../linux-headers-4.15.0-108-generic_4.15.0-108.109_amd64.deb ...
Unpacking linux-headers-4.15.0-108-generic (4.15.0-108.109) ...
Preparing to unpack .../linux-headers-generic_4.15.0.108.96_amd64.deb ...
Unpacking linux-headers-generic (4.15.0.108.96) over (4.15.0.106.94) ...
Setting up linux-headers-4.15.0-108 (4.15.0-108.109) ...
Setting up linux-headers-4.15.0-108-generic (4.15.0-108.109) ...
/etc/kernel/header_postinst.d/dkms:
 * dkms: running auto installation service for kernel 4.15.0-108-generic

Kernel preparation unnecessary for this kernel.  Skipping...

Building module:
cleaning build area...
make -j8 KERNELRELEASE=4.15.0-108-generic -C /lib/modules/4.15.0-108-generic/build M=/var/lib/dkms/virtualbox/5.2.34/build...(bad exit status: 2)
ERROR: Cannot create report: [Errno 17] File exists: '/var/crash/virtualbox-dkms.0.crash'
Error! Bad return status for module build on kernel: 4.15.0-108-generic (x86_64)
Consult /var/lib/dkms/virtualbox/5.2.34/build/make.log for more information.
   ...done.
Setting up linux-headers-generic (4.15.0.108.96) ...
libdvd-pkg: Checking orig.tar integrity...
/usr/src/libdvd-pkg/libdvdcss_1.4.2.orig.tar.bz2: OK
libdvd-pkg: `apt-get check` failed, you may have broken packages. Aborting...

18.04
apt
kernel
virtualbox
share improve this question follow
edited Jun 25 at 11:34
asked Jun 24 at 19:09
Abdelrahman Shoman
10144 bronze badges

    which version of VirtualBox are you trying ? the latest ? try with an older version, 6.0 for example, if that's the case – Giorgos Saridakis Jun 24 at 19:14
    @GiorgosSaridakis I have installed the latest version (i.e 6.1.10) through the official site. The apt verion that I installed was 6.0. – Abdelrahman Shoman Jun 24 at 19:27 

1
Show us the complete input and output you did for Suggestion 3. Do it again if neccessary. – user535733 Jun 24 at 19:46
1
I am sure you did, but can you confirm you did a reboot after suggestion 3. – crip659 Jun 24 at 20:16
1
Looks like you ran Virtualbox, which refused to run and told you exactly why and how to fix it ("Please install the virtualbox-dkms package and the appropriate headers, most likely linux-headers-generic.") Did you do that? Do you have that output? Installing virtualbox-dkms is only half of the instruction. – user535733 Jun 24 at 21:30

show 6 more comments
1 Answer
0

So while I was trying to update the question in respond to @user535733 comment, I noticed a gcc error in the log file /var/lib/dkms/virtualbox/5.2.34/build/make.log

gcc: error: unrecognized command line option ‘-fstack-protector-strong’

In this article user Ville Nummela (ville-nummela) mentioned that

    building kernel drivers for newer kernels requires gcc 4.9

I had an old gcc which was the issue. After updating my gcc it now works fine.

gcc --version
gcc (Ubuntu 8.4.0-1ubuntu1~18.04) 8.4.0

This was also mentioned in this answer but I didn't ntice it as it was not upvoted.

Note:: After updating my gcc I had to rerun the following commands

sudo dpkg-reconfigure virtualbox-dkms 
sudo dpkg-reconfigure virtualbox

Hopefully this helps someone else in the future




 Ubuntu18.04 VMtools的安装与卸载

VM不推荐在Ubuntu中使用VMtools而是open-vm-tools，原文地址https://kb.vmware.com/s/article/2073803

安装方式

　　1 更新系统源

　　sudo apt update

　　2 安装open-vm-tools

　　sudo apt install open-vm-tools

　　3 如果要实现文件夹共享，需要安装 open-vm-tools-dkms

　　sudo apt install open-vm-tools-dkms

　　4 桌面环境还需要安装 open-vm-tools-desktop 以支持双向拖放文件

　　sudo apt install open-vm-tools-desktop

