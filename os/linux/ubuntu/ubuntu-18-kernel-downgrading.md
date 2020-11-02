#ubuntu-18-kernel-downgrading.md


个人在安装vcs过程中遇到部分问题，猜测是因为内核版本太高，特此记录
降级linux内核版本

vi ~/etc/apt/sources.list
root模式下进入文件夹，对文件备份

deb http://security.ubuntu.com/ubuntu trusty-security main
在最后一行添加软件源地址

apt-get update
访问源列表里的每个网址，并读取软件列表，然后保存在本地电脑

sudo apt-cache search linux-image
查询列表中可更新的内核

apt-get install linux-image-unsigned-4.4.0-148-generic
安装4.4版本内核

dpkg -l |grep linux-image
查看是否安装成功

vim /etc/default/grub
进入grub文件


Advanced options for Ubuntu>Ubuntu, with Linux 4.4.0-148-generic
修改文件如上

update-grub
更新grub

重启后重新查看内核版本，发现已经完成内核降级
uname -a
