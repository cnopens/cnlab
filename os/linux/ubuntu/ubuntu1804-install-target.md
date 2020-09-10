# ubuntu1804-install-target.md

## user privillage rise  -oK

Node :diff from centoS 
usermod -aG sudo cnopens 


要确保用户具有sudo权限，请运行whoami命令：
$ sudo whoami
系统将提示你输入密码，如果用户有sudo访问权限，该命令将打印“root”：
root

## youdao

 wget https://github.com/yomun/youdaodict_5.5/raw/master/youdao-dict_1.1.1-0~ubuntu_amd64.deb


## sougou


@todo 


## 7z
apt-get install p7zip-full
解压7z：使用方法：7z x file file是你要解压的文件名

---
## source.list config
------------------------------------
	Ubuntu 18.04 TLS版本阿里云镜像源：
	# https://opsx.alibaba.com/mirror
	deb https://mirrors.aliyun.com/ubuntu/ bionic main restricted universe multiverse 
	deb https://mirrors.aliyun.com/ubuntu/ bionic-security main restricted universe multiverse 
	deb https://mirrors.aliyun.com/ubuntu/ bionic-updates main restricted universe multiverse 
	deb https://mirrors.aliyun.com/ubuntu/ bionic-proposed main restricted universe multiverse 
	deb https://mirrors.aliyun.com/ubuntu/ bionic-backports main restricted universe multiverse 

	# 仿照清华镜像源，注释了源码镜像以提高 apt update 速度，如有需要可自行取消注释
	# deb-src https://mirrors.aliyun.com/ubuntu/ xenial main restricted universe multiverse 
	# deb-src https://mirrors.aliyun.com/ubuntu/ xenial-security main restricted universe multiverse 
	# deb-src https://mirrors.aliyun.com/ubuntu/ xenial-updates main restricted universe multiverse 
	# deb-src https://mirrors.aliyun.com/ubuntu/ xenial-proposed main restricted universe multiverse 
	# deb-src https://mirrors.aliyun.com/ubuntu/ xenial-backports main restricted universe multiverse

	Ubuntu 18.04 TLS版本清华镜像源：
	# https://mirrors.tuna.tsinghua.edu.cn/help/ubuntu/
	# 默认注释了源码镜像以提高 apt update 速度，如有需要可自行取消注释
	deb https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ bionic main restricted universe multiverse
	# deb-src https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ bionic main restricted universe multiverse
	deb https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ bionic-updates main restricted universe multiverse
	# deb-src https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ bionic-updates main restricted universe multiverse
	deb https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ bionic-backports main restricted universe multiverse
	# deb-src https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ bionic-backports main restricted universe multiverse
	deb https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ bionic-security main restricted universe multiverse
	# deb-src https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ bionic-security main restricted universe multiverse

	# 预发布软件源，不建议启用
	# deb https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ bionic-proposed main restricted universe multiverse
	# deb-src https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ bionic-proposed main restricted universe multiverse

	sudo apt-get update
	sudo apt-get upgrade
	sudo apt-get install build-essential

## sublime3 install 

------------------------------------
1. fetch 
wget -qO - https://download.sublimetext.com/sublimehq-pub.gpg | sudo apt-key add -

2. add source.list 
echo "deb https://download.sublimetext.com/ apt/stable/" | sudo tee /etc/apt/sources.list.d/sublime-text.list

3. install 
sudo apt update && sudo apt install sublime-text

reomve :

sudo apt remove --autoremove sublime-text

## java config (Note: centos jvm(usr/local/jvm/ ubuntu:/usr/lib/jvm/))
------------------------------------
第一种是到java官网下载，默认是下载到根目录下的下载，我们可以通过
cp /下载/jdk-8u181-linux-x64.tar.gz /usr/lib/jvm/将下载好的压缩包放置在jvm目录下，然后通过命令root@cnos-ThinkPad-Edge-E431:/home/cnos# tar zxvf jdk-8u171-linux-x64.tar.gz解压

第二种，我们可以在/usr/lib/jvm下通过命令直接下载：

  	wget --no-check-certificate --no-cookies --header "Cookie: oraclelicense=accept-securebackup-cookie" download.oracle.com/otn-pub/java/jdk/8u111-b14/jdk-8u111-linux-x64.tar.gz

    安装jdk
    通过这个命令：root@cnlab:/home/cnos# sudo vim ~/.bashrc，如果提示vim找不到命令，点此链接。如果没有提示，点击键盘的 i 进行编辑，按 Esc切换指令，输入wq保存退出。

	export JAVA_HOME=/usr/lib/jvm/jdk1.8.0_171 //这是你安装的解压后的jdk的文件名
	export JRE_HOME=${JAVA_HOME}/jre   
	export CLASSPATH=.:${JAVA_HOME}/lib:${JRE_HOME}/lib   
	export PATH=${JAVA_HOME}/bin:$PATH

我们也可以这样写：
	JAVA_HOME=/usr/lib/jvm/jdk1.8.0_171 //这是你安装的解压后的jdk的文件名
	JRE_HOME=${JAVA_HOME}/jre   
	CLASSPATH=.:${JAVA_HOME}/lib:${JRE_HOME}/lib   
	PATH=${JAVA_HOME}/bin:$PATH
	export JAVA_HOME JRE_HOME CLASSPATH PATH

Effect:

source ~/.bashrc


### jdk 多版本安装
----------------------

1. 下载

2. /usr/lib/jvm/目录下，然后解压 example :/usr/lib/jvm/jdk8 |jd11 | jdk7


然后输入这几条指令

	sudo update-alternatives –install /usr/bin/java java /usr/lib/jvm/jdk1.8.0_171/bin/java 300 
	sudo update-alternatives –install /usr/bin/javac javac /usr/lib/jvm/jdk1.8.0_171/bin/javac 300
	sudo update-alternatives –install /usr/bin/jar jar /usr/lib/jvm/jdk1.8.0_171/bin/jar 300 
	sudo update-alternatives –install /usr/bin/javah javah /usr/lib/jvm/jdk1.8.0_171/bin/javah 300 
	sudo update-alternatives –install /usr/bin/javap javap /usr/lib/jvm/jdk1.8.0_171/bin/javap 300


安装实例：
sudo update-alternatives --install /usr/bin/java java /usr/lib/jvm/jdk1.8.0_181/bin/java 300
sudo update-alternatives --install /usr/bin/javac javac /usr/lib/jvm/jdk1.8.0_181/bin/javac 300

改变这个指向呢，用到update-alternatives命令了。

sudo update-alternatives –config java



jdk config (myconfig)
sudo vim /etc/profile

sudo gedit /etc/profile | ~/.profile 
export JAVA_HOME=/home/cnopens/servers/jdk/jdk1.8.0_181 
export JRE_HOME=$JAVA_HOME/jre
export PATH=$PATH:$JAVA_HOME/bin:$JRE_HOME/bin
export CLASSPATH=.:$JAVA_HOME/lib:$JAVA_HOME/jre/lib:$CLASSPATH


## git config
------------------------------------
1. sudo apt-get install git

2. config:
git config --global user.name "cnopens"
git config --global user.email "cnopens@163.com"
git config --list

3. ssh-keygen -C 'cnopens@163.com' -t rsa

4. Testing ssh -T git@github.com

5. git base use:
---
git clone 项目地址  拉项目
git pull    拉代码
git push  提交到仓库
git init指令初始化一个git仓库
git add .添加文件
git commit -m "注释"提交至仓库。
git remote add origin https://git.oschina.net/你的用户名/项目名.
git push origin master即可完成推送
git checkout master   切换到master分支


## maven config 
------------------------------------
M2_HOME 
settings.xml


## Node config 
------------------------------------
在系统内安装nodejs

在系统内一次输入以下指令

sudo apt-get update  
# 下方的下载地址，请根据需要更换
sudo wget https://nodejs.org/dist/v10.15.3/node-v10.15.3.tar.gz
# 下方的解压文件，根据下载的文件名更换
sudo tar xvf node-v10.15.3.tar.gz
# 下方的文件夹名称根据解压之后的文件名更换
cd node-v10.15.3
sudo ./configure
sudo make
sudo make test
sudo make install  
sudo cp /usr/local/bin/node /usr/sbin/

ln -s  /home/root/node-v10.11.0-linux-x64/bin/node  /usr/local/bin/
ln -s /home/root/node-v10.11.0-linux-x64/bin/node  /usr/local/bin/
// 注意要写文件的绝对路径


## redis install
---
https://blog.csdn.net/hzlarm/article/details/99432240




## vmware15 workstation install && remove

1. remove method

vmware-installer -u vmware-workstation
find / -name '*vmware*' |grep -v "/opt" | xargs rm -rf
防止把opt下的虚拟机删掉，加上过滤
reboot


安装vmware 14
由于Ubuntu 1804的内核过高，会导致安装虚拟机会报错，但是先不用管它

./VMware-Workstation-Full-14.1.7.bundle --console --eulas-agreed --required -s vmware-workstation serialNumber ZC3WK-AFXEK-488JP-A7MQX-XL8YF 

# xxx为序列号
vmware-modconfig --console --install-all
# 这个时候按照完毕后，查看网卡是发现未生成vmware 的虚拟卡的，需要去下载vm-host文件，重新编译安装虚拟网卡

下载地址：https://github.com/mkubecek/vmware-host-modules

vmware vmnet模块无法加载，重新配置vmnet模块

wget https://github.com/mkubecek/vmware-host-modules/archive/workstation-14.1.7.tar.gz
# 改成自己的版本号即可
tar -xzf workstation-14.1.0.tar.gz
cd vmware-host-modules-workstation-14.1.0
make
make install
/etc/init.d/vmware status
# 查看模块是否加载
/etc/init./vmware start
# 启动模块

如果是虚拟机不是正常卸载，可能也会装不上，因为.so 文件没有删除，查看vmnet模块 make install ,是更改了哪些模块手动拷贝过去。
如果模块一直为加载，将make 好的两个.ko文件拷贝至/lib/modules/5.3.0-28-generic/misc/vmmon.ko
然后启动模块


Workstation 14 Pro for Linux
https://www.vmware.com/go/getworkstation-linux

vmware workstation14永久激活密钥分享

CG54H-D8D0H-H8DHY-C6X7X-N2KG6

ZC3WK-AFXEK-488JP-A7MQX-XL8YF

AC5XK-0ZD4H-088HP-9NQZV-ZG2R4

ZC5XK-A6E0M-080XQ-04ZZG-YF08D

ZY5H0-D3Y8K-M89EZ-AYPEG-MYUA8


https://download3.vmware.com/software/wkst/file/VMware-Workstation-Full-14.1.7-12989993.x86_64.bundle?HashKey=9b8673db22585a6439bc3f3d9d1abbb6&params=%7B%22custnumber%22%3A%22ZCp0cCVkaCVqcA%3D%3D%22%2C%22sourcefilesize%22%3A%22439.75+MB%22%2C%22dlgcode%22%3A%22WKST-1417-LX%22%2C%22languagecode%22%3A%22en%22%2C%22source%22%3A%22DOWNLOADS%22%2C%22downloadtype%22%3A%22manual%22%2C%22eula%22%3A%22Y%22%2C%22downloaduuid%22%3A%229f84922c-0158-453f-a461-011118ea5145%22%2C%22purchased%22%3A%22N%22%2C%22dlgtype%22%3A%22Product+Binaries%22%2C%22productversion%22%3A%2214.1.7%22%2C%22productfamily%22%3A%22VMware+Workstation+Pro%22%7D&AuthKey=1599627715_7046264218b9dc1cc83eb942824f1e38