#docker-unimit-param.md

## WWH

Why?
---
Docker在1.6版本之后才支持；之前的版本，Docker Container继承来自Docker Daemon的ulimit设置。

ulimit 可以设置当前进程以及其子进程的资源使用量，此处讨论我们启动的docker 容器的资源限制。

Note: 
通过 ulimit 改善系统性能.


Reference: 
ulimit usage
[1](https://www.ibm.com/developerworks/cn/linux/l-cn-ulimit/index.html)
docker run params
[2](https://docs.docker.com/engine/reference/commandline/run/#set-ulimits-in-container---ulimit)


What?
---
ulimit本是一个Linux内的命令。最初设计是用来限制进程对资源的使用情况的，因为早期的系统系统资源包括内存,CPU都是非常有限的，系统要保持公平，就要限制大家的使用，以达到一个相对公平的环境。

How to use?
---
# 使用格式
>>> ulimit [options] [limit]
>>> ulimit -n 1024 # 打开文件描述符的数量
>>> ulimit -n # 查看相应参数
1024

## Docker modify ulimit methods

1. docker run --ulimit
docker run --ulimit nofile=1024:1024 --rm debian sh -c "ulimit -n"

2. docker service default setting

>>> vim /usr/lib/systemd/system/docker.service
[Service]
LimitNOFILE=1048576
LimitNPROC=1048576
LimitCORE=infinity
>>> systemctl daemon-reload
>>> systemctl restart docker

3. daemon.json

>>> vim /etc/docker/daemon.json
{
        "default-ulimits": {
                "nofile": {
                        "Name": "nofile",
                        "Hard": 64000,
                        "Soft": 64000
                }
        }
}
>>> systemctl restart docker

Reference:

- ulimit 与 容器 (https://www.cnblogs.com/JenningsMao/p/9209689.html)
- 深入理解Docker ulimit (http://dockone.io/article/522)
- 系统技术非业余研究 (http://blog.yufeng.info/archives/tag/ulimit)
- Docker ulimit配置 (https://sukbeta.github.io/docker-ulimit-configure/)
- ulimit的一些理解和在docker中的经验 (https://blog.51cto.com/buranle/1677642)
