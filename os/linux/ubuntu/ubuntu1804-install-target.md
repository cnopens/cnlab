# ubuntu1804-install-target.md

## user privillage rise  -oK

Node :diff from centoS 
usermod -aG sudo cnopens 


要确保用户具有sudo权限，请运行whoami命令：
$ sudo whoami
系统将提示你输入密码，如果用户有sudo访问权限，该命令将打印“root”：
root

## youdao


## sougou


@todo 

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

然后输入这几条指令

	sudo update-alternatives –install /usr/bin/java java /usr/lib/jvm/jdk1.8.0_171/bin/java 300 
	sudo update-alternatives –install /usr/bin/javac javac /usr/lib/jvm/jdk1.8.0_171/bin/javac 300
	sudo update-alternatives –install /usr/bin/jar jar /usr/lib/jvm/jdk1.8.0_171/bin/jar 300 
	sudo update-alternatives –install /usr/bin/javah javah /usr/lib/jvm/jdk1.8.0_171/bin/javah 300 
	sudo update-alternatives –install /usr/bin/javap javap /usr/lib/jvm/jdk1.8.0_171/bin/javap 300

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


## redis install
---
https://blog.csdn.net/hzlarm/article/details/99432240




