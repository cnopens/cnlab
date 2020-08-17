#vagrant-manual.md
---
Vagrant是一款用来构建虚拟开发环境的外挂工具，可以简化虚拟机配置和管理。它底层支持VirtualBox、VMware、AWS等，非常适合使用php/python/ruby/java语言开发web应用，“代码在我机子上运行没有问题”这种说辞将成为历史。


## install 
1. virtualbox/vmware dowanload 

2. vgrant install 
[vgrant office url:](https://www.vagrantup.com/downloads.html) note: add sys path 

3. prepare box image resources(centos7.2)

vagrant开源社区提供了许多已经打包好的操作系统，我们称之为box。你可以从box下载地址（下文列出），找到你想要的box，当然你也可以自己制作一个。

官方仓库：https://atlas.hashicorp.com/boxes/search

官方镜像：https://vagrantcloud.com/boxes/search

第三方仓库：http://www.vagrantbox.es/（国内）


安装box模板有两种途径。一种是在线下载安装；一种是离线下载，然后添加进去。

1. 在线下载安装

    vagrant box add username/boxname 添加vagrant云仓库里别人做好的box模板。https://app.vagrantup.com/boxes/search在这里可以搜索到，还可以看到详情信息。

例如CentOS官方的两个：
vagrant box add centos/7
vagrant box add centos/6

vagrant box remove xxx/yyy可以移除你安装的box模板。
在线下载安装的时候，会让你选择下哪种虚拟机软件的box模板，我们选择virtualbox版本的
用vagrant命令在线下载完成后，会有一行绿色的Sucessfully显示成功：

2. 离线下载安装

因为主机在国外，有时候网络环境不是，下载慢。那么就可以复制上上个图那里圈起来的链接。复制链接到浏览器下载。（其实我发现，那个地址有的是地址转换，如下图，我们可以看到是从cloud.centos.org下载的，文件名都变成CentOS-x-Vagrant-xxxx.Virtualbox.box这种长格式。而且地址是centos.org的地址说明这个景象是CentOS官方提供的）：

这里贴出目前最新的，你可以自己去用上述方法找到最新的：
离线下载完成后，就可以用命令添加下载好的box模板，例如：
vagrant box add ~/Downloads/CentOS-6-x86_64-Vagrant-1707_01.VirtualBox.box --name centos/6

--name 参数，是指定本地离线添加的文件导入vagrant程序后的名字

列出添加的box模板：
vagrant box list

我们可以看到括号里有版本号。
在线下载安装的，括号里会跟其他人制作时所定义版本号。
离线下载然后本地添加的，括号里默认版本号是0


常用的几个模板
几个常用的官方在线box模板：

centos/7 CentOS 7 x64
centos/6 CentOS 6 x64





4. use case

安装virtualbox，vagrant直接按照平常安装软件一样即可。
安装好后，进入磁盘目录，任意磁盘都行，创建一个管理目录。这里以vagrant目录为例。同时推荐终端工具不适用windows自带的dos，这里推荐xshell工具。
我们添加一个虚拟机，vagrant box add 。我这里把镜像文件放在wamp64下面的

https://github.com/tommy-muehle/puppet-vagrant-boxes/releases/download/1.1.0/centos-7.0-x86_64.box

添加完之后，我们在查看当前的虚拟机，即可看到我们方才添加的虚拟机 vagrant box list

初始化虚拟机 vagrant init centos7。

此时我们查看vagrant目录下面就会多一个名为Vagrantfile的配置文件。

(这个配置文件主要后期我们在对虚拟做修改时，直接修改该文件。)

开启虚拟机 vagrant up







## example for 

#添加box
$ vagrant box add ubuntu_xenial64 xenial-server-cloudimg-amd64-vagrant.box
 
#新增配置文件 vagrantfile
```
# -*- mode: ruby -*-
# vi: set ft=ruby :
 
ENV["LC_ALL"] = "en_US.UTF-8"
 
Vagrant.configure("2") do |config|
config.vm.define "node10" do |node10|
node10.vm.box = "ubuntu_xenial64"
node10.vm.network "private_network", ip: "192.168.2.10"
end
config.vm.hostname = "node10"
config.vm.synced_folder "E:/VM/vagrant_share", "/vagrant_share"
end
```
#安装启动虚拟机
$ vagrant up
#登陆虚拟机
$ vagrant ssh
#新增账户
$ sudo useradd -m oniong -s /bin/bash
$ sudo passwd oniong
#sudo提权
$ vagrant@node10:~$ sudo chmod u+w /etc/sudoers
$ vagrant@node10:~$ sudo vim /etc/sudoers
oniong ALL=(ALL) NOPASSWD: ALL
$ sudo chmod u-w /etc/sudoers
#免密登陆
$ ssh oniong@192.168.2.10
$ ssh-keygen -t rsa
将宿主机的公钥添加到 oniong home .ssh/authorized_keys 中
$ chmod 600 authorized_keys
#ubuntu 镜像加速
https://mirrors.tuna.tsinghua.edu.cn/help/ubuntu/
#docker 镜像加速
https://mirrors.tuna.tsinghua.edu.cn/help/docker-ce/
https://docs.docker.com/install/linux/docker-ce/ubuntu/
$ sudo curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -
$ sudo add-apt-repository \ "deb [arch=amd64] https://mirrors.tuna.tsinghua.edu.cn/docker-ce/linux/ubuntu \ $(lsb_release -cs) \ stable"
$ sudo apt update
$ sudo apt-get install \ apt-transport-https \ ca-certificates \ curl \ gnupg-agent \ software-properties-common
$ sudo apt-get install docker-ce docker-ce-cli containerd.io docker-compose
#docker hub 镜像加速
https://www.daocloud.io/mirror
$ curl -sSL https://get.daocloud.io/daotools/set_mirror.sh | sh -s http://f1361db2.m.daocloud.io
$ sudo systemctl restart docker.service
$ sudo docker run hello-world
$ sudo groupadd docker
$ sudo usermod -aG docker oniong
#注销再登陆
#安装openjdk8
$ sudo apt-get install openjdk-8-jdk-headless
$ java -version
#结束
 
后续操作

#在宿主机上重新打包配置好的虚拟机

$ vagrant package --output ubuntu_16_04_simple.box

#创建另一个文件夹配置启动三个此虚拟机

$ mkdir nodes && cd nodes

$ vagrant box add ubuntu_16_04_simple ubuntu_16_04_simple.box

$ vim vrgrantfile

```


# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure("2") do |config|
    config.vm.define "node11" do |node11|
        node11.vm.box = "ubuntu_16_04_simple"
        node11.vm.network "private_network", ip: "192.168.2.11"
        node11.vm.hostname = "node11"
    end
    config.vm.define "node12" do |node12|
        node12.vm.box = "ubuntu_16_04_simple"
        node12.vm.network "private_network", ip: "192.168.2.12"
        node12.vm.hostname = "node12"
    end
    config.vm.define "node13" do |node13|
        node13.vm.box = "ubuntu_16_04_simple"
        node13.vm.network "private_network", ip: "192.168.2.13"
        node13.vm.hostname = "node13"
    end
    config.vm.synced_folder "E:/VM/vagrant_share", "/vagrant_share"
end

```

$ vagrant up

然后使用 oniong 账号登陆这三个虚拟机