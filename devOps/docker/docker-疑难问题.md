#docker-疑难问题.md
---
## 1.关于Docker时的权限问题解决dial unix /var/run/docker.sock: connect: `permission denied

```
	resove1: 在docker命令前加sudo

	resove2: 只需要操作一次
	    1. 将当前用户加入到docker组中
	    	sudo gpasswd -a 用户名 用户组 ---> sudo gpasswd -a go docker
	    		表示当前用户: $USER
	    2. 将当前用户切换到docker组中
	    	newgrp - docker

	resove3: 只要docker服务重启, 就需要重新设置一次

	    cd /var/run
	    sudo chmod 666 docker.sock

```
## Docker 拉取 报错【error pulling image configuration】及镜像下载慢问题

*Note: tee usage 

```

1.出现这个问题原因为网络问题，无法连接到docker hub。 但国内有 daocloud加速，docker指定该源即可.

echo "DOCKER_OPTS=\"\$DOCKER_OPTS --registry-mirror=http://f2d6cb40.m.daocloud.io\"" | sudo tee -a /etc/default/docker
DOCKER_OPTS="$DOCKER_OPTS --registry-mirror=http://f2d6cb40.m.daocloud.io"

2.镜像下载慢问题:
vi /etc/docker/daemon.json

{
  "registry-mirrors": ["https://xxxselfid.mirror.aliyuncs.com"]
}

3.重新启动Docker
systemctl restart docker

```


## docker 命令参数之ulimit使用

docker run -ti --name test1 --ulimit nproc=65535 centos:7
docker run -ti --name test2 --ulimit nproc=1024 centos:7

--ulimit nproc=65535 在启动前来指定大小
ulimt -u #进入容器查看


