#vagrant-qa.md

vagrant is currently configured to create VirtualBox synced folders with
the `SharedFoldersEnableSymlinksCreate` option enabled. If the Vagrant
guest is not trusted, you may want to disable this option. For more
information on this option, please refer to the VirtualBox manual:

  https://www.virtualbox.org/manual/ch04.html#sharedfolders

This option can be disabled globally with an environment variable:

  VAGRANT_DISABLE_VBOXSYMLINKCREATE=1

or on a per folder basis within the Vagrantfile:

  config.vm.synced_folder '/host/path', '/guest/path', SharedFoldersEnableSymlinksCreate: false




Vagrant was unable to mount VirtualBox shared folders. This is usually
because the filesystem "vboxsf" is not available. This filesystem is
made available via the VirtualBox Guest Additions and kernel module.
Please verify that these guest additions are properly installed in the
guest. This is not a bug in Vagrant and is usually caused by a faulty
Vagrant box. For context, the command attempted was:

mount -t vboxsf -o uid=1000,gid=1000 vagrant /vagrant

The error output from the command was:

mount: unknown filesystem type 'vboxsf'


A: vagrant plugin install vagrant-vbguest



## vagrant 配置虚拟机名称


如果我们不在vagrant init 命令生成的vagrantfile文件中声明虚拟机的名字的话，一般会默认给我们指定一个名字，指定的方法：

  config.vm.provider "virtualbox" do |v|
      v.name = "worker1"
  end

在vagrantfile中添加上面的语句，然后再运行vagrant up


## vagrant 能同时配置2个以上网络吗？
