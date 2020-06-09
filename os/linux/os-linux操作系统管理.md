# linux操作系统管理.md
---
## Ubuntu内部错误信息及处理

实际情况并不是Ubuntu容易出现内部错误，而是一旦程序崩溃过一次，就会生成一个.crash文件，记录程序崩溃信息并保存在目录:
/var/crash/
只要不处理，每次开机都会提示你有错误。也就是说：报错并不一定是出现了什么错误，而是曾经出现过错误没有处理。

## 解决方案
(1) 临时关闭Apport错误报告

要临时关闭Apport，使用命令
sudo service apport stop
注意:重启Ubuntu系统后Apport会继续开启
(2) 永久关闭Apport错误报告

要永久关闭Apport，编辑/etc/default/apport，修改下列参数
enabled=0
重启Ubuntu系统后，Apport将会自动关闭
如果不再使用Apport，可以完全移除该服务
sudo apt-get purge apport
(3) 简单处理: 删除.crash文件

到/var/crash/目录查看崩溃文件，如果不是什么大问题(通常都没什么大问题)，删除该目录下的崩溃文件，之后就不会再报错误了.

## SD