#vgrant命令学习.md

##常用命令：
----
命令 	作用
vagrant box add [box_name] [box_path] 	添加box的操作
vagrant init 	初始化box的操作，会生成vagrant的配置文件Vagrantfile
vagrant up [vm_name] 	启动指定环境 （默认为本地）
0. （默认）在VirtualBox上启动
1. 在vmware上：
vagrant up –provider=vmware_fusion [vm_name]
2. 在AWS上：
vagrant up –provider=aws [vm_name]
vagrant ssh [vm_name] 	通过ssh登录指定环境所在虚拟机（默认为本地）
通常情况下，vagrant在创建虚拟机的时候，内置了1个用户：
username:vagrant
password:vagrant
vagrant halt [vm_name] 	虚拟机关闭 （默认为本地）
vagrant suspend [vm_name] 	虚拟机挂起 （默认为本地）
vagrant resume 	恢复本地环境
vagrant reload [vm_name] 	虚拟机重启 （默认为本地）
修改了Vagrantfile后，使之生效（相当于先 halt，再 up）
vagrant destroy [vm_id] 	彻底销毁虚拟机 （默认销毁本地）
虚拟机删除后，所有在虚拟机中做的改动都不再存在，慎用。
vagrant box list 	显示当前已经添加的box列表
vagrant box remove [box_name] 	删除相应的box
vagrant package 	打包命令，可以把当前的运行的虚拟机环境进行打包
vagrant plugin 	用于安装卸载插件
vagrant status 	获取当前虚拟机的状态
vagrant global-status 	显示当前用户Vagrant的所有环境状态

## 快照管理：
vagrant snapshot save init


vagrant snapshot list

命令及作用
---

命令 	作用
vagrant snapshot save your_snapshot_name 	新建快照
vagrant snapshot list 	查看快照
vagrant snapshot restore your_snapshot_name 	恢复快照
vagrant snapshot delete your_snapshot_name 	删除快照


## 配置镜像位置

1. 修改virtualbox默认存储位置
略过
2. 配置vagrant的镜像存储位置
vagrant以我安装的ubuntu18为例默认位置：/home/cnopens/.vagrant.d

reconfig: VAGRANT_HOME : /media/cnopens/DevWspace/Vagrant_boxes

## 共享配置
vagrant提供了将本机目录挂载到虚拟机目录下的功能，默认是将vagrant配置文件所在目录挂载到虚拟机/vagrant目录下。

打开配置文件Vagrantfile，找到如下配置项：
config.vm.synced_folder

```配置项如下：
config.vm.synced_folder   
   "your_folder"(必须)   //物理机目录，可以是绝对地址或相对地址，相对地址是指相对与vagrant配置文件所在目录
  ,"vm_folder(必须)"    // 挂载到虚拟机上的目录地址
  ,create(boolean)--可选     //默认为false，若配置为true，挂载到虚拟机上的目录若不存在则自动创建
  ,disabled(boolean):--可选   //默认为false，若为true,则禁用该项挂载
  ,owner(string):'www'--可选   //虚拟机系统下文件所有者(确保系统下有该用户，否则会报错)，默认为vagrant
  ,group(string):'www'--可选   //虚拟机系统下文件所有组( (确保系统下有该用户组，否则会报错)，默认为vagrant
  ,mount_options(array):["dmode=775","fmode=664"]--可选  dmode配置目录权限，fmode配置文件权限  //默认权限777
  ,type(string):--可选     //指定文件共享方式，例如：'nfs'，vagrant默认根据系统环境选择最佳的文件共享方式
```
例子
```
  config.vm.synced_folder
        "D:/www/code"
        , "/code"
        , owner:"www"
        , group:"www"
        ,create:true
        ,mount_options:["dmode=775","fmode=664"]

  config.vm.synced_folder ".","/vagrant",disabled:true //禁用vagrant的默认共享目录
```

## 网络配置
vagrant提供了三种网络配置方式：端口转发(默认)、私有网络、公有网络，可以在 配置文件 Vagrantfile 进行网络配置，推荐使用私有网络。

### 端口转发(forwarded ports)

1. 定义
    端口转发指把宿主机的端口映射到虚拟机的某一个端口上，访问宿主机端口时，请求实际是被转发到虚拟机上指定端口的。
    注：宿主机指运行虚拟机的物理机。
2. 优点
    容易实现外网访问虚拟机
3. 缺点
    如果端口较少需要映射很容易，但是端口比较多时，就比较麻烦，例如：MySQL，redis，nginx等服务。
    不支持在宿主机使用小于1024的端口来转发，例如：不能使用SSL的443端口来进行https连接。
4. 配置
在配置文件Vagrantfile下做如下编辑

Vagrant.configure("2") do |config|
  config.vm.network  
      "forwarded_port"(必须) //端口转发标识
      , guest(必须): //虚拟机端口
      , host(必须): //宿主机端口，值必须大于1024
      ,guest_ip(可选): //虚拟机端口绑定虚拟机ip地址
      ,host_ip(可选): //虚拟机端口绑定宿主机ip
      ,protocol(可选)://指定通信协议，可以使用tcp/udp，默认tcp
      ,auto_correct(可选)://true/false，若配置为true，则每次开启虚拟机的时候自动检查是否存在端口冲突
end

注：若guest_ip和host_ip两项配置为空，则局域网下的所有设备都可以访问该虚拟机。
示例配置，如下：
Vagrant.configure("2") do |config|
  config.vm.network "forwarded_port", guest: 80, host: 8080,
    auto_correct: true
end


### 私有网络(private networks)

    定义
    私有网络是指只有宿主机可以访问虚拟机，如果多个虚拟机设定在同一个网段也可以互相访问。
    优点
    安全，只有自己可以访问
    缺点
    团队成员不能访问你的虚拟机
    配置
    配置如下：

 config.vm.network 
                "private_network"//必须 ，私有网络标识
                , ip: "192.168.33.10"
注：私有ip可以自行指定   

### 公有网络(public networks)

    定义
    公有网络是指设置虚拟机和宿主机有相同的网络配置。

    优点
    方便团队协作，别人可以访问你的虚拟机

    缺点
    只有在有网络的情况下才能访问虚拟机

    配置
```
Vagrant.configure("2") do |config|
  config.vm.network 
  "public_network" //必须 公有网络标识
  ,ip(string):  //可选，配置静态ip
  ,bridge(string/array): "en1: Wi-Fi (AirPort)"//可选，设置桥接的网卡
end
```

## Resources
vagrant 中文文档：http://tangbaoping.github.io/vagrant_doc_zh/v2/
