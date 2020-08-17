#vagrant实战虚拟机创建.md

## 在线资源：
https://app.vagrantup.com/boxes/search

[清华镜像] （https://mirror.tuna.tsinghua.edu.cn/help/virtualbox/）

http://vault.centos.org/6.4/isos/x86_64/

## Vagrant创建虚拟机生命周期
 启动(up)、关闭(halt)、暂停(suspend)、恢复(resume)、重载(reload)虚机

我们可以看到下面有一行提示几个命令的作用：

    vagrant halt 关闭(halt)虚机
    vagrant up 启动(up)虚机
    vagrant suspend 暂停(suspend)虚机

还有两条用的也很多：

    vagrant resume 从暂停中恢复（resume）虚机
    vagrant reload 重载(reload)虚拟机，也就是重启虚拟机(在Vmware类的软件里也叫做重置，都是一个意思）。如果修改了配置文件，reload会按照配置文件来更改虚拟机的一些配置。

登录虚机

vagrant ssh可以登录虚拟机，是以vagrant用户登录的，我们可以看vagrant init那张图，可以看到有生成秘钥对并上传公钥到虚拟机了，所以vagrant ssh可以无秘钥登录。

销毁(destory)虚机
vagrant destory

tips: 上述的命令都是基于一台虚拟机的操作，所以相对比较简单，多台是在上述命令基础上加一些参数而已。


## 简单默认配置创建

离线下载完成后，就可以用命令添加下载好的box模板，例如：

vagrant box add ~/Downloads/CentOS-6-x86_64-Vagrant-1707_01.VirtualBox.box --name centos/6



在线安装：
vagrant box add --name centos/7
https://github.com/tommy-muehle/puppet-vagrant-boxes/releases/download/1.1.0/centos-7.0-x86_64.box