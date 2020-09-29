## ubuntu安装应用问题.md




workstation安装:


0 将VMware的 .bundle 文件放在当前用户的根目录下（和桌面同级）。

1 赋予 .bundle 文件执行权限

sudo chmod +x VMware-Workstation-Full-14.1.1-7528167.x86_64.bundle

    1

2 安装

sudo ./VMware-Workstation-Full-14.1.1-7528167.x86_64.bundle

    1

3 卸载

sudo vmware-installer -u vmware-workstation


错误:
Gtk-Message: Failed to load module "canberra-gtk-module"解决方案?
安装gtk和gtk3即可
sudo apt install libcanberra-gtk-module libcanberra-gtk3-module



## Sorry,ubuntu18.04 experienced an internal error

sudo apt --reinstall install signon-ui
sudo apt --reinstall install signon-ui-x11
sudo apt --reinstall install signon-ui-service
sudo reboot


## Linux 安装软件报错 Sub-process /usr/bin/dpkg returned an error code (1)?

解决办法：

cd /var/lib/dpkg/
/var/lib/dpkg# mv info/ info_old
/var/lib/dpkg# mkdir info
/var/lib/dpkg# apt-get update
/var/lib/dpkg# apt-get -f install
/var/lib/dpkg# mv info/* info_old/
/var/lib/dpkg# rm -rf info/
/var/lib/dpkg# mv info_old/ info


debconf: DbDriver "config": /var/cache/debconf/config.dat is locked by another process: Resource temporarily unavailable

debconf: DbDriver "config": /var/cache/debconf/config.dat is locked by another process: 资源暂时不可用

resoved: 
sudo lsof /var/cache/debconf/config.dat
frontend 4250 root    4uW  REG    7,0    40347 2966 /var/cache/debconf/config.dat
sudo kill 4250
sudo apt-get autoclean
sudo apt-get clean
sudo apt-get autoremove 


modprobe: ERROR: could not insert 'vboxdrv': Operation not permitted?






## 解决Ubuntu中“检测到系统程序错误”的办法?
打开一个终端，输入以下命令：

    sudo apt install gksu  //gksu是用来执行图形的（GUI）程序
    gksu gedit /etc/default/apport
     

将打开的文件中的enabled = 1 修改为 0，然后保存，以后就不会推送系统程序出现问题了。

很显然，如果我们想重新开启错误报告功能，只要再打开这个文件，把enabled设置为1就可以了.



/home/cnopens/.config/qv2ray/vcore/

/home/cnopens/.config/qv2ray/vcore/v2ray


/home/cnopens/snap/qv2ray/2733/.config/qv2ray/vcore/v2ray

/home/cnopens/snap/qv2ray/2733/.config/qv2ray/vcore/



## apt-get安装出现E: Sub-process /usr/bin/dpkg returned an error code (1)问题

cd /var/lib/dpkg/
sudo mv info/ info_bak          # 现将info文件夹更名
sudo mkdir info                 # 再新建一个新的info文件夹
sudo apt-get update             # 更新
sudo apt-get -f install         # 修复
sudo mv info/* info_bak/        # 执行完上一步操作后会在新的info文件夹下生成一些文件，现将这些文件全部移到info_bak文件夹下
sudo rm -rf info                # 把自己新建的info文件夹删掉
sudo mv info_bak info           # 把以前的info文件夹重新改回名



How to fix and prevent VirtualBox Kernel driver not installed?

$ sudo apt install --reinstall virtualbox-dkms && sudo apt install libelf-dev
$ sudo /sbin/vboxconfig
