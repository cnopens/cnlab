#docker-access-mysql-host-proplem.md

grant all privileges on *.* to 'root'@'172.18.16.108' identified by '123.com' with grant option;

grant all privileges on *.* to 'root'@'%' identified by 'pswd' with grant option;

flush privileges;