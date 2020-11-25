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




## En vars

[root@oracledb ~] vim /home/oracle/.bash_profile
export ORACLE_BASE=/ora/oracle
export ORACLE_HOME=$ORACLE_BASE/product/11.2.0/db_1
export ORACLE_SID=orcl
export ORACLE_TERM=xterm
export PATH=$ORACLE_HOME/bin:/usr/sbin:$PATH
export LD_LIBRARY_PATH=$ORACLE_HOME/lib:/lib:/usr/lib
export NLS_LANG=AMERICAN_AMERICA.ZHS16GBK


## database instance create


###12c
https://www.cnblogs.com/zyxnhr/p/11789306.html

###11g

1. 修改初始化穿件init.ora
2. 创建文件夹

