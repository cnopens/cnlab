# mariadb.md


问题：Ubuntu--MySQL：ERROR 1698 (28000): Access denied for user 'root'@'localhost' 

A: update mysql.user set authentication_string=PASSWORD('123.com'),plugin='mysql_native_password' where user='root'

