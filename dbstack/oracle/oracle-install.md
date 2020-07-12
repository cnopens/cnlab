# oracle-install.md
---
ubuntu Oracle SQL Developer 安装

安装命令:
udo apt-get install tar
sudo apt-get install alien


将rpm包转化为deb包

sudo alien -cv sqldeveloper-18.2.0.183.1748-1.noarch.rpm

四.安装

sudo dpkg -i sqldeveloper.deb

安装时包错  /usr/local/lib/libexpat.so.1 is not a symbolic link 江此文件改为软连接即可

五.启动

输入sqldeveloper启动 启动时要输入jdk安装路径


## oracle 启动过程

startup nomount

startup mount

startup open 

startup force 

startup pfile

查看数据库状态
select status from v$istance



## oracle 自动启动




