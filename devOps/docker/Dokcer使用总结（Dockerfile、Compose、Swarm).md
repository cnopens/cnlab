#Dokcer使用总结（Dockerfile、Compose、Swarm).md
Dokcer基础

查看Linux版本

uname -r

查看Linux详尽信息

cat /etc/*elease


CentOS Linux release 7.6.1810 (Core) 
NAME="CentOS Linux"
VERSION="7 (Core)"
ID="centos"
ID_LIKE="rhel fedora"
VERSION_ID="7"
PRETTY_NAME="CentOS Linux 7 (Core)"
ANSI_COLOR="0;31"
CPE_NAME="cpe:/o:centos:centos:7"
HOME_URL="https://www.centos.org/"
BUG_REPORT_URL="https://bugs.centos.org/"

CENTOS_MANTISBT_PROJECT="CentOS-7"
CENTOS_MANTISBT_PROJECT_VERSION="7"
REDHAT_SUPPORT_PRODUCT="centos"
REDHAT_SUPPORT_PRODUCT_VERSION="7"

CentOS Linux release 7.6.1810 (Core) 

容器的五大隔离

    pid：进程隔离
    net：网络隔离 （独有的ip地址，网关，子网掩码）
    ipc：进程间交互隔离
    mnt：文件系统隔离
    uts：主机和域名隔离 （hostname，domainname）container 有自己的机器名

centos上安装docker

官方地址：https://docs.docker.com/install/linux/docker-ce/centos/

    卸载旧版本


    sudo yum remove docker \
                      docker-client \
                      docker-client-latest \
                      docker-common \
                      docker-latest \
                      docker-latest-logrotate \
                      docker-logrotate \
                      docker-engine


    安装包环境

    sudo yum install -y yum-utils \
      device-mapper-persistent-data \
      lvm2

    设置仓储地址


    # 阿里云
    sudo yum-config-manager --add-repo https://mirrors.aliyun.com/docker-ce/linux/centos/docker-ce.repo
    # 官方
    sudo yum-config-manager \
        --add-repo \
        https://download.docker.com/linux/centos/docker-ce.repo


    安装Docker-CE

    sudo yum install docker-ce docker-ce-cli containerd.io

    启动Docker，运行开机自启

    systemctl start docker
    systemctl enable docker

Docker安装位置

    查找Docker可执行程序地址 /usr/bin/docker 

    find / -name docker

    View Code

    查找Docker服务端程序 /usr/bin/dockerd 

    find / -name dockerd

    View Code

    lib + data： /var/lib/docker

    config： /etc/docker
    查找docker.service服务程序 /usr/lib/systemd/system/docker.service 

    find / -name docker.service



    [root@localhost ~]# cat /usr/lib/systemd/system/docker.service
    [Unit]
    Description=Docker Application Container Engine
    Documentation=https://docs.docker.com
    BindsTo=containerd.service
    After=network-online.target firewalld.service containerd.service
    Wants=network-online.target
    Requires=docker.socket

    [Service]
    Type=notify
    # the default is not to use systemd for cgroups because the delegate issues still
    # exists and systemd currently does not support the cgroup feature set required
    # for containers run by docker
    ExecStart=/usr/bin/dockerd -H fd:// --containerd=/run/containerd/containerd.sock
    ExecReload=/bin/kill -s HUP $MAINPID
    TimeoutSec=0
    RestartSec=2
    Restart=always

    # Note that StartLimit* options were moved from "Service" to "Unit" in systemd 229.
    # Both the old, and new location are accepted by systemd 229 and up, so using the old location
    # to make them work for either version of systemd.
    StartLimitBurst=3

    # Note that StartLimitInterval was renamed to StartLimitIntervalSec in systemd 230.
    # Both the old, and new name are accepted by systemd 230 and up, so using the old name to make
    # this option work for either version of systemd.
    StartLimitInterval=60s

    # Having non-zero Limit*s causes performance problems due to accounting overhead
    # in the kernel. We recommend using cgroups to do container-local accounting.
    LimitNOFILE=infinity
    LimitNPROC=infinity
    LimitCORE=infinity

    # Comment TasksMax if your systemd version does not supports it.
    # Only systemd 226 and above support this option.
    TasksMax=infinity

    # set delegate yes so that systemd does not reset the cgroups of docker containers
    Delegate=yes

    # kill only the docker process, not all processes in the cgroup
    KillMode=process

    [Install]
    WantedBy=multi-user.target



解读dockerd配置文件

dockerd：https://docs.docker.com/engine/reference/commandline/dockerd/
硬盘挂载

    使用 fdisk -l 命令查看主机上的硬盘

    fdisk -l

    View Code
    使用mkfs.ext4命令把硬盘格式化

    # mkfs.ext4    磁盘名称

    mkfs.ext4   /dev/vdb

    使用mount命令挂载磁盘

    mount /dev/vdb /boot

    输入指令： df -h 查看当前磁盘的情况

    df -h

    View Code
    用 blkid  获取磁盘的uuid和属性

    [root@localhost ~]# blkid
    /dev/vda1: UUID="105fa8ff-bacd-491f-a6d0-f99865afc3d6" TYPE="ext4" 
    /dev/vdb: UUID="97a17b64-d025-478c-8981-105214e99ff4" TYPE="ext4" 

     

    设置开机自动mount

    vim /etc/fstab

    UUID=97a17b64-d025-478c-8981-105214e99ff4  /data  ext4  defaults  1  1

修改docker存储位置

    创建或修改docker配置文件


    # 创建或修改docker配置文件
    vim /etc/docker/daemon.json

    {
     "data-root": "/data/docker"
    }


    创建docker数据存储文件夹

    # 创建docker数据存储文件夹
    mkdir /data
    mkdir /data/docker

    停止Docker

    # 停止Docker
    service docker stop

    拷贝存储文件

    # 拷贝存储文件
    cp -r /var/lib/docker/* /data/docker/

    删除源文件

    # 删除源文件(不建议先删除，后面没问题了再删除)
    # rm -rf /var/lib/docker/

    验证docker数据存储位置是否改变

    # 验证docker数据存储位置是否改变
    docker info

    注意：最好在docker刚安装完就执行切换数据目录，不然等容器运行起来后里面的一些volume会还是使用的原来的

镜像加速器
复制代码

sudo mkdir -p /etc/docker
vim /etc/docker/daemon.json

{
  "registry-mirrors": ["https://uwxsp1y1.mirror.aliyuncs.com"],
  "data-root": "/data/docker"
}

sudo systemctl daemon-reload
sudo systemctl restart docker

复制代码
查看系统日志
复制代码

# 修改配置信息
vim /etc/docker/daemon.json

{
  "registry-mirrors": ["https://uwxsp1y1.mirror.aliyuncs.com"],
  "data-root": "/data/docker",
  "debug":true
}


# journalctl 统一查看service所有的日志。
journalctl -u docker.service -f 

复制代码
远程连接docker deamon

    修改docker.service启动信息

    # 修改docker.service启动信息
    vim /usr/lib/systemd/system/docker.service

    # ExecStart=/usr/bin/dockerd -H fd:// --containerd=/run/containerd/containerd.sock
    ExecStart=/usr/bin/dockerd --containerd=/run/containerd/containerd.sock

    修改daemon.json


    #修改daemon.json
    vim /etc/docker/daemon.json

    {
      "registry-mirrors": ["https://uwxsp1y1.mirror.aliyuncs.com"],
      "data-root": "/data/docker",
      "debug":true,
      "hosts": ["192.168.103.240:6381","unix:///var/run/docker.sock"]
    }



    重载、重启

    # 重载、重启
    sudo systemctl daemon-reload
    service docker restart

    查看端口


    # 查看端口
    netstat -tlnp

    [root@localhost docker]# netstat -tlnp
    Active Internet connections (only servers)
    Proto Recv-Q Send-Q Local Address           Foreign Address         State       PID/Program name    
    tcp        0      0 192.168.103.240:6381    0.0.0.0:*               LISTEN      27825/dockerd       
    tcp        0      0 0.0.0.0:111             0.0.0.0:*               LISTEN      1/systemd           
    tcp        0      0 192.168.122.1:53        0.0.0.0:*               LISTEN      3743/dnsmasq        
    tcp        0      0 0.0.0.0:22              0.0.0.0:*               LISTEN      3122/sshd           
    tcp        0      0 127.0.0.1:631           0.0.0.0:*               LISTEN      3109/cupsd          
    tcp        0      0 127.0.0.1:25            0.0.0.0:*               LISTEN      3479/master         
    tcp        0      0 127.0.0.1:6010          0.0.0.0:*               LISTEN      14503/sshd: root@pt 
    tcp6       0      0 :::111                  :::*                    LISTEN      1/systemd           
    tcp6       0      0 :::22                   :::*                    LISTEN      3122/sshd           
    tcp6       0      0 ::1:631                 :::*                    LISTEN      3109/cupsd          
    tcp6       0      0 ::1:25                  :::*                    LISTEN      3479/master         
    tcp6       0      0 ::1:6010                :::*                    LISTEN      14503/sshd: root@pt 



    远程连接测试

    # 远程连接测试
    docker -H 192.168.103.240:6381 ps

容器基础
docker container 中常用操控命令

docker run --help

View Code
docker run,docker exec

run可以让容器从镜像中实例化出来，实例化过程中可以塞入很多参数

Usage: docker run [OPTIONS] IMAGE [COMMAND] [ARG...]

docker run -d --name some-redis redis 外界无法访问，因为是网络隔离，默认bridge模式。

    -a stdin: 指定标准输入输出内容类型，可选 STDIN/STDOUT/STDERR 三项；

    -d: 后台运行容器，并返回容器ID；

    -i: 以交互模式运行容器，通常与 -t 同时使用；

    -P: 随机端口映射，容器内部端口随机映射到主机的高端口

    -p: 指定端口映射，格式为：主机(宿主)端口:容器端口

    -t: 为容器重新分配一个伪输入终端，通常与 -i 同时使用；

    --name="nginx-lb": 为容器指定一个名称；

    --dns 8.8.8.8: 指定容器使用的DNS服务器，默认和宿主一致；

    --dns-search example.com: 指定容器DNS搜索域名，默认和宿主一致；

    -h "mars": 指定容器的hostname；

    -e username="ritchie": 设置环境变量；

    # 设置东八区
    docker run -e TZ=Asia/Shanghai -d --name some-redis redis

    --env-file=[]: 从指定文件读入环境变量；

    --cpuset="0-2" or --cpuset="0,1,2": 绑定容器到指定CPU运行；

    -m :设置容器使用内存最大值；

    --net="bridge": 指定容器的网络连接类型，支持 bridge/host/none/container:<name|id> 四种类型；

    --link=[]: 添加链接到另一个容器；

    --expose=[]: 开放一个端口或一组端口；

    --volume , -v: 绑定一个卷

    docker run -p 16379:6379 -d --name some-redis redis

    --add-host: 添加自定义ip

    # 场景：consul做健康检查的时候，需要宿主机的ip地址
    docker run --add-host machineip:192.168.103.240 -d --name some-redis redis

    docker exec -it some-redis bash
    tail /etc/hosts

docker start,docker stop, docker kill

    docker start :启动一个或多个已经被停止的容器

    docker stop :停止一个运行中的容器

    docker restart :重启容器
    docker kill :杀掉一个运行中的容器。

batch delete 容器

docker rm -f 
docker rm -f `docker ps -a -q`
docker containers prune

# 极其强大的删除清理方式，慎重使用
# docker system prune

docker container 状态监控命令
查看容器日志

docker logs

复制代码

[root@localhost ~]# docker logs some-redis
1:C 09 Jul 2019 03:07:03.406 # oO0OoO0OoO0Oo Redis is starting oO0OoO0OoO0Oo
1:C 09 Jul 2019 03:07:03.406 # Redis version=5.0.5, bits=64, commit=00000000, modified=0, pid=1, just started
1:C 09 Jul 2019 03:07:03.406 # Warning: no config file specified, using the default config. In order to specify a config file use redis-server /path/to/redis.conf
1:M 09 Jul 2019 03:07:03.406 * Running mode=standalone, port=6379.
1:M 09 Jul 2019 03:07:03.406 # WARNING: The TCP backlog setting of 511 cannot be enforced because /proc/sys/net/core/somaxconn is set to the lower value of 128.
1:M 09 Jul 2019 03:07:03.406 # Server initialized
1:M 09 Jul 2019 03:07:03.406 # WARNING overcommit_memory is set to 0! Background save may fail under low memory condition. To fix this issue add 'vm.overcommit_memory = 1' to /etc/sysctl.conf and then reboot or run the command 'sysctl vm.overcommit_memory=1' for this to take effect.
1:M 09 Jul 2019 03:07:03.406 # WARNING you have Transparent Huge Pages (THP) support enabled in your kernel. This will create latency and memory usage issues with Redis. To fix this issue run the command 'echo never > /sys/kernel/mm/transparent_hugepage/enabled' as root, and add it to your /etc/rc.local in order to retain the setting after a reboot. Redis must be restarted after THP is disabled.
1:M 09 Jul 2019 03:07:03.406 * Ready to accept connections

复制代码
容器性能指标

docker stats

[root@localhost ~]# docker stats

CONTAINER ID        NAME                CPU %               MEM USAGE / LIMIT     MEM %               NET I/O             BLOCK I/O           PIDS
aaa8bec01038        some-redis          0.04%               8.375MiB / 1.795GiB   0.46%               656B / 0B           139kB / 0B          4

容器 -> 宿主机端口

查询port映射关系

知道容器的端口，不知道宿主机的端口。。。
不知道容器的端口，知道宿主机的端口。。。

docker port [container]

View Code
查看容器内运行的进程

docker top [container]

View Code
容器的详细信息

docker inspect [OPTIONS] NAME|ID [NAME|ID...]

View Code
容器的导入导出

    docker export :将文件系统作为一个tar归档文件导出到STDOUT。


    docker export [OPTIONS] CONTAINER

    # OPTIONS说明：
    # -o :将输入内容写到文件。

    # PS：
    # docker export -o /app2/1.tar.gz some-redis



    docker import : 从归档文件中创建镜像。


    docker import [OPTIONS] file|URL|- [REPOSITORY[:TAG]]

    # OPTIONS说明：
    # -c :应用docker 指令创建镜像；
    # -m :提交时的说明文字；

    # PS：
    # 还原镜像
    # docker import /app2/1.tar.gz newredis
    # 创建容器并运行redis-server启动命令
    # docker run -d --name new-some-redis-2 newredis redis-server



docker images命令详解

docker image

View Code
镜像的获取，删除，查看

    docker pull : 从镜像仓库中拉取或者更新指定镜像

    docker pull [OPTIONS] NAME[:TAG|@DIGEST]

    # OPTIONS说明：
    # -a :拉取所有 tagged 镜像
    # --disable-content-trust :忽略镜像的校验,默认开启

    docker rmi : 删除本地一个或多少镜像。

    docker rmi [OPTIONS] IMAGE [IMAGE...]

    # OPTIONS说明：
    # -f :强制删除；
    # --no-prune :不移除该镜像的过程镜像，默认移除；

    docker inspect : 获取容器/镜像的元数据。


    docker inspect [OPTIONS] NAME|ID [NAME|ID...]

    # OPTIONS说明：
    # -f :指定返回值的模板文件。
    # -s :显示总的文件大小。
    # --type :为指定类型返回JSON。


    docker images : 列出本地镜像。


    docker images [OPTIONS] [REPOSITORY[:TAG]]

    # OPTIONS说明：
    # -a :列出本地所有的镜像（含中间映像层，默认情况下，过滤掉中间映像层）；
    # --digests :显示镜像的摘要信息；
    # -f :显示满足条件的镜像；
    # --format :指定返回值的模板文件；
    # --no-trunc :显示完整的镜像信息；
    # -q :只显示镜像ID。



镜像的导入导出，迁移

docker export/import 对容器进行打包
docker save / load 对镜像进行打包

    docker save : 将指定镜像保存成 tar 归档文件。


    docker save [OPTIONS] IMAGE [IMAGE...]

    # OPTIONS 说明：
    # -o :输出到的文件。

    # PS:
    # docker save -o /app2/1.tar.gz redis


    docker load : 导入使用 docker save 命令导出的镜像。


    docker load [OPTIONS]

    # OPTIONS 说明：
    # -i :指定导出的文件。
    # -q :精简输出信息。

    # PS:
    # docker load -i /app2/1.tar.gz



docker tag

打标签的目的，方便我上传到自己的私有仓库

    docker tag : 标记本地镜像，将其归入某一仓库。


    docker tag [OPTIONS] IMAGE[:TAG] [REGISTRYHOST/][USERNAME/]NAME[:TAG]

    # PS:
    # docker tag redis:latest 13057686866/redis_1
    # 登录
    # docker login
    # 推送到远程私有仓库
    # docker push 13057686866/redis_1



手工构建

    docker build 命令用于使用 Dockerfile 创建镜像。


    docker build [OPTIONS] PATH | URL | -

    # OPTIONS说明：
    # --build-arg=[] :设置镜像创建时的变量；
    # --cpu-shares :设置 cpu 使用权重；
    # --cpu-period :限制 CPU CFS周期；
    # --cpu-quota :限制 CPU CFS配额；
    # --cpuset-cpus :指定使用的CPU id；
    # --cpuset-mems :指定使用的内存 id；
    # --disable-content-trust :忽略校验，默认开启；
    # -f :指定要使用的Dockerfile路径；
    # --force-rm :设置镜像过程中删除中间容器；
    # --isolation :使用容器隔离技术；
    # --label=[] :设置镜像使用的元数据；
    # -m :设置内存最大值；
    # --memory-swap :设置Swap的最大值为内存+swap，"-1"表示不限swap；
    # --no-cache :创建镜像的过程不使用缓存；
    # --pull :尝试去更新镜像的新版本；
    # --quiet, -q :安静模式，成功后只输出镜像 ID；
    # --rm :设置镜像成功后删除中间容器；
    # --shm-size :设置/dev/shm的大小，默认值是64M；
    # --ulimit :Ulimit配置。
    # --tag, -t: 镜像的名字及标签，通常 name:tag 或者 name 格式；可以在一次构建中为一个镜像设置多个标签。
    # --network: 默认 default。在构建期间设置RUN指令的网络模式



dockerfile
docker build自己动手构建镜像

官方文档：https://docs.docker.com/engine/reference/builder/
dockerfile参数

    FROM

    ENV

    RUN

    CMD

    LABEL

    EXPOSE

    ADD

    不仅可以copy文件，还可以下载远程文件。。。
    如果是本地的zip包，还能自动解压。

    COPY

    ENTRYPOINT

    VOLUME

    USER

    WORKDIR

    ONBUILD

    STOPSIGNAL

    HEALTHCHECK

    新建项目 WebApplication1 空项目即可
    新建 Dockerfile 配置文件


    # 1-有了基础镜像
    FROM mcr.microsoft.com/dotnet/core/sdk:2.2

    # 2-把我的文件拷贝到这个操作系统中的/app文件夹中
    COPY . /app

    # 工作目录
    WORKDIR /app

    # 3-publish
    RUN cd /app && dotnet publish "WebApplication1.csproj" -c Release -o /work

    # 4-告诉外界我的app暴露的是80端口
    EXPOSE 80

    # else
    ENV TZ Asia/Shanghai
    ENV ASPNETCORE_ENVIRONMENT Production

    # 作者信息
    LABEL version="1.0"
    LABEL author="wyt"

    # 执行角色
    USER root

    # 设置工作目录
    WORKDIR /work

    # 4-启动
    CMD ["dotnet","WebApplication1.dll"]



    将 WebApplication1 整个目录拷贝到远程服务器下

    构建镜像

    cd /app/WebApplication1
    docker build -t 13057686866/webapp:v1 .

    运行容器

    docker run -d -p 18000:80 --name webapp3 13057686866/webapp:v1

    运行成功

    curl http://192.168.103.240:18000/
    Hello World!

Dockerfile优化策略
使用 .dockerignore 忽略文件

官方地址：https://docs.docker.com/engine/reference/builder/#dockerignore-file
复制代码

**/.dockerignore
**/.env
**/.git
**/.gitignore
**/.vs
**/.vscode
**/*.*proj.user
**/azds.yaml
**/charts
**/bin
**/obj
**/Dockerfile
**/Dockerfile.develop
**/docker-compose.yml
**/docker-compose.*.yml
**/*.dbmdl
**/*.jfm
**/secrets.dev.yaml
**/values.dev.yaml
**/.toolstarget

复制代码

我们完全可以使用VS来创建Dockerfile，会自动生成 .dockerignore 

使用多阶段构建

多阶段构建：一个From一个阶段

dockerfile中只有最后一个From是生效的，其他的from只是给最后一个from打辅助。。。

当最后一个from生成完毕的时候，其他的from都会自动销毁。。。

 FROM build AS publish  给当前的镜像取一个别名。。
复制代码

FROM mcr.microsoft.com/dotnet/core/aspnet:2.2 AS base
WORKDIR /app
EXPOSE 80

FROM mcr.microsoft.com/dotnet/core/sdk:2.2 AS build
WORKDIR /src
COPY ["WebApplication1.csproj", ""]
RUN dotnet restore "WebApplication1.csproj"
COPY . .
WORKDIR "/src/"
RUN dotnet build "WebApplication1.csproj" -c Release -o /app

FROM build AS publish
RUN dotnet publish "WebApplication1.csproj" -c Release -o /app

FROM base AS final
WORKDIR /app
COPY --from=publish /app .
ENTRYPOINT ["dotnet", "WebApplication1.dll"] 

复制代码
及时移除不必须的包

# 3-publish
RUN cd /app && dotnet publish "WebApplication1.csproj" -c Release -o /work && rm -rf /app

最小化层的个数   

    可参考官方dockerfile
    ADD 和 COPY,ADD 会增加 layer的个数。
    RUN尽可能合并

搭建自己的私有registry仓库

官网介绍：https://docs.docker.com/registry/deploying/
搭建自己内网仓库，可以加速

    拉取本地仓库镜像

    docker pull registry:2

    运行本地仓库容器

    # 运行本地仓库容器
    docker run -d -p 5000:5000 --restart=always --name registry registry:2

    拉取alpine镜像

    # 拉取alpine镜像
    docker pull alpine

    重命名标签，指向本地仓库

    # 重命名标签，指向本地仓库
    docker tag alpine 192.168.103.240:5000/alpine:s1

    远程推送到本地仓库

    # 远程推送到本地仓库
    docker push 192.168.103.240:5000/alpine:s1

    故障：http: server gave HTTP response to HTTPS client（https client 不接受  http response）
    解决办法： https://docs.docker.com/registry/insecure/


    # 编辑该daemon.json文件，其默认位置 /etc/docker/daemon.json在Linux或 C:\ProgramData\docker\config\daemon.jsonWindows Server上。如果您使用Docker Desktop for Mac或Docker Desktop for Windows，请单击Docker图标，选择 Preferences，然后选择+ Daemon。
    # 如果该daemon.json文件不存在，请创建它。假设文件中没有其他设置，则应具有以下内容：

    {
      "insecure-registries" : ["192.168.103.240:5000"]
    }

    # 将不安全注册表的地址替换为示例中的地址。

    # 启用了不安全的注册表后，Docker将执行以下步骤：
    # 1-首先，尝试使用HTTPS。
    # 2-如果HTTPS可用但证书无效，请忽略有关证书的错误。
    # 3-如果HTTPS不可用，请回退到HTTP。

    # 重新启动Docker以使更改生效。
    service docker restart


    验证镜像是否推送成功

    docker pull 192.168.103.240:5000/alpine:s1

    拉取开源registry UI镜像
    官方地址：https://hub.docker.com/r/joxit/docker-registry-ui

    # 拉取registry-ui镜像
    docker pull joxit/docker-registry-ui

    设置允许repositry跨域


    # 设置允许跨域https://docs.docker.com/registry/configuration/
    # 复制文件到本地
    docker cp registry:/etc/docker/registry/config.yml /app
    # 修改配置文件，添加跨域
    vim /etc/docker/registry/config.yml

    version: 0.1
    log:
      fields:
        service: registry
    storage:
      cache:
        blobdescriptor: inmemory
      filesystem:
        rootdirectory: /var/lib/registry
    http:
      addr: :5000
      headers:
        X-Content-Type-Options: [nosniff]
        Access-Control-Allow-Origin: ['*']
        Access-Control-Allow-Methods: ['*']
        Access-Control-Max-Age: [1728000]
    health:
      storagedriver:
        enabled: true
        interval: 10s
        threshold: 3
        
    # 重新启动registry容器
    docker rm registry -f
    docker run -d -p 5000:5000 --restart=always --name registry -v /app/config.yml:/etc/docker/registry/config.yml registry:2


    运行registry-ui容器

    # 运行容器
    docker rm -f registry-ui
    docker run -d -p 8002:80 --name registry-ui joxit/docker-registry-ui

    访问可视化容器

使用阿里云镜像存储服务

官方地址：https://cr.console.aliyun.com/cn-hangzhou/instances/repositories

接入操作：

    登录阿里云Docker Registry

    sudo docker login --username=tb5228628_2012 registry.cn-hangzhou.aliyuncs.com

    用于登录的用户名为阿里云账号全名，密码为开通服务时设置的密码。

    从Registry中拉取镜像

    sudo docker pull registry.cn-hangzhou.aliyuncs.com/wyt_registry/wyt_registry:[镜像版本号]

    将镜像推送到Registry

    sudo docker tag [ImageId] registry.cn-hangzhou.aliyuncs.com/wyt_registry/wyt_registry:[镜像版本号]
    sudo docker push registry.cn-hangzhou.aliyuncs.com/wyt_registry/wyt_registry:[镜像版本号]

volume数据挂载

三种方式可以让 数据 脱离到 容器之外，减少容器层的size，也提升了性能（避免容器的读写层）。
volume 管理

# 创建数据卷
docker volume create redisdata
# 使用数据卷
docker run -d -v redisdata:/data --name some-redis redis

优点：

    不考虑宿主机文件结构，所以更加方便迁移，backup。
    可以使用 docker cli 命令统一管理
    volumes支持多平台，不用考虑多平台下的文件夹路径问题。
    使用volumn plugin 可以方便和 aws, 等云平台远程存储。

bind 管理 （文件，文件夹）

将宿主机文件夹初始化送入容器中，后续进行双向绑定。
tmpfs 容器内目录挂载到宿主机内存中

# 不隐藏容器内/tmp内文件
docker run --rm -it webapp bash
# 隐藏容器内/tmp内文件
docker run --rm --tmpfs /tmp -it webapp bash

network网络
单机网络

默认情况下是 bridge，overlay，host， macvlan，none

docker host 的bridge 的 docker0 默认网桥
默认的 bridge 的大概原理

当docker启动的时候，会生成一个默认的docker0网桥。。。

当启动容器的时候，docker会生成一对 veth设备。。。。这个设备的一端连接到host的docker0网桥，一端连接到container中，重命名为eth0

veth一端收到了数据，会自动传给另一端。。。
View Code
默认的bridge缺陷

无服务发现功能，同一个子网，无法通过 “服务名” 互通，只能通过 ip 地址。。。
自定义bridge网络

自带服务发现机制
复制代码

# 创建桥接网络
docker network create my-net 
# 创建容器
docker run -it --network my-net --name some-redis alpine ash
docker run -it --network my-net --name some-redis2 alpine ash
# 在some-redis中ping容器some-redis2
ping some-redis2

复制代码
容器网络发布

如果让宿主机之外的程序能能够访问host上的bridge内的container:-p 发布端口
复制代码

# 运行容器进行端口转发
docker run -it --network my-net -p 80:80 --name some-redis-1 alpine ash
# 查看网络转发详情
iptables -t nat -L -n

Chain DOCKER (2 references)
target     prot opt source               destination         
RETURN     all  --  0.0.0.0/0            0.0.0.0/0           
RETURN     all  --  0.0.0.0/0            0.0.0.0/0           
DNAT       tcp  --  0.0.0.0/0            0.0.0.0/0            tcp dpt:80 to:172.18.0.4:80

复制代码
多机网络
overlay网络

可实现多机访问

    使用docker swarm init 实现docker 集群网络

    # 192.168.103.240
    docker swarm init
    # 192.168.103.226
    docker swarm join --token SWMTKN-1-0g4cs8fcatshczn5koupqx7lulak20fbvu99uzjb5asaddblny-bio99e9kktn023k527y3tjgyv 192.168.103.240:2377

    实现自定义的 可独立添加容器的 overlay网络

    docker network create --driver=overlay --attachable test-net

    TCP 2377 集群 manage 节点交流的
    TCP 的 7946 和 UDP 的 7946 nodes 之间交流的
    UDP 4789 是用于overlay network 流量传输的。

演示

    192.168.103.226 redis启动

    docker run --network test-net --name some-redis -d redis

    192.168.103.240 python

    mkdir /app
    vim /app/app.py
    vim /app/Dockerfile
    vim /app/requirements.txt

    app.pv
    View Code

    Dockerfile
    View Code

    requirements.txt
    View Code

    # 构建镜像
    docker build -t pyweb:v1 .
    # 运行容器
    docker run -d --network test-net -p 80:80 -v /app:/app --name pyapp pyweb:v1

    访问结果

host 模式 

这种模式不和宿主机进行网络隔离，直接使用宿主机网络

最简单最粗暴的方式

overlay虽然复杂，但是强大， 不好控制。
docker-compose

什么是docker-compose？应用程序栈一键部署，（独立程序一键部署），docker-compose 可以管理你的整个应用程序栈的生命周期。
下载

官方地址：https://docs.docker.com/compose/install/
复制代码

# 下载Docker Compose的当前稳定版本
sudo curl -L "https://github.com/docker/compose/releases/download/1.24.0/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose
# https://github.com/docker/compose/releases/download/1.24.1/docker-compose-Linux-x86_64
# 建议迅雷下载后进行重命名，这样速度快
# 对二进制文件应用可执行权限
sudo chmod +x /usr/local/bin/docker-compose
# 测试安装
docker-compose --version

复制代码
简单示例

    新建项目 WebApplication1 空网站项目添加NLog、Redis包支持

    Install-Package NLog.Targets.ElasticSearch
    Install-Package StackExchange.Redis

    修改 Program.cs 使用80端口

    public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
        WebHost.CreateDefaultBuilder(args)
            .UseUrls("http://*:80")
            .UseStartup<Startup>();

    修改 Startup.cs 添加日志和redis


    public Logger logger = LogManager.GetCurrentClassLogger();
    public ConnectionMultiplexer redis = ConnectionMultiplexer.Connect("redis");

    // This method gets called by the runtime. Use this method to configure the HTTP request pipeline.
    public void Configure(IApplicationBuilder app, IHostingEnvironment env)
    {
        if (env.IsDevelopment())
        {
            app.UseDeveloperExceptionPage();
        }

        app.Run(async (context) =>
        {
            var count = await redis.GetDatabase(0).StringIncrementAsync("counter");
            var info= $"you have been seen {count} times !";
            logger.Info(info);

            await context.Response.WriteAsync(info);
        });
    }


    添加 nlog.config 配置文件


    <?xml version="1.0" encoding="utf-8" ?>
    <nlog xmlns="http://www.nlog-project.org/schemas/NLog.xsd"
          xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
          autoReload="true"
          internalLogLevel="Warn">

        <extensions>
            <add assembly="NLog.Targets.ElasticSearch"/>
        </extensions>

        <targets>
            <target name="ElasticSearch" xsi:type="BufferingWrapper" flushTimeout="5000" >
                <target xsi:type="ElasticSearch" uri="http://elasticsearch:9200" documentType="web.app"/>
            </target>
        </targets>

        <rules>
            <logger name="*" minlevel="Trace" writeTo="ElasticSearch" />
        </rules>
    </nlog>



    添加 Dockerfile 文件


    FROM mcr.microsoft.com/dotnet/core/aspnet:2.2-stretch-slim AS base

    WORKDIR /data
    COPY . .

    EXPOSE 80

    ENTRYPOINT ["dotnet", "WebApplication1.dll"]



    添加 docker-compose.yml 文件


    version: '3.0'

    services:

      webapp: 
        build: 
          context: .
          dockerfile: Dockerfile
        ports: 
          - 80:80
        depends_on: 
          - redis
        networks: 
          - netapp

      redis: 
        image: redis
        networks: 
          - netapp

      elasticsearch: 
        image: elasticsearch:5.6.14
        networks: 
          - netapp

      kibana: 
        image: kibana:5.6.14
        ports: 
          - 5601:5601
        networks: 
          - netapp

    networks: 
      netapp:



    发布项目文件，并拷贝到远程服务器/app文件夹内

    运行 docker-compose 

    cd /app
    docker-compose up --build

    查看效果
    访问网站http://192.168.103.240/

    访问Kibana查看日志http://192.168.103.240:5601

docker-compose 常见命令

     操控命令


    docker-compose ps
    docker-compose images
    docker-compose kill webapp
    docker-compose build
    docker-compose run      -> docker exec
    docker-compose scale
    docker-compose up       -> docker run
    docker-compose down



    状态命令

    docker-compose logs
    docker-compose ps
    docker-compose top
    docker-compose port 
    docker-compose config

compose命令讲解

官方地址：https://docs.docker.com/compose/compose-file/
yml常用命令分析
复制代码

version      3.7 
services
config    （swarm）
secret    （swarm）
volume     
networks  

复制代码
appstack 补充

修改 WebApplication1 项目中的 docker-compose.yml 
View Code

部分docker-compose脚本：https://download.csdn.net/download/qq_25153485/11324352
docker-compose 一些使用原则
使用多文件部署

    生产环境代码直接放在容器中，test环境实现代码挂载

    test:   docker-compose -f  docker-compose.yml  -f test.yml   up 
    prd:   docker-compose -f  docker-compose.yml  -f prd.yml   up 

    生产环境中绑定程序默认端口，测试机防冲突绑定其他端口。
    生产环境配置 restart: always , 可以容器就可以挂掉之后重启。
    添加日志聚合,对接es

按需编译，按需构建

# 只构建service名称为webapp的镜像，也会构建其依赖
docker-compose build webapp
# 只构建service名称为webapp的镜像，不构建其依赖
docker-compose up --no-deps --build -d webapp

变量插值

    设置宿主机环境变量


    # 设置环境变量
    export ASPNETCORE_ENVIRONMENT=Production
    # 获取环境变量
    echo $ASPNETCORE_ENVIRONMENT
    # hostip 网卡ip 埋进去，方便获取
    # image的版本号


    修改 docker-compose.yml 读取环境变量

    environment:
      ASPNETCORE_ENVIRONMENT: ${ASPNETCORE_ENVIRONMENT}

docker可视化portainer

安装教程参考：https://www.cnblogs.com/wyt007/p/11104253.html

yml文件
复制代码

protainer:
  image: portainer/portainer
  ports:
    - 9000:9000
  volumes:
    - /var/run/docker.sock:/var/run/docker.sock
  restart: always
  networks: 
    - netapp

复制代码
使用python 和 C# 远程访问 docker

    开放tcp端口，方便远程访问
    修改 docker.service ,修改掉ExecStart

    vim /usr/lib/systemd/system/docker.service

    # ExecStart=/usr/bin/dockerd -H fd:// --containerd=/run/containerd/containerd.soc
    ExecStart=/usr/bin/dockerd --containerd=/run/containerd/containerd.soc

    配置 daemon.json 

    vim /etc/docker/daemon.json

    "hosts": ["192.168.103.240:18080","unix:///var/run/docker.sock"]

    刷新配置文件，重启docker

    systemctl daemon-reload
    systemctl restart docker

    查看docker进程是否监听

    netstat -ano | grep 18080

    tcp        0      0 192.168.103.240:18080   0.0.0.0:*               LISTEN      off (0.00/0/0)

python访问docker

官方地址：https://docs.docker.com/develop/sdk/examples/
c#访问docker

社区地址：https://github.com/microsoft/Docker.DotNet
复制代码

class Program
{
    static  async Task Main(string[] args)
    {
        DockerClient client = new DockerClientConfiguration(
                new Uri("http://192.168.103.240:18080"))
            .CreateClient();
        IList<ContainerListResponse> containers = await client.Containers.ListContainersAsync(
            new ContainersListParameters()
            {
                Limit = 10,
            });
        Console.WriteLine("Hello World!");
    }
}

复制代码
cluster volumes

开源分布式文件系统：https://www.gluster.org/

    部署前准备，修改 /etc/hosts 文件，增加如下信息
    2台机器

    vim /etc/hosts

    192.168.103.240 fs1
    192.168.103.226 fs2

    安装GlusterFS   【两个node】

    yum install -y centos-release-gluster
    yum install -y glusterfs-server
    systemctl start glusterd
    systemctl enable glusterd

    将fs2加入到集群中


    # 在fs1中执行
    # 将fs2加入集群节点中
    gluster peer probe fs2
    # 查看集群状态
    gluster peer status
    # 查看集群列表
    gluster pool list
    # 查看所有命令
    gluster help global



    创建volume


    # 创建文件夹（两个都要创建）
    mkdir -p /data/glusterfs/glustervolume
    # 创建同步副本数据卷 replica集群 2复制分发 force强制（fs1）
    gluster volume create glusterfsvolumne replica 2 fs1:/data/glusterfs/glustervolume fs2:/data/glusterfs/glustervolume force
    # 启动卷使用
    gluster volume start glusterfsvolumne



    相当于两台机器都拥有了glusterfsvolumne
    创建本地文件夹挂载 volume 即可


    # 分别创建
    mkdir /app
    # 【交叉挂载】
    # fs1
    mount -t glusterfs fs2:/glusterfsvolumne /app
    # fs2
    mount -t glusterfs fs1:/glusterfsvolumne /app


    View Code
    View Code

    在fs1新建文件

    在fs2中查看

    容器部署

    # fs1 fs2
    # 数据是共享的
    docker run --name some-redis -p 6379:6379 -v /app/data:/data -d  redis

搭建自己的docker swarm集群

集群的搭建

    准备三台服务器

    192.168.103.240 manager1
    192.168.103.226 node1
    192.168.103.227 node2

    初始化swarm

    # 192.168.103.240 manager1
    docker swarm init

    View Code

    加入节点

    # 192.168.103.226 node1
    # 192.168.103.227 node2
    docker swarm join --token SWMTKN-1-10bndgdxqph4nqmjn0g4oqse83tdgx9cbb50pcgmf0tn7yhlno-6mako3nf0a0504tiopu9jefxc 192.168.103.240:2377

 关键词解释

    managernode 

    用于管理这个集群。（manager + work ）
    用于分发task 给 worknode 去执行。
    worknode
    用于执行 manager 给过来的 task。
    给manager report task的执行情况 或者一些 统计信息。
    service 服务
    task 容器
    overlay 网络

swarm操作的基本命令

    docker swarm 

    docker swarm init
    docker swarm join
    docker swarm join-token
    docker swarm leave

    docker node 

    docker node demote / promote 
    docker node ls / ps

    docker service 


    docker service create
    docker service update
    docker service scale
    docker service ls
    docker service ps
    docker service rm




    # 在随机节点上创建一个副本
    docker service create --name redis redis:3.0.6
    # 创建每个节点都有的redis实例
    docker service create --mode global --name redis redis:3.0.6
    # 创建随机节点的5个随机的redis实例
    docker service create --name redis --replicas=5 redis:3.0.6
    # 创建端口映射3个节点的redis实例
    docker service create --name my_web --replicas 3 -p 6379:6379 redis:3.0.6
    # 更新服务，副本集提高成5个
    docker service update --replicas=5 redis
    # 更新服务，副本集提高成2个
    docker service scale redis=2
    # 删除副本集
    docker service rm redis



compose.yml自定义swarm集群

官方文档：https://docs.docker.com/compose/compose-file/#deploy

所有分布式部署都使用compose中的 deploy 进行节点部署
使用compose中的 deploy 进行节点部署

    准备4台服务器

    192.168.103.240 manager1
    192.168.103.228 manager2
    192.168.103.226 node1
    192.168.103.227 node2

    编写 docker-compose.yml 文件


    vim /app/docker-compose.yml

    version: '3.7'
    services:
      webapp:
        image: nginx
        ports:
          - 80:80
        deploy:
          replicas: 5


    运行yml文件

    # 与docker-compose不同，这里是基于stack deploy的概念
    docker stack deploy -c ./docker-compose.yml nginx

    查看stack

    # 查看所有栈
    docker stack ls
    # 查看名称为nginx的栈
    docker stack ps nginx

带状态的容器进行约束

placement:
  constraints:
    - xxxxxx

    借助node的自带信息
    https://docs.docker.com/engine/reference/commandline/service_create/#specify-service-constraints---constraint
    node.id / node.hostname / node.role
    node.id 	Node ID 	node.id==2ivku8v2gvtg4
    node.hostname 	Node hostname 	node.hostname!=node-2
    node.role 	Node role 	node.role==manager
    node.labels 	user defined node labels 	node.labels.security==high
    engine.labels 	Docker Engine's labels
    借助node的自定义标签信息  [更大的灵活性]
    node.labels / node.labels.country==china

让 5个task 分配在 node1节点上

    编写 docker-compose.yml 文件|


    vim /app/docker-compose.yml

    version: '3.7'
    services:
      webapp:
        image: nginx
        ports:
          - 80:80
        deploy:
          replicas: 5
          placement:
            constraints:
              - node.id == icyia3s2mavepwebkyr0tqxly


    运行yml文件

    # 先删除，发布，延迟5秒、查看详情
    docker stack rm nginx &&  docker stack deploy -c ./docker-compose.yml nginx && sleep 5 && docker stack ps nginx

     

让 5 个 task 在东部地区运行

    给node打标签

    docker node update --label-add region=east --label-add country=china  0pbg8ynn3wfimr3q631t4b01s
    docker node update --label-add region=west --label-add country=china  icyia3s2mavepwebkyr0tqxly
    docker node update --label-add region=east --label-add country=usa  27vlmifw8bwyc19tpo0tbgt3e

    编写 docker-compose.yml 文件


    vim /app/docker-compose.yml

    version: '3.7'
    services:
      webapp:
        image: nginx
        ports:
          - 80:80
        deploy:
          replicas: 5
          placement:
            constraints:
              - node.labels.region == east



    运行yml文件

    # 先删除，发布，延迟5秒、查看详情
    docker stack rm nginx &&  docker stack deploy -c ./docker-compose.yml nginx && sleep 5 && docker stack ps nginx

     

让 5 个 task 在中国东部地区运行
复制代码

deploy:
  replicas: 5
  placement:
    constraints:
      - node.labels.region == east
      - node.labels.country == china

复制代码
均匀分布

目前只有 spread 这种策略，用于让task在指定的node标签上均衡的分布。

placement:
  preferences:
    - spread: node.labels.zone

让 8 个task 在 region 均匀分布

    编写 docker-compose.yml 文件


    vim /app/docker-compose.yml

    version: '3.7'
    services:
      webapp:
        image: nginx
        ports:
          - 80:80
        deploy:
          replicas: 8
          placement:
            constraints:
              - node.id != ryi7o7xcww2c9e4j1lotygfbu
            preferences:
              - spread: node.labels.region



    运行yml文件

    # 先删除，发布，延迟5秒、查看详情
    docker stack rm nginx &&  docker stack deploy -c ./docker-compose.yml nginx && sleep 5 && docker stack ps nginx

     

重启策略
复制代码

deploy:
  restart_policy:
    condition: on-failure
    delay: 5s
    max_attempts: 3
    window: 120s

复制代码

默认是any，(always) 单要知道和 on-failure, 前者如果我stop 容器，一样重启， 后者则不是
复制代码

version: '3.7'
services:
  webapp:
    image: nginx
    ports:
      - 80:80
    deploy:
      replicas: 2
      restart_policy:
        condition: on-failure
        delay: 5s
      placement:
        constraints:
          - node.role == worker

复制代码
其他属性

endpoint_mode vip -> keepalive 【路由器的一个协议】
labels：标签信息
mode：分发还是全局模式
resources：限制可用资源
update_config 【覆盖的一个策略】
把之前的单机版程序修改放到分布式环境中

修改 docker-compose.yml 文件
复制代码

version: '3.0'

services:

  webapp:
    image: registry.cn-hangzhou.aliyuncs.com/wyt_registry/wyt_registry
    ports:
      - 80:80
    depends_on:
      - redis
    networks:
      - netapp
    deploy:
      replicas: 3
      placement:
        constraints:
          - node.id == ryi7o7xcww2c9e4j1lotygfbu

  redis:
    image: redis
    networks:
      - netapp
    deploy:
      placement:
        constraints:
          - node.role == worker

  elasticsearch:
    image: elasticsearch:5.6.14
    networks:
      - netapp
    deploy:
      placement:
        constraints:
          - node.role == worker

  kibana:
    image: kibana:5.6.14
    ports:
      - 5601:5601
    networks:
      - netapp
    deploy:
      placement:
        constraints:
          - node.role == worker
networks:
  netapp:

复制代码

在私有仓库拉取的时候记得 带上这个参数，，否则会 no such image 这样的报错的。

docker stack deploy -c ./docker-compose.yml nginx --with-registry-auth
docker新特性
使用config实现全局挂载

    创建config配置


    vim /app/nlog.config

    <?xml version="1.0" encoding="utf-8" ?>
    <nlog xmlns="http://www.nlog-project.org/schemas/NLog.xsd"
          xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
          autoReload="true"
          internalLogLevel="Warn">

        <extensions>
            <add assembly="NLog.Targets.ElasticSearch"/>
        </extensions>

        <targets>
            <target name="ElasticSearch" xsi:type="BufferingWrapper" flushTimeout="5000" >
                <target xsi:type="ElasticSearch" uri="http://elasticsearch:9200" documentType="web.app"/>
            </target>
        </targets>

        <rules>
            <logger name="*" minlevel="Trace" writeTo="ElasticSearch" />
        </rules>
    </nlog>



    # 创建名称为nlog的配置
    docker config create nlog /app/nlog.config

    查看config内容，默认是base64编码


    docker config inspect nlog

    [
        {
            "ID": "1zwa2o8f71i6zm6ie47ws987n",
            "Version": {
                "Index": 393
            },
            "CreatedAt": "2019-07-11T10:30:58.255006156Z",
            "UpdatedAt": "2019-07-11T10:30:58.255006156Z",
            "Spec": {
                "Name": "nlog",
                "Labels": {},
                "Data": "PD94bWwgdmVyc2lvbj0iMS4wIiBlbmNvZGluZz0idXRmLTgiID8+CjxubG9nIHhtbG5zPSJodHRwOi8vd3d3Lm5sb2ctcHJvamVjdC5vcmcvc2NoZW1hcy9OTG9nLnhzZCIKICAgICAgeG1sbnM6eHNpPSJodHRwOi8vd3d3LnczLm9yZy8yMDAxL1hNTFNjaGVtYS1pbnN0YW5jZSIKICAgICAgYXV0b1JlbG9hZD0idHJ1ZSIKICAgICAgaW50ZXJuYWxMb2dMZXZlbD0iV2FybiI+CgogICAgPGV4dGVuc2lvbnM+CiAgICAgICAgPGFkZCBhc3NlbWJseT0iTkxvZy5UYXJnZXRzLkVsYXN0aWNTZWFyY2giLz4KICAgIDwvZXh0ZW5zaW9ucz4KCiAgICA8dGFyZ2V0cz4KICAgICAgICA8dGFyZ2V0IG5hbWU9IkVsYXN0aWNTZWFyY2giIHhzaTp0eXBlPSJCdWZmZXJpbmdXcmFwcGVyIiBmbHVzaFRpbWVvdXQ9IjUwMDAiID4KICAgICAgICAgICAgPHRhcmdldCB4c2k6dHlwZT0iRWxhc3RpY1NlYXJjaCIgdXJpPSJodHRwOi8vZWxhc3RpY3NlYXJjaDo5MjAwIiBkb2N1bWVudFR5cGU9IndlYi5hcHAiLz4KICAgICAgICA8L3RhcmdldD4KICAgIDwvdGFyZ2V0cz4KCiAgICA8cnVsZXM+CiAgICAgICAgPGxvZ2dlciBuYW1lPSIqIiBtaW5sZXZlbD0iVHJhY2UiIHdyaXRlVG89IkVsYXN0aWNTZWFyY2giIC8+CiAgICA8L3J1bGVzPgo8L25sb2c+Cg=="
            }
        }
    ]


    #解密
    <?xml version="1.0" encoding="utf-8" ?>
    <nlog xmlns="http://www.nlog-project.org/schemas/NLog.xsd"
          xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
          autoReload="true"
          internalLogLevel="Warn">

        <extensions>
            <add assembly="NLog.Targets.ElasticSearch"/>
        </extensions>

        <targets>
            <target name="ElasticSearch" xsi:type="BufferingWrapper" flushTimeout="5000" >
                <target xsi:type="ElasticSearch" uri="http://elasticsearch:9200" documentType="web.app"/>
            </target>
        </targets>

        <rules>
            <logger name="*" minlevel="Trace" writeTo="ElasticSearch" />
        </rules>
    </nlog>



    给servcie作用域加上 config 文件, 根目录有一个 nlog 文件

    docker service create --name redis --replicas 3 --config nlog redis

    View Code

    使用docker-compose实现


    vim /app/docker-compose.yml

    version: "3.7"
    services:
      redis:
        image: redis:latest
        deploy:
          replicas: 3
        configs:
          - nlog2
    configs:
      nlog2:
        file: ./nlog.config



    运行

    docker stack deploy -c docker-compose.yml redis --with-registry-auth

    挂载到指定目录（这里的意思是挂在到容器内的/root文件夹内）


    vim /app/docker-compose.yml

    version: "3.7"
    services:
      redis:
        image: redis:latest
        deploy:
          replicas: 1
        configs:
          - source: nlog2
            target: /root/nlog2
    configs:
      nlog2:
        file: ./nlog.config



serect挂载明文和密文

如果你有敏感的配置需要挂载在swarm的service中，可以考虑使用 serect

    用户名和密码
    生产的数据库连接串   

使用方式与config一致,挂在目录在：/run/secrets/<secret_name>

