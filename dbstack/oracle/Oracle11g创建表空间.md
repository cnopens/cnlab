#Oracle11g创建表空间.md

oracle 数据库就是指的oracle 整体，
oracle建立好以后，实际上oracle是一个一个的DBF文件，然后N个DBF文件组成一个表空间
你的表就建立在表空间下，比如我举个例子：
一个数据库叫jack，
jack下用户使用的表空间有3个： users , abc, jacc
其中
users由d:\1.dbf组成
abc由d:\11.dbf d:\22.dbf组成
jacc 由 d:\jacc.dbf组成
你建的表可以选择放在这3个表空间的任意一个里（如果不写，就放在你这个用户的默认表空间里，一般都是users，这个表空间是系统自己建立的）
临时表空间你也可以用，但是只能将临时表放在里面，临时表空间主要放置一些临时数据，比如你查询一个复杂的sql语句，系统会将中间数据放在临时表空间里暂存
临时表空间会自己删除（可以选择会话结束就删除）

smallfile tablespace:小文件表空间是创建数据库的默认选项。
maxsize unlimited 表空间大小最大值没有限制
segment space management auto:表示段空间管理为自动

索引：把表数据和索引分离，分别存储在各自的表空间/物理磁盘上。在检索过程中，oracle就可以在不同的表空间中分别并行检索索引键值和数据，提高查询效率。

查看所有已创建的表空间及其对应的数据文件：select file_name,tablespace_name,status from dba_data_files;

insert into shoes(picId) values('524811')

查询数据字典：
select tablespace_name,status from dba_tablespaces;
alter tablespace zq tablespace group tbs_temp_group;
alter tablespace b tablespace group tbs_temp_group;
select * from dba_tablespace_group

reuse:重用制定路径下原有的文件。
blocksize：数据块的大小，默认为：8kb
logging:将日志写入日志文件。
online,offline 联机脱机
extent management local 区管理本地化
segment space management 段空间管理方式
flashback 表空间是否可闪回
当表空间使用本地管理且段空间为字段管理时，表空间才能使用大文件表空间。（本地管理的撤销表空间和临时表空间除外。）
quota unlimited on tbs_main 表示用户在tbs_main上可使用的限额无限制。
account unlimited on tbs_main 创建的用户状态为未锁。

SET SERVEROUTPUT ON; ，只有将serveroutput变量设为on后，信息才能显示在屏幕上。

/*分为四步 /
/第1步：创建临时表空间 */

create temporary tablespace yuhang_temp
tempfile 'D:\oracledata\yuhang_temp.dbf'
size 50m 
autoextend on 
next 50m maxsize 20480m 
extent management local; 
/*第2步：创建数据表空间 */

create tablespace yuhang_data 
logging    /*logging 是对象的属性，创建数据库对象时，oracle 将日志信息记录到练级重做日志文件中。代表空间类型为永久型 */
datafile 'D:\oracledata\yuhang_data.dbf'
size 50m 
autoextend on       /*autoextend on    表空间大小不够用时自动扩展*/
next 50m maxsize 20480m    　/*next 50m 自动扩展增量为50MB */
extent management local;       /*extent management local   代表管理方式为本地*/
/*第3步：创建用户并指定表空间 */

create user yuhang identified by yuhang 
default tablespace yuhang_data 
temporary tablespace yuhang_temp; 
/*第4步：给用户授予权限 */

grant connect,resource,dba to yuhang;


## 表空间删除操作

以system用户登录，查找需要删除的用户：

--查找用户

select  *　from dba_users;

--查找工作空间的路径
select * from dba_data_files; 

--删除用户
drop user 用户名称 cascade;
--删除表空间
drop tablespace 表空间名称 including contents and datafiles cascade constraint;

例如：删除用户名成为xxx，表空间名称为xxx

--删除用户，及级联关系也删除掉
drop user xxx cascade;
--删除表空间，及对应的表空间文件也删除掉
drop tablespace xxx including contents and datafiles cascade constraint;


示例

shutdown immediate ;//关闭连接
startup//重启
drop user aly cascade;//删除aly用户
drop tablespace aly including contents and datafiles cascade constraint;//删除表空间
drop tablespace aly_temp including contents and datafiles cascade constraint;