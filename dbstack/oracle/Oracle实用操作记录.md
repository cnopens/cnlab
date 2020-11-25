# Oracle实用操作记录.md
--  
## 获取表字段：  
      
    select *   
    from user_tab_columns   
    where Table_Name='用户表'   
    order by column_name  
--     
    ## 获取表注释：  
     
    select *   
    from user_tab_comments   
    where Table_Name='用户表'  
      
    order by Table_Name  
--      
    ## 获取字段注释：  
      
    select *   
    from user_col_comments   
    where Table_Name='用户表'  
      
    order by column_name  
    
  
## 获取表：
 --     
    select table_name from user_tables; //当前用户的表        
      
    select table_name from all_tables; //所有用户的表    
      
    select table_name from dba_tables; //包括系统表  
      
    select table_name from dba_tables where owner='zfxfzb'  
      
    /*   
    user_tables：  
      
    table_name,tablespace_name,last_analyzed等  
      
    dba_tables：  
      
    ower,table_name,tablespace_name,last_analyzed等  
      
    all_tables：  
      
    ower,table_name,tablespace_name,last_analyzed等  
      
    all_objects：  
      
    ower,object_name,subobject_name,object_id,created,last_ddl_time,timestamp,status等   
    */  
      
    /*  获取表字段：*/  
      
    select * from user_tab_columns where Table_Name='用户表';  
      
    select * from all_tab_columns where Table_Name='用户表';  
      
    select * from dba_tab_columns where Table_Name='用户表';  
      
    /* user_tab_columns：  
      
    table_name,column_name,data_type,data_length,data_precision,data_scale,nullable,column_id等  
      
    all_tab_columns ：  
      
    ower,table_name,column_name,data_type,data_length,data_precision,data_scale,nullable,column_id等  
      
    dba_tab_columns：  
      
    ower,table_name,column_name,data_type,data_length,data_precision,data_scale,nullable,column_id等   
    */  
      
    /*  获取表注释：*/  
      
    select * from user_tab_comments  
      
    /*   
    user_tab_comments：table_name,table_type,comments  
      
    相应的还有dba_tab_comments，all_tab_comments，这两个比user_tab_comments多了ower列。   
    */  
      
    /* 获取字段注释：*/  
      
    select * from user_col_comments  
      
    /*  
      
    user_col_comments：table_name,column_name,comments  
      
    相应的还有dba_col_comments，all_col_comments，这两个比user_col_comments多了ower列。   
    */  
    查询表名及表注释

    SELECT T.TABLE_NAME, P.COMMENTS
    FROM ALL_ALL_TABLES T, USER_TAB_COMMENTS P
    WHERE T.TABLE_NAME LIKE 'PMS_WSH%'
    AND T.TABLE_NAME = P.TABLE_NAME
    GROUP BY T.TABLE_NAME, P.COMMENTS


## 系统操作：

oracle用户解锁语句

解决过程：
使用有alter user数据库权限的用户登陆，角色选sysdba，执行以下命令：
解锁命令： SQL> ALTER USER 用户名 ACCOUNT UNLOCK;
锁定用户命令：SQL> ALTER USER 用户名 ACCOUNT LOCK;


注：登陆用户没有alter user数据库权限，使用拥有dba角色的用户登陆执行以下命令：
SQL> grant alter user to 用户名;
这样，对应的需要登录sqlplus的用户就可以去解锁其它用户了。直接使用具有dba角色就是的用户登陆解锁就OK了，因为dba角色拥有alter user权限。
 
查看数据库中所有角色和对应权限的语句：select * from role_sys_privs;
查看当前登陆用户拥有的角色的语句：select * from user_role_privs


## java.sql.SQLException: ORA-28001: 密碼已經屆滿?

解决周期问题

oracle创建用户默认有180天密码过期的限制；
1、sqlplus /nolog
2、conn /as sysdba
3、查看用户所属的文件夹

SELECT username, PROFILE FROM dba_users;
1
在这里插入图片描述
4、查看此文件下

SELECT * FROM dba_profiles s WHERE s.profile='DEFAULT' AND resource_name = 'PASSWORD_LIFE_TIME';
1
在这里插入图片描述
5、修改为密码周期为不限制；

alter profile default limit password_life_time unlimited;
1
在这里插入图片描述
6、如果已经提示：java.sql.SQLException: ORA-28001: 口令已经失效这样的错误，还需要修改一下密码，改为本密码就可以。

alter user 用户名 identified by 旧密码;

## Oracle数据备份及导入和导出

---
note : oracle install ->https://blog.csdn.net/csgd2000/article/details/100224722#commentBox

Oracle数据库导入导出命令

Exp/Imp

此组合命令属于客户端命令，在Oracle客户端和数据库服务器上皆可使用。

导出：

exp system/manager@orcl  file=d:\test.dmp owner=test （按用户导出）

exp system/manager@orcl file=d:\test.dmp full=y （将数据库完全导出）

exp system/manager@orcl file=d:\test.dmp tables=(table1,table2) （按表导出）

导入：
使用imp命令：
指定用户导入-》
imp system/password fromuser=导出的用户 touser=导入的用户 file=文件路径

imp system/manager@orcl  file=d:\test.dmp （导入整个备份）

导入时可能因为表存在而报错，可加入 ignore=y即可。

imp system/manager@orcl file=d:\ test.dmp tables=(table1,table2) （导入备份中指定表数据）

导入导出时需看完整日志则可加入参数log=export.log

Expdp/Impdp

此组合命令属于服务端命令，只可在服务器端执行。依赖于定义的逻辑目录，命令中需申明” DIRECTORY=” ，此目录一般为手动创建并赋权。

（z; 查看

create directory dir as ‘/home/test/’;创建

grant read,write on directory dir to 用户名;

drop directory DIR ;  删除）

导出：

expdp test/test dumpfile=test.dmp DIRECTORY=DIR schemas=test （按用户导出）

expdp test/test dumpfile=test.dmp DIRECTORY=DIR TABLES=table1, table2

（导出指定表）

导入：

impdp test/test DIRECTORY=DIR DUMPFILE=test.dmp schemas=test

导入时可能因为表已存在而导入失败，可加入参数TABLE_EXISTS_ACTION=truncate

注意事项：

a.使用exp/imp命令导出时，容易空表导不出；(导出日志可看出空表是否导出)

此现象取决于oracle本身参数deferred_segment_creation=true;默认开着，空表不能导出，一般在建表之前修改系统参数后，后续空表可导出。

alter system set  deferred_segment_creation=false;

Expdp/Impdp无此现象。

b.导入时修改所属用户

imp test/test@orcl fromuser=usera touser=userb file=exp.dmp log=exp.log;

impdp test/test directory=dir dumpfile=expdp.dmp remap_schema='usera':'userb' logfile=exp.log;

c.导入时修改所属表空间

exp/imp修改表空间需手工处理，导入进去之后（导入时还是要创建原表空间），alter table xxx move tablespace_new（表较多时，需批量生成该命令）

impdp只需remap_tablespace='tabspace_old':'tablespace_new '



## ORACLE CENTOS7监听器配置

CentOS 7下oracle 11g配置监听
1、本机系统为CentOS-7-x86_64

2、已正常安装了oracle 11g并且实例startup启动成功

1、编辑listener.ora配置文件，设置监听

#1.1、进入oracle监听配置文件所在目录

[oracle@imzcy ~]$ cd /db/app/oracle/product/11.2.0/network/admin/

[oracle@imzcy admin]$ ls

listener.ora  samples  shrept.lst  sqlnet.ora  tnsnames.ora

 

#1.2、编辑监听配置文件

[oracle@imzcy admin]$ vim listener.ora

#可以直接复制下面提供的配置内容使用，需按如下所诉根据本身配置做修改。

#GLOBAL_DBNAME= DB01                           修改为自己的全局数据库名

#ORACLE_HOME=/db/app/oracle/product/11.2.0     修改为自己的oracle家目录

#SID_NAME=orcl                                 修改为自己的SID

#HOST=192.168.122.155                          修改为自己电脑IP

#ADR_BASE_LISTENER = /db/app/oracle            修改为自己oracle安装目录

# listener.ora Network Configuration File: /db/app/oracle/product/11.2.0/network/admin/listener.ora

# Generated by Oracle configuration tools.

SID_LIST_LISTENER=

  (SID_LIST=

    (SID_DESC=

      (GLOBAL_DBNAME= DB01)

      (ORACLE_HOME=/db/app/oracle/product/11.2.0)

      (SID_NAME=orcl)

    )

  )

LISTENER=(DESCRIPTION=(ADDRESS=(PROTOCOL=TCP)(HOST=192.168.122.155)(PORT=1521)))

ADR_BASE_LISTENER = /db/app/oracle

2、重启oracle监听程序

[oracle@imzcy ~]$ lsnrctl stop    //停止监听程序

[oracle@imzcy ~]$ lsnrctl start    //启动监听程序

 

[oracle@imzcy ~]$ lsnrctl status   //查看监听状态




##oraclce启停

oracle 数据库启停
一. oracle 数据库启停三个阶段

1.1 startup nomount : 启动实例

   (1) 读取参数文件 （2）分配内存 （3）启动后台进程

SQL> startup nomount
ORACLE instance started.

Total System Global Area 849530880 bytes
Fixed Size 1339824 bytes
Variable Size 528485968 bytes
Database Buffers 314572800 bytes
Redo Buffers 5132288 bytes

--startup nomount 此时实例已经启动

SQL> select status from v$instance;

STATUS
------------------------
STARTED

 

--启动至nomount 阶段可以查看如下视图，也仅仅能够查看与实例有关的视图

v$instance 、v$sgastat 、v$process 、v$parameter

--若查看与数据库相关的信息则无法查看，如下查看数据文件及表空间信息


SQL> select name from v$datafile;
select name from v$datafile
*
ERROR at line 1:
ORA-01507: database not mounted


SQL> select name from v$tablespace;
select name from v$tablespace
*
ERROR at line 1:
ORA-01507: database not mounted

 

1.2 alter database mount：挂载数据库

    （1）读取控制文件

    （2）检查控制文件个数是否一致（控制文件有2个），检查控制文件内容是否一致

    （3）检查控制文件数据库名和参数文件记录的数据库名是否一致

             

SQL> select name from v$database;

NAME
----------------------------------------
ORCL

SQL> show parameter name;

NAME                                                    TYPE                               VALUE
------------------------------------ ---------------------------------------- ------------------------------
db_file_name_convert                            string
db_name                                                string                                  orcl
db_unique_name 　　　　　　　　　  string　　　　　　　    　 orcl
global_names 　　　　　　　　　　　 boolean 　　　　　　　 FALSE
instance_name　　　　　　　　　  　 string 　　　　　   　　　 orcl
lock_name_space 　　　　　　　　　 string
log_file_name_convert　　　　　　　 string
service_names 　　　　　　　　　　 string　　　　　　　　　  orcl

    （4）可以查看与数据文件有关的动态视图v$datafile

    （5）可以查看与控制文件有关的动态视图v$controlfile 、v$controlfile_record_section

    （6）可以查看表空间

 

--此时数据库已经启动
SQL> alter database mount;

Database altered.

 

--此时查看数据库open_mode 状态已经挂载

SQL> select open_mode from v$database;

OPEN_MODE
----------------------------------------
MOUNTED

 

--此时则可以查看与数据库有关的信息如下

SQL> select name from v$datafile;

NAME
--------------------------------------------------------------------------------
/u01/app/oracle/oradata/orcl/system01.dbf
/u01/app/oracle/oradata/orcl/sysaux01.dbf
/u01/app/oracle/oradata/orcl/undotbs01.dbf
/u01/app/oracle/oradata/orcl/users01.dbf
/u01/app/oracle/oradata/orcl/example01.dbf

     

--于此同时我们可以通过动态性能视图查看数据库信息，但还不能访问数据库表

SQL> select * from scott.emp;
select * from scott.emp
*
ERROR at line 1:
ORA-01219: database not open: queries allowed on fixed tables/views only

 

1.3 alter database open：打开数据库

 （1）检查数据文件和日志文件

 （2）检查数据文件头ckpt和控制文件ckpt和数据文件last ckpt

SQL> select a.checkpoint_change# "header_ckpt",
2 b.checkpoint_change# "ctl_ckpt",
3 b.last_change# "final_ckpt"
4 from v$datafile_header a, v$datafile b
5 where a.file# = b.file#;

header_ckpt ctl_ckpt final_ckpt
----------- ---------- ----------
890202 890202
890202 890202
890202 890202
890202 890202
890202 890202

 （3）验证控制文件和数据文件的一致性

 

 

SQL> alter database open;

Database altered.

 

1.4 关闭数据库

         shutdown abort :非一致性关库

         shutdown transactional

         shutdown normal

         shutdown immediate :关闭数据库

         采用immediate 关闭模式会出现以下情况：

       （1）oracle db 正在处理的当前SQL语句不会完成；

       （2）oracle 服务器不会等待当前连接到数据库的用户断开连接；

       （3）oracle 服务器会回退活动的事物处理，而且会断开所有连接用户；

       （4）oracle 服务器在关闭并断开数据库后关闭实例，下一次启动不需要不需进行实例恢复。

         

