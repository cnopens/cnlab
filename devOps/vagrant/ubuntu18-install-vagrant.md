# ubuntu18-install-vagrant.md

## 在 Ubuntu 18.04 环境下，安装 vagrant 十分简单，只需执行以下命令： 

    # apt install virtualbox -y
    # wget -c https://releases.hashicorp.com/vagrant/2.2.10/vagrant_2.2.10_x86_64.deb
    # dpkg -i vagrant_2.2.10_x86_64.deb
    # vagrant plugin install vagrant-vbguest

验证是否安装成功：

去官方网站下载一个 box 镜像，http://www.vagrantbox.es/


比如我这里使用：

Ubuntu 15.04（https://github.com/kraksoft/vagrant-box-ubuntu/releases/download/15.04/ubuntu-15.04-amd64.box）

    # vagrant box add ubuntu1504 file:///opt/boxes/ubuntu-15.04-amd64.box
    # vagrant init ubuntu1404 
    # vagrant up



