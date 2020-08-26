# mysql-tips.md

## install mariadb 

### install method

sudo apt install mariadb-server

### 
GRANT ALL PRIVILEGES ON *.* TO 'root'@'%' IDENTIFIED BY '123.com' WITH GRANT OPTION;
flush privileges;



47.ip administrator@vsphere.local null 192.168.31.112 p3-ftpserver-t28 'Windows2008-template' 2IaaS_Cluster 192.168.31.3 255.255.255.0 8.8.8.8 192.168.31.1 30 2 6 'Test' 1qaz@WSX



## mysql export/import

1. 导出
数据和表结构：
mysqldump -u用户名 -p密码 数据库名 > 数据库名.sql
mysqldump -uroot -p dbname > dbname .sql
敲回车后会提示输入密码

只导出表结构
mysqldump -u用户名 -p密码 -d 数据库名 > 数据库名.sql
mysqldump -uroot -p -d dbname > dbname .sql

2. 导入
mysql>create database dbname ;
导入数据库
method1：
（1）选择数据库
mysql>use dbname ;
（2）设置数据库编码
mysql>set names utf8;
（3）导入数据（注意sql文件的路径）
mysql>source /home/xxxx/dbname .sql;
method2：
mysql -u用户名 -p密码 数据库名 < 数据库名.sql

 
3. 导入存储过程

前面加上：DELIMITER // 

后面加上：//
