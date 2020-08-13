# index.md
---
## nmap tools usage


##  ssh-copy-id三步实现SSH无密码登录和ssh常用命令

第一步:在本地机器上使用ssh-keygen产生公钥私钥对

    $ ssh-keygen

第二步:用ssh-copy-id将公钥复制到远程机器中

$  ssh-copy-id -i .ssh/id_rsa.pub  用户名字@192.168.x.xxx

注意: ssh-copy-id 将key写到远程机器的 ~/ .ssh/authorized_key.文件中

第三步: 登录到远程机器不用输入密码

    $  ssh 用户名字@192.168.x.xxx
    Last login: Sun Nov 16 17:22:33 2008 from 192.168.1.2

常见问题：

 ssh-copy-id -u eucalyptus -i ~eucalyptus/.ssh/id_rsa.pub ssh 用户名字@192.168.x.xxx 第一次需要密码登录

