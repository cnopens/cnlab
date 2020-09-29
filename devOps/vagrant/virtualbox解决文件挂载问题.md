# virtualbox解决文件挂载问题.md

--------------

首先，我们要知道，我们使用

vagrant init centos/7
vagrant up

会有下面的问题：

Vagrant was unable to mount VirtualBox shared folders. This is usually
because the filesystem "vboxsf" is not available. This filesystem is
made available via the VirtualBox Guest Additions and kernel module.
Please verify that these guest additions are properly installed in the
guest. This is not a bug in Vagrant and is usually caused by a faulty
Vagrant box. For context, the command attempted was:

mount -t vboxsf -o uid=1000,gid=1000 home_vagrant_labs /home/vagrant/labs

The error output from the command was:

/sbin/mount.vboxsf: mounting failed with the error: No such device

因为没有在 vagrant box 中安装 VBoxGuestAdditions ( centos/7 base box 就没装，不怪你们)
虚拟机安装VBoxAdditions增强功能，该功能有如下作用：

    实现客户机和主机间的鼠标平滑移动
    与主机实现文件共享
    安装虚拟显卡驱动，实现2D和3D视频图形加速，自动调整客户机分辨率
    支持无缝模式
    与主机共享剪贴板的内容，也就是说直接可以在主机、客户机之间复制、粘贴（不支持文件）
    实现屏幕分辨率的自适应

其他的也不多说

如何安装？

    使用 vagrant 插件
    借助 virtualbox
    自己动手丰衣足食

使用 vagrant 插件

vagrant plugin install vagrant-vbguest
vagrant reload --provision

这个插件的作用，就是自动给你的box装个 VBoxGuestAdditions。全自动，非常好用。但是我就是不想安装。。。
友情提示: 小白用户，就用这个就好了
使用 Virtualbox 界面辅助安装 (基本放弃)

    vagrant up
    不用管挂载错误(因为只是文件没挂载上，但是不影响系统的启动)
    打开virtualbox
    双击启动虚拟机
    启动后点击 Devices 选择最下面的 Insert Guest Additions CD image （放心吧，一般都会失败，怎么重新安装都没用，我也不知道原因，这是为了让你知道有这么个方式）

自己动手丰衣

    Box要求Centos/7, 使用Ubuntu的小伙伴不要慌，官方对你们不要太偏心（来点这个链接 -> Ubuntu）

    vagrant up
    vagrant ssh (别管挂载报错)
    sudo yum update -y (常规操作)
    sudo yum install -y dkms binutils gcc make patch libgomp glibc-headers glibc-devel kernel-headers kernel-devel (这个 dkms yum告诉我没有，也是很气，感兴趣的小伙伴，找到原因请在评论区讨论一下)
    curl -O https://download.virtualbox.org/virtualbox/6.0.8/VBoxGuestAdditions_6.0.8.iso (为啥不用wget，秉承尽量少装软件的原则，为啥，应为你总不能每一个box都来一遍吧，你要有自己的 vagrant base box)
    sudo mkdir /media/VBoxGuestAdditions
    sudo mount -o loop,ro VBoxGuestAdditions_6.0.8.iso /media/VBoxGuestAdditions
    sudo sh /media/VBoxGuestAdditions/VBoxLinuxAdditions.run --nox11

注意： 按理说，但这个地方应该就可以安装上了，但是，实际情况就是报错，再报错。

    不要慌，遇见报错，按照提示走: sudo /sbin/rcvboxadd setup
    然后还是报错（MMP，来个高手看看咋回事，欢迎评论区讨论），不要慌，没事干就重启
    exit (退出登录)
    vagrant reload --provision (--provision可加可不加，看自己情况)
    vagrant ssh
    sudo /sbin/rcvboxadd setup
    重复步骤 7, 8 (再加个报错提示里面的命令)
    不报错的时候就说明安装上了

注意: 说句实话，我也不搞运维，这一堆操作也搞得我头疼，真心希望有个大神搞定这个：一次不能安装好的问题！
自己动手足食

费了老大劲把它给弄好，实在不想再来一遍了！怎么办？我们制作一下 base box！
装好之后呢，实验一下是否能挂载上文件，然后登录到 centos/7，我们继续!

    把之前搞得先干掉

    sudo umount /media/VBoxGuestAdditions
    sudo rmdir /media/VBoxGuestAdditions

    搞定ssh

    cd ~/.ssh/
    rm -rf authorized_keys
    curl -O https://raw.githubusercontent.com/mitchellh/vagrant/master/keys/vagrant.pub
    mv vagrant.pub authorized_keys
    chmod 600 authorized_keys

    删除 history（我把操作流程记在一个空文档中，但是懒得起名字，不小心随手就该删了，写教程的时候一脸懵逼，我想，在我制作的box中有没有history，嘿嘿，还真有!）
    删除挂载的文件
    退出

    执行如下命令打包

    # 查看你的box名称
    vboxmanage list vms
    vagrant package --base 列出来的名称 --output 指定打包后的存放位置及名称
    # 举个例子： vagrant package --base centos_7_swarm-manager_1562576409540_69924 --output /data/download/virtualbox/virtualbox_amor_centos_7_1902.01.box

    打完包直接添加 vagrant box

    vagrant box add --name xxxxx 你制作的基础box
    # 举个例子： vagrant box add --name amor/centos /data/download/virtualbox/virtualbox_amor_centos_7_1902.01.box

    修改 Vagrantfile 把 box 改成你的 box 名称
    vagrant up
    搞定(从此和自己的 base box过上了幸福的生活)

情感危机

突然你脑子一抽经，误删了你的 base box，或者你和你大老婆（系统）闹翻了，要干掉它（重装系统），但是你还是想保留你的 base box（装系统都没它烦人），怎么办？

    登录注册一个 vagrant boxx cloud 账号
    然后在自己的主页，点击创建
