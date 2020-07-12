# centos7中oracle开机自启动.md

## 配置命令
chkconfig命令主要用来更新（启动或停止）和查询系统服务的运行级信息。谨记chkconfig不是立即自动禁止或激活一个服务，它只是简单的改变了符号连接。

使用语法：

chkconfig [--add][--del][--list][系统服务] 或 chkconfig [--level <等级代号>][系统服务][on/off/reset]

chkconfig --level 234 oracle on 
chkconfig --add oracle


---
## 修改oratab文件
以root身份进入系统，通过vi命令打开文件
vi /etc/oratab
进入vi编辑器后，找到“orcl:/home/oracle/app/oracle/product/11.2.0/db_1:N”，
改为“orcl:/home/oracle/app/oracle/product/11.2.0/db_1:Y”。修改完成后，保存退出vi

# This file is used by ORACLE utilities.  It is created by root.sh
# and updated by the Database Configuration Assistant when creating
# a database.

# A colon, ':', is used as the field terminator.  A new line terminates
# the entry.  Lines beginning with a pound sign, '#', are comments.
#
# Entries are of the form:
#   $ORACLE_SID:$ORACLE_HOME:<N|Y>:
#
# The first and second fields are the system identifier and home
# directory of the database respectively.  The third filed indicates
# to the dbstart utility that the database should , "Y", or should not,
# "N", be brought up at system boot time.
#
# Multiple entries with the same $ORACLE_SID are not allowed.
#
#
#orcl11g:/home/oracle/app/oracle/product/11.2.0/db_1:N
orcl11g:/ora/oracle/product/11.2.0/db_1:Y

键入命令“vi /etc/rc.d/rc.local”
在vi编辑器中，添加：

su oracle -lc "/ora/oracle/product/11.2.0/db_1/bin/lsnrctl start"
su oracle -lc "/ora/oracle/product/11.2.0/db_1/bin/dbstart"

注：
1)其中/home/oracle/app/oracle/product/11.2.0/db_1 为ORACLE_HOME
2)-l 表示同时切换用户目录。比如你要换到oracle用户下你的目录就同时在oracle目录下了。
－c则表示执行完命令好再返回到原来的用户。 

## oracle启动绑定监听器
修改dbstart和dbshut启动关闭脚本,使其启动数据库的同时也自动启动监听器（即启动数据库时启动监听器，停止数据库时停止监听器）：
vim $ORACLE_HOME/dbstart
找到下面的代码，在实际脚本代码的同样也修改dbshut脚本：
vim $ORACLE_HOME/dbshut

# The this to bring down Oracle Net Listener  
ORACLE_HOME_LISTNER=$1  
# 将此处的 ORACLE_HOME_LISTNER=$1 修改为   
ORACLE_HOME_LISTNER=$ORACLE_HOME  
if [ ! $ORACLE_HOME_LISTNER ] ; then  
echo "ORACLE_HOME_LISTNER is not SET, unable to auto-stop Oracle Net Listener"  
echo "Usage: $0 ORACLE_HOME"  
else  
LOG=$ORACLE_HOME_LISTNER/listener.log  


## 新建Oracle服务启动脚本
vim /etc/init.d/oracle

新建一个以oracle命名的文件，并将以下脚本代码复制到文件里

#!/bin/sh
# chkconfig: 345 61 61
# description: Oracle 11g R2 AutoRun Servimces
# /etc/init.d/oracle
#
# Run-level Startup script for the Oracle Instance, Listener, and
# Web Interface
export ORACLE_BASE=/ora
export ORACLE_HOME=$ORACLE_BASE/product/11.2.0/dbhome_1
export ORACLE_SID=ORCL
export PATH=$PATH:$ORACLE_HOME/bin
ORA_OWNR="oracle"
# if the executables do not exist -- display error
if [ ! -f $ORACLE_HOME/bin/dbstart -o ! -d $ORACLE_HOME ]
then
echo "Oracle startup: cannot start"
exit 1
fi
# depending on parameter -- startup, shutdown, restart
# of the instance and listener or usage display
case "$1" in
start)
# Oracle listener and instance startup
su $ORA_OWNR -lc $ORACLE_HOME/bin/dbstart
echo "Oracle Start Succesful!OK."
;;
stop)
# Oracle listener and instance shutdown
su $ORA_OWNR -lc $ORACLE_HOME/bin/dbshut
echo "Oracle Stop Succesful!OK."
;;
reload|restart)
$0 stop
$0 start
;;
*)
echo $"Usage: `basename $0` {start|stop|reload|reload}"
exit 1
esac
exit 0
保存退出
----

### 检测启停
cd /etc/rc.d/init.d
./oracle start
./oracle stop

## 检查一下脚本能否正确执行
cd /etc/rc.d/init.d
./oracle start
./oracle stop


## 加入自动启动行列

执行如下命令:
chmod 750 /etc/rc.d/init.d/oracle
ln -s /etc/rc.d/init.d/oracle /etc/rc2.d/S61oracle
ln -s /etc/rc.d/init.d/oracle /etc/rc3.d/S61oracle
ln -s /etc/rc.d/init.d/oracle /etc/rc4.d/S61oracle

ln -s /etc/rc.d/init.d/oracle /etc/rc0.d/K61oracle
ln -s /etc/rc.d/init.d/oracle /etc/rc6.d/K61oracle 

chkconfig –level 234 oracle on
chkconfig –add oracle 