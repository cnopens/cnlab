Oracle修改SYS-system用户密码.md

SYS用户是Oracle中权限最高的用户，而SYSTEM是一个用于数据库管理的用户。在数据库安装完之后，应立即修改SYS,SYSTEM这两个用户的密码，以保证数据库的安全。

 

安装完之后修改密码方法

cmd命令行下输入 sqlplus / as sysdba;

法1.SQL>alter user sys identified by huozhe

 

法2.SQL>grant connect to sys identified by 123456

 

法3. SQL> password system

更改 system 的口令

新口令:

重新键入新口令:

口令已更改

（注：法3只适用于SYSTEM）

 

验证：

SQL> conn system/huozhe

已连接。

SQL> show user

USER 为 "SYSTEM"

SQL> exit

 

注：SYS和SYSTEM用户之间可以相互修改口令

 

修改SYS用户口令后的登录

将SYS用户的口令修改成123456后，可按以下几种方法登录：

法1.sqlplus / as sysdba 【以操作系统认证的方式登录，不需要用户名和口令】

法2.sqlplus sys/abcde as sysdba;

法3.sqlplus sys/ as sysdba

SQL*Plus: Release 11.2.0.1.0 Production on 星期二 11月 6 19:10:54 2012

Copyright (c) 1982, 2010, Oracle.  All rights reserved.

 

输入口令:

注意：这里提示输入口令，不输入口令直接回车

连接到:

Oracle Database 11g Enterprise Edition Release 11.2.0.1.0 - Production

With the Partitioning, OLAP, Data Mining and Real Application Testing optionssqlplus sys as sysdba;

上述语句，都可以登录成功，然后查看当前用户：

SQL> show user

USER 为 "SYS"

这是为什么呢，为什么修改了口令没有效果，不用口令或者随便用什么口令都可以进入呢。

答案是：认证方法。

 

oracle的口令认证

SYS口令认证分为操作系统认证和Oracle认证方法。

 

操作系统认证方式

对于如果是Unix操作系统，只要是以DBA组中的用户登录的操作系统，就可以以SYSDBA的身份登录数据库，不会验证SYS的口令。

 

对于windows操作系统，在oracle数据库安装后，会自动在操作系统中安装一个名为ORA_DBA的用户组，只要是该组中的用户，即可以SYSDBA的身份登录数据库而不会验证SYS的口令。也可以创建名为ORA_SID_DBA(SID为实例名)的用户组，属于该用户组的用户也具备以上特权。

 

如何修改认证方式

如何修改认证方式为操作系统认证或oracle认证。(windows，unix平台有大同小异)

 

要将认证方式设置为操作系统认证：

1.  修改sqlnet.ora文件

….\ product\11.2.0\ dbhome_2\ NETWORK\ADMIN\sqlnet.ora

…\product\版本号\home目录\ NETWORK\ADMIN\sqlnet.ora

记事本打开该文件，修改参数为：

SQLNET.AUTHENTICATION_SERVICES= (NTS)

WINDOWS下，默认就是这样，即使用NT认证

 

2.  修改init.ora文件

….\ product\11.2.0\dbhome_2\dbs\init.ora

说明：…\product\版本号\home目录\dbs\init.ora

 

记事本打开该文件，修改参数为：

remote_login_passwordfile='NONE'

 

3.重新启动数据库。

SQL> shutdown immediate

SQL> startup open

 

将认证方式设置为oracle认证(密码文件认证)：

1.  同上，修改sqlnet.ora

….\ product\11.2.0\ dbhome_2\ NETWORK\ADMIN\sqlnet.ora

记事本打开该文件，修改参数为：

#SQLNET.AUTHENTICATION_SERVICES= (NTS) ，#注释掉这句话，即不使用NT认证

或者

SQLNET.AUTHENTICATION_SERVICES= (NONE)

 

2.  同上，修改init.ora

记事本打开该文件，修改参数为：

remote_login_passwordfile='EXCLUSIVE'

或者

remote_login_passwordfile='SHARED'

 

EXCLUSIVE表示只有当前实例使用这个密码文件，且允许有别的用户作为SYSDBA登录进入系统，若选择了SHARED，则表示不止一个实例使用这个密码文件。

 

3.重新启动数据库。

SQL> shutdown immediate

SQL> startup open

 

如果发生sys密码丢失的情况，怎么办？

步骤1.使用system用户进行密码更改

SQL> conn system/huozhe

已连接。

SQL> alter user sys identified by huozhe

 

说明：

1）默认情况下，只要用户具有alter   user的权限，那么可以修改 oracle中任意用户，包括alter   user中的所有optional

 

2）默认情况下，system账户之所以能修改sys的密码，是因为它属于dba角色，而dba角色当然具有alter   user权限

 

SQL> select * from v$pwfile_users;

 

USERNAME                       SYSDB SYSOP SYSAS

------------------------------ ----- ----- -----

SYS                            TRUE  TRUE  FALSE

STUDY                          TRUE  FALSE FALSE

说明现在有sys及STUDY账户拥有sysdba与sysoper的权限[STUDY默认创建的]。

 

步骤2.创建密码文件

如果存在密码文件(PWDsid.ora)，则删除它

路径

….\product\11.2.0\dbhome_2\database\PWDorcl.ora

….\product\版本\home目录\database\PWDsid.ora

 

然后用orapwd.exe创建密码文件

orapwd路径

…\product\11.2.0\dbhome_2\BIN\orapwd.exe

说明：…\product\版本号\home目录\BIN\orapwd.exe

 

--cmd下输入 cd 命令进入到….\product\版本号\home目录\BIN 目录下，然后键入命令

orapwd file=filepath\pwd.ora password=password_of_sys entries=N

 

其中filepath表示密码文件路径,pwd.ora为密码文件名，sid是数据库实例名

eg:

E:\app\Administrator\product\11.2.0\dbhome_2\dbs\PWDorcl.ora

 

entries表示允许最大的超级用户数。

当没有指定文件路径时，密码文件默认存放在…\product\版本号\dbs\目录下。