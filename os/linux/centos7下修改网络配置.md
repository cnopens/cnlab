#centos7 下修改网络配置
---

修改ip地址
编辑 /etc/sysconfig/network-scripts/ifcfg-eth0

---

TYPE=Ethernet 
BOOTPROTO=static 静态ip 
DEFROUTE=yes 
IPV4_FAILURE_FATAL=no 
IPV6INIT=yes 
IPV6_AUTOCONF=yes 
IPV6_DEFROUTE=yes 
IPV6_FAILURE_FATAL=no 
NAME=eno16777736 
UUID=34bbe4fa-f0b9-4ced-828a-f7f7e1094e4a 
DEVICE=eno16777736 
ONBOOT=yes 
PEERDNS=yes 
PEERROUTES=yes 
IPV6_PEERDNS=yes 
IPV6_PEERROUTES=yes 
IPADDR=192.168.179.3 ip地址 
NETMASK=255.255.255.0 子网掩码 
GATEWAY=192.168.179.2 网关

运行 service network restart

修改dns地址
编辑/etc/resolv.conf 
修改文件内容 nameserver 114.114.114.114

常用dns地址
114.114.114.114 
114.114.115.115 
223.5.5.5 阿里 
223.6.6.6 阿里 
180.76.76.76 