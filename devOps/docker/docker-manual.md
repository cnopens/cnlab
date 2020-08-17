#docker-manual.md
===

## install & reconition

ubuntu install docker


sudo add-apt-repository \
"deb [arch=arm64] https://download.docker.com/linux/ubuntu \
$(lsb_release -cs) \
stable"


sudo apt-get update
sudo apt-get install docker-ce docker-ce-cli containerd.io


列出可用版本：
apt-cache madison docker-ce

$ sudo apt-get install docker-ce=<VERSION_STRING> docker-ce-cli=<VERSION_STRING> containerd.io

## common operation

删除 apt-key
sudo apt-key list

会出现这样：
pub   1024R/B455BEF0 2010-07-29
uid                  Launchpad clicompanion-nightlies

删除想删除的
sudo apt-key del B455BEF0


添加PPA源的命令为：
sudo add-apt-repository ppa:user/ppa-name

添加好更新一下： sudo apt-get update

删除命令格式则为：
sudo add-apt-repository -r ppa:user/ppa-name

或者

1,到 源的 目 录:cd  /etc/apt/sources.list.d/

2,可以看到关于源的文件,删除即可 


## docker run script

````
version=last
docker run --restart=always dce \
-p 9001:9001 \
-p 80:80 \
-v /paas/dce/logs:/log \
-e RUN_MODE=release \
-e TZ=Asia/Shanghai \
-d 192.168.31.136/dashuo_containers/dce:${version}
````



