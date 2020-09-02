# mariadb.md


问题：Ubuntu--MySQL：ERROR 1698 (28000): Access denied for user 'root'@'localhost' 

A: update mysql.user set authentication_string=PASSWORD('123.com'),plugin='mysql_native_password' where user='root'


##检测本机是否已安装mariadb 或者MySQL

    rpm -qa|grep mariadb
    rpm -qa|grep mysql

下面是我的结果，因为我已经安装过了

##如果检测到有类似的安装包，建议先全部删除，重新安装，否则会有一些配置被莫名奇妙的改动，导致各种问题

1、卸载mariadb：

yum remove mariadb

     

2、删除配置文件：

rm -f /etc/my.cnf

3、删除数据目录：

rm -rf /var/lib/mysql/

##安装mariadb

yum install mariadb mariadb-server

这样就安装成功了

##启动mariadb

 service mariadb start       或者

systemctl start mariadb     （启动）

systemctl stop mariadb     （停止）

systemctl restart mariadb     （重启）

systemctl status mariadb     （查看状态）

如果启动时失败，先查看一下是不是又别的程序占用了3306的端口了

netstat -anp|grep 3306

有的话，就杀死，再次启动

五、设置管理员密码

mysql_secure_installation

    首先是设置密码，会提示先输入密码
     
    Enter current password for root (enter for none):<–初次运行直接回车
     
    设置密码
     
    Set root password? [Y/n] <– 是否设置root用户密码，输入y并回车或直接回车
    New password: <– 设置root用户的密码
    Re-enter new password: <– 再输入一次你设置的密码
     
    其他配置
     
    Remove anonymous users? [Y/n] <– 是否删除匿名用户，回车
     
    Disallow root login remotely? [Y/n] <–是否禁止root远程登录,回车（后面授权配置）
     
    Remove test database and access to it? [Y/n] <– 是否删除test数据库，回车
     
    Reload privilege tables now? [Y/n] <– 是否重新加载权限表，回车

六、设置其他IP的电脑也可以连接数据库

mysql -uroot -p

输入密码：

use mysql;

select Host,User from user;

默认Host 只有一个localhost.

3、给该用户添加权限

    root账户中的host项是localhost表示该账号只能进行本地登录，我们需要修改权限，输入命令：
     
    GRANT ALL PRIVILEGES ON *.* TO 'root'@'%' IDENTIFIED BY 'password' WITH GRANT OPTION;
     
    修改权限。%表示针对所有IP，password表示将用这个密码登录root用户，如果想只让某个IP段的主机连接，可以修改为：
     
    GRANT ALL PRIVILEGES ON *.* TO 'root'@'192.168.71.%' IDENTIFIED BY 'my-new-password' WITH GRANT OPTION;

4、刷新权限

flush privileges;

（注：测试可以用软件连接测试，可以连接成功了再关闭命令行交互）

5、成功后，重启数据库

systemctl restart mariadb

6、设置开机启动（可选）

systemctl enable mariadb


##远程链接错误问题

 Centos7下无法远程连接mysql数据库的原因与解决

有两种原因

    数据库没有授权
    服务器防火墙没有开放3306端口

###数据库没有授权

对于mysql数据库没有授权，只需要用一条命令就可以了

 GRANT ALL PRIVILEGES ON *.* TO 'root'@'%' IDENTIFIED BY '123456' WITH GRANT OPTION;

输入后使修改生效还需要下面的语句

FLUSH PRIVILEGES;

###服务器防火墙没有开放3306端口 

centos 有两种防火墙 FirewallD和iptables防火墙

centos7 使用的是FirewallD防火墙。

FirewallD 是 iptables 的前端控制器，用于实现持久的网络流量规则。它提供命令行和图形界面，在大多数 Linux 发行版的仓库中都有。与直接控制 iptables 相比，使用 FirewallD 有两个主要区别：

FirewallD 使用区域和服务而不是链式规则。
它动态管理规则集，允许更新规则而不破坏现有会话和连接。
FirewallD 是 iptables 的一个封装，可以让你更容易地管理 iptables 规则 - 它并不是 iptables 的替代品。虽然 iptables 命令仍可用于 FirewallD，但建议使用 FirewallD 时仅使用 FirewallD 命令。
1.FirewallD防火墙开放3306端口

firewall-cmd --zone=public --add-port=3306/tcp --permanent

命令含义： 
- zone #作用域 
- add-port=3306/tcp #添加端口，格式为：端口/通讯协议 
- permanent #永久生效，没有此参数重启后失效

重启防火墙

systemctl restart firewalld.service

2.iptables 开发3306端口

/sbin/iptables -I INPUT -p tcp -dport 3306 -j ACCEPT

/etc/rc.d/init.d/iptables save

