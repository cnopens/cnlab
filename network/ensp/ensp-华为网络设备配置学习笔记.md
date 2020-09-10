#ensp-华为网络设备配置学习笔记.md

system-view
sysname AR1





## ensp配置注意事项？

1. 设备配置丢失问题？
A: 系统视图配置完毕后，quit,进入普通视图，输入save对配置进行保存.


2. 设备配置完毕后如何激活？
A: undo shutdown 


3. interface Ethernet 0/0/0 not shutdown?
A: 指的是以太网端口0下面的子端口0


4. 配置静态路由跨网段联通，如何检测联通？
A: ip route-static 192.168.0.24 192.168.1.2
dis ip routing-table (ping dest ip)

5. 配置路由时如何切换接口？
A： int g 0/0/1