Centos7-oracle11.2-命令行创建数据库.md

安装完数据库oracle11.2默认是没有安装数据库，需要手动安装：

oracle常用命令：


sqlplus / as sysdba      进入dba模式
shutdown immediate   立即停止
startup open credit     打开实例

lsnrctl            start     启动监听：
lsnrctl            status   查看状态
lsnrctl           stop       停止监听


我们可以使用DBCA创建数据库，但是手工建库也是DBA必须掌握的，学会了手工建库有利于我们更好的了解oracle的体系结构。我们一起看一下手工建库的步骤吧！

 
数据库系统版本：11.2g

1)设置数据库的环境变量
[oracle@ENMOEDU ~]$ vi .bash_profile

# .bash_profile
# Get the aliases and functions

if [ -f ~/.bashrc ]; then

        . ~/.bashrc

fi

# User specific environment and startup programs
PATH=$PATH:$HOME/bin
export PATH
export ORACLE_BASE=/u01/app/oracle
export ORACLE_HOME=$ORACLE_BASE/product/11.2.0/dbhome_1
export ORACLE_SID=ENMOEDU
export PATH=$ORACLE_HOME/bin:$PATH

2）创建口令文件
[oracle@ENMOEDU ~]$ cd $ORACLE_HOME/dbs
[oracle@ENMOEDU dbs]$ ls
init.ora
[oracle@ENMOEDU dbs]$ orapwd file=orapwENMOEDU password=oracle entries=30;
[oracle@ENMOEDU dbs]$ ls
init.ora  orapwENMOEDU

3）修改已有的init.ora创建参数文件 此数据库的db_name=ENMOEDU

[oracle@ENMOEDU dbs]$ cat init.ora|grep -v ^$|grep -v ^# > initENMOEDU.ora

[oracle@ENMOEDU dbs]$ vi initENMOEDU.ora

db_name='ENMOEDU'
memory_target=1G
processes = 150
audit_file_dest='/u01/app/oracle/admin/ENMOEDU/adump'
audit_trail ='db'
db_block_size=8192
db_domain=''
db_recovery_file_dest='/u01/app/oracle/flash_recovery_area'
db_recovery_file_dest_size=2G
diagnostic_dest='/u01/app/oracle'
dispatchers='(PROTOCOL=TCP) (SERVICE=ORCLXDB)'
open_cursors=300
remote_login_passwordfile='EXCLUSIVE'
undo_tablespace='UNDOTBS1'
control_files = ('/u01/app/oracle/oradata/ENMOEDU/control01.ctl', '/u01/app/oracle/oradata/ENMOEDU/control.ctl')
compatible ='11.2.0'

4）创建数据库需要的文件夹

[oracle@ENMOEDU ~]$ mkdir -p /u01/app/oracle/admin/ENMOEDU/adump
[oracle@ENMOEDU ~]$ mkdir -p /u01/app/oracle/flash_recovery_area
[oracle@ENMOEDU ~]$ mkdir -p /u01/app/oracle/oradata/ENMOEDU


5）启动数据库到nomount

[oracle@ENMOEDU dbs]$ sqlplus / as sysdba
SQL*Plus: Release 11.2.0.3.0 Production on Wed Feb 5 20:48:58 2014
Copyright (c) 1982, 2011, Oracle.  All rights reserved.
Connected to an idle instance.

SQL> startup nomount
ORACLE instance started.

Total System Global Area 1071333376 bytes

Fixed Size                  1349732 bytes

Variable Size             620758940 bytes

Database Buffers          444596224 bytes

Redo Buffers                4628480 bytes

 

6）创建数据库的一个脚本（根据官方文档）

[oracle@ENMOEDU oracle]$ vi create_db.sql

CREATE DATABASE ENMOEDU

   USER SYS IDENTIFIED BY oracle

   USER SYSTEM IDENTIFIED BY oracle

   LOGFILE GROUP 1 ('/u01/app/oracle/oradata/ENMOEDUredo01a.log','/u01/app/oracle/oradata/ENMOEDUredo01b.log') SIZE 100M BLOCKSIZE 512,

           GROUP 2 ('/u01/app/oracle/oradata/ENMOEDUredo02a.log','/u01/app/oracle/oradata/ENMOEDUredo02b.log') SIZE 100M BLOCKSIZE 512,

           GROUP 3 ('/u01/app/oracle/oradata/ENMOEDUredo03a.log','/u01/app/oracle/oradata/ENMOEDUredo03b.log') SIZE 100M BLOCKSIZE 512

   MAXLOGFILES 5

   MAXLOGMEMBERS 5

   MAXLOGHISTORY 1

   MAXDATAFILES 100

   CHARACTER SET AL32UTF8

   NATIONAL CHARACTER SET AL16UTF16

   EXTENT MANAGEMENT LOCAL

   DATAFILE '/u01/app/oracle/oradata/ENMOEDU/system01.dbf' SIZE 325M REUSE

   SYSAUX DATAFILE '/u01/app/oracle/oradata/ENMOEDU/sysaux01.dbf' SIZE 325M REUSE

   DEFAULT TABLESPACE users

      DATAFILE '/u01/app/oracle/oradata/ENMOEDU/users01.dbf'

      SIZE 500M REUSE AUTOEXTEND ON MAXSIZE UNLIMITED

   DEFAULT TEMPORARY TABLESPACE tempts1

      TEMPFILE '/u01/app/oracle/oradata/ENMOEDU/temp01.dbf'

      SIZE 20M REUSE

   UNDO TABLESPACE undotbs1

      DATAFILE '/u01/app/oracle/oradata/ENMOEDU/undotbs01.dbf'

      SIZE 200M REUSE AUTOEXTEND ON MAXSIZE UNLIMITED;

 

7）运行脚本创建数据库

SQL> @/home/oracle/create_db.sql
Database created.

8）运行脚本创建数据字典视图

SQL> @?/rdbms/admin/catalog.sql
SQL> @?/rdbms/admin/catproc.sql

@?/rdbms/admin/bmssutil.sql
alter package standard compile;
alter package dbms_standard compile;

9）查看数据库的状态

SQL> select status from v$instance;  
STATUS
------------
OPEN
1 row selected.

10）创建spfile文件

SQL> create spfile from pfile;
File created.

手工创建数据库就完成了。

总结：手工建库的步骤为：设置环境变量；创建口令文件；创建pfile文件，创建必要的文件目录；创建数据库，运行必要的脚本；创建spfile文件。