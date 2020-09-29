#Vagrant相关配置.md

一、导出自己的Box

导出前，先进行相关配置1、vagrant box打包前的准备   2、VirtrualBox压缩打包

我们在vagrant的虚拟机下面进行了一些操作之后想把它导出作为备份，为的是以后在别的机器上安装完vagrant以后可以立即导入自己熟悉的box进行开发。步骤如下

1、进入到VirtualBox的安装目录下，运行 vboxmanage list vms 命令（此处务必使用cmd，如果使用git，命令为./vboxmanage list vms），可以看出我们的vagrant下的虚拟机列表

2、导出Box。进入到你的vagrant工作目录（即vagrantFile文件所在目录下），我安装在E:\workspace，打开git窗口运行下面命令（注意：复制代码的时候留意--base和--output，这里都是两个英文字符下的横杠，复制的时候有可能会变成中文的横杠或少一个横杠）。

vagrant package –-base newbox_default_1503366286622_12977 –-output ./ubuntu1604.box

vagrant package是导出box的打包命令

–base 代表本地

newbox_default_1503366286622_12977是你要导出的box的名称

–output代表导出

./ubuntu1604.box 表示导出后的box名为ubuntu1604.box，并保存在当前目录下

 

二、导入。

你来到了另一台电脑，你想把你的工作环境完全的copy一份到这台电脑，接下来就很关键了。

1.创建一个你要的工作目录，我的是E:\workspace，把公共打好的包放进来

2.在这个目录下打开Git窗口，输入

vagrant box add 你自定义的别名 包名

ps:添加前最好使用vagrant box list查看一下当前的box列表，如果有跟你要设置的别名一样的box，先删除 vagrant box remove laravel/homestead --box-version 0

3.初始化工作环境

vagrant init

发现你的文件夹中自动生成了一个文件，Vagrantfile。

4.由于你是直接引入自己打的包，而不是vagrant官方提供的包，所以有可能存在一些问题。我们通过编辑Vagrantfile来解决。在Vagrantfile中的“config.vm.box”这一行下，加上这三句

config.ssh.username = "vagrant"
config.ssh.password = "vagrant"
config.ssh.insert_key = false

由于vagrant默认使用private_key登录，此时你有很大的可能是没有private_key的，我们直接改成用户名+密码登录

 5.启动vagrant

vagrant up

 

 

三、高级应用

1)端口转发

    说明：点击  设置->网络->高级-端口转发 就可以看到各个转发的端口配置情况。

        官网文档位置：https://www.vagrantup.com/docs/networking/forwarded_ports.html

        ##############配置代码#################################

        Vagrant.configure("2") do |config|

          config.vm.network "forwarded_port", guest: 80, host: 8888

        end

        ########################################################

    配置说明：

        将虚拟机的80端口转发到宿主机的8888

        config.vm.network "forwarded_port", guest: 80, host: 8888

2)共享目录

    说明：同步宿主主机文件到虚拟机：

        官网文档位置：https://www.vagrantup.com/docs/synced-folders/basic_usage.html

        Windows配置用SMB配置共享目录

        官网文档位置：https://www.vagrantup.com/docs/synced-folders/smb.html

        Linux系列系统用NFS配置共享目录

        官网文档位置：https://www.vagrantup.com/docs/synced-folders/nfs.html

    将写代码目录映射到虚拟机服务器目录【Windows机配置示范】：

    ##############配置代码#################################

        Vagrant.configure("2") do |config|

          config.vm.synced_folder "D://workspace/", "/data/wwwroot/web", type: "smb"

        end

    ########################################################

3)ip配置

        ###########重要提示！################

        #      公有ip不能共享目录          #

        #####################################

    1)私有IP配置

        ##############配置代码#################################

            Vagrant.configure("2") do |config|

              config.vm.network "private_network", ip: "192.168.50.4"

            end

        ########################################################

        好处说明：配置私有ip好处。直接访问私有ip

    2)共有ip配置

        查看宿主机ip: 192.168.1.37

        ##############配置代码#################################

            Vagrant.configure("2") do |config|

              config.vm.network "public_network", ip: "192.168.1.17"

            end

        ########################################################

        和宿主主机一样通过DHCP分配

        ##############配置代码#################################

            Vagrant.configure("2") do |config|

              config.vm.network "public_network",use_dhcp_assigned_default_route: true

            end

        ########################################################   
三、常规配置优化

1）虚拟机名称配置

  config.vm.provider "virtualbox" do |vb|

    vb.cpus = 2                #虚拟机核数

    vb.memory = "1024"          #虚拟机内存

    vb.name = "centos7_lnmp"    #虚拟机名称

  end

2）主机名称配置

    Vagrant.configure("2") do |config|

        config.vm.hostname = "dh2y"

    end

3）nginx和apache同步延时配置

官网文档位置：https://www.vagrantup.com/docs/synced-folders/virtualbox.html

In Nginx:    sendfile off;

In Apache:  EnableSendfile Off  #默认已经关闭

 
修改virutalbox和vagrant的默认目录

virtualbox和vagrant默认都是放到系统C盘中，如果安装的box比较多，很容易打满C盘。这个目标路径的配置是可以修改的：

（1）更改VirtualBox虚拟机映像文件的位置

打开 VirtualBox 程序，点击管理/全局设定菜单项(Ctrl+G), 将常规栏里的默认虚拟电脑位置(M)改为其他磁盘下的路径

将原路径 C:\Users\user_name\.VirtualBox\VirtualBox VMs 下的文件移动到新路径下。

重新启动VirtualBox程序，在虚拟机列表里，以前建立的虚拟机虽然都还在，但已经不可用了，将他们全部删除。

双击打开新路径各个文件夹里的vbox文件，将建立的虚拟机重新导入。

（2）更改vagrant配置文件的位置

将 C:\Users\user_name\.vagrant.d 移动到新的位置

新建环境变量VAGRANT_HOME，并指向新路径

 

参考:

https://imququ.com/post/vagrantup.html

https://github.com/astaxie/go-best-practice/blob/master/ebook/zh/01.1.md
