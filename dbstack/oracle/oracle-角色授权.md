oracle-角色授权.mds

一般使用oracle数据库时，用HR和SCOTT用户登录居多。在数据库中同样可以自己创建用户，然后分配相应的权限

例如我们要创建一个用户名为kobe的用户，密码为kobe123，则可以使用如下SQL语句：

SQL> create USER kobe identified by kobe123;

系统提示了如下的错误：

这是由于当前登录的用户是SCOTT用户，创建用户时应该以sys管理员身份登录

这样，用户kobe就创建完成了。要是想修改新创建用户的密码，使用如下命令即可：

ALTER USER kobe

IDENTIFIED BY 新密码；

 

接下来要给用户授权，具体语法：

GRANT 权限1[，权限2...]

TO 用户名；

权限有系统权限，角色权限和对象权限

系统权限：
允许用户登录： create session
允许创建表：create table
允许创建视图：create view
允许创建序列：create sequence
允许创建过程：create procedur
例如，将前两个权限授予kobe用户，使用语句

GRANT create session,create table TO kobe;

 

角色权限：
数据库中系统权限，用户权限很多，如果针对每个用户逐个分配每个权限，费时又费力。因此，可以将相关的一系列权限组成一个命名的组，这个权限组就是角色。角色拥有的权限就是角色权限，将角色权限授予用户，则这个角色权限拥有的权限就全部授予了这个用户。一个用户可以被指定为多个角色，则同时拥有这多个角色所拥有的全部权限。例如：

create role dev;           //创建dev角色

grant create table ,create view,create sequence to dev;     //给dev角色授权

grant dev to kobe;              //把dev中的权限授予kobe用户

对象权限：
对象权限是在指定的表，视图，序列和过程等对象上执行特定操作的权限，包括insert，delete，update，select以及index，alter，references（引用权限）和execute（执行权限）。但并不是每个对象都可以被授予这些权限

例如，给kobe用户授予针对雇员表emp的插入和查询权限该如何做？

雇员表emp的所有者是SCOTT用户，因此用SCOTT用户访问oracle数据库，完成对kobe用户的授权工作

SQL语句如下：

grant select, insert
on emp
to kobe
此时授权完成。执行下面的SQL语句：

select * from emp;
提示“表或视图不存在”。这是因为在数据库中，方案（schema）是管理数据库对象的逻辑结构，一个数据库对象的全称应该是：“方案名.对象名”（默认情况下方案名等于用户的名字），之前不使用方案名的原因是用户操作的数据库对象是本用户的对象，方案名可以省略。而现在是用kobe用户访问数据库，需要操作SCOTT用户的emp对象，所以必须要写全名。正确的SQL语句如下：

select * from Scott.emp;
每次访问数据库对象时都得带着方案名，若是觉得不舒服，可创建同义词（SYNONYM，需要有创建同义词的权限），通过同义词进行访问，SQL语句如下：

create SYNONYM emp for Scott.emp;
select * from emp;
 

撤销权限：
使用revoke语句撤销已经授予用户的权限

例如要撤销给kobe用户针对雇员表emp的插入权限（scott用户身份下操作），SQL语句如下：

revoke insert
on emp
from Kobe
撤销kobe用户创建序列的系统权限的功能，SQL语句如下（用系统管理员账户）：

revoke create sequence
from Kobe
 