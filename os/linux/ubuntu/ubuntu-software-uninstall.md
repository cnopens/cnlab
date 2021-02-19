#ubuntu-software-uninstall.md

## software uninstall
----
输入命令：dpkg --list 浏览并找到已安装的程序名字，baidunetdisk

输入命令：
不完全卸载：sudo apt-get remove baidunetdisk
完全卸载：sudo apt-get --purge remove baidunetdisk




## apt update

samba depends on samba-common (= 2:4.7.6+dfsg~ubuntu-0ubuntu2.19); however:
 Package samba-common is not configured yet.

 dpkg: error processing package samba (--configure):
 dependency problems - leaving unconfigured
 dpkg: dependency problems prevent configuration of samba-common-bin:
 samba-common-bin depends on samba-common (= 2:4.7.6+dfsg~ubuntu-0ubuntu2.19); however:
 Package samba-common is not configured yet.



具体出现的错误问题如下

penguin@penguin-dev:~$ sudo apt-get install samba
正在读取软件包列表... 完成
正在分析软件包的依赖关系树
正在读取状态信息... 完成
有一些软件包无法被安装。如果您用的是 unstable 发行版，这也许是
因为系统无法达到您要求的状态造成的。该版本中可能会有一些您需要的软件
包尚未被创建或是它们已被从新到(Incoming)目录移出。
下列信息可能会对解决问题有所帮助：

下列软件包有未满足的依赖关系：
 samba : 依赖: samba-common (= 2:3.6.3-2ubuntu2) 但是 2:3.6.3-2ubuntu2.11 正要被安装
         依赖: libwbclient0 (= 2:3.6.3-2ubuntu2) 但是 2:3.6.3-2ubuntu2.11 正要被安装
         推荐: tdb-tools 但是它将不会被安装
E: 无法修正错误，因为您要求某些软件包保持现状，就是它们破坏了软件包间的依赖关系。

  

解决方案如下

#在安装samba之前我们先更新一下
sudo apt-get update
#安装上面出现错误提示的依赖版本软件包
#注意这里我们直接指定版本安装 samba-common=2:3.6.3-2ubuntu2 指定版本这里不要有多余的空格
sudo apt-get install samba-common=2:3.6.3-2ubuntu2
------------------------------------------------------------------
sudo apt-get install samba-common=2:4.7.6+dfsg~ubuntu-0ubuntu2.19


sudo apt-get install samba-common-bin=2:4.7.6+dfsg~ubuntu-0ubuntu2.19


#注意这里我们直接指定版本安装 libwbclient0=2:3.6.3-2ubuntu2 指定版本这里不要有多余的空格
sudo apt-get install libwbclient0=2:3.6.3-2ubuntu2


#上面的错误有提示多少个依赖包就按照上面的指令安装多少个就好了
sudo apt-get install samba

   

修改配置文件

sudo vim /etc/samba/smb.conf

#在配置文件最后一行增加共享文件夹内容
[vagrant]
  comment = vagrant
  #指定共享文件夹的路径
  path = /vagrant
  writable = yes
  browseable = yes
  guest ok = yes
  #指定共享文件夹账户权限为 penguin 用户，具体视自己账户为准
  force user = penguin

#执行命令设置账户访问密码，我的账户是 penguin 具体视自己账户为准
sudo smbpasswd -a penguin
#上面的命令执行完毕后会让你输入 2 次密码

#最后执行命令重启 Samba 服务即可
sudo /etc/init.d/smbd restart


## Ubuntu 18.04安装Charles 

wget -q -O - https://www.charlesproxy.com/packages/apt/PublicKey | sudo apt-key add -
sudo sh -c 'echo deb https://www.charlesproxy.com/packages/apt/ charles-proxy main > /etc/apt/sources.list.d/charles.list'
sudo apt update
sudo apt install charles-proxy


Registered Name: https://zhile.io
License Key: 48891cf209c6d32bf4


## ubuntu1804 update软件失败解决步骤

1，执行-f dist-upgrade
2，执行apt --fix-broken install
3，再执行apt-get update
4，最后执行apt-get upgrade