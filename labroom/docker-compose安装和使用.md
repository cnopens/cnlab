# docker-compose安装和使用.md
----------------------------------------------------------------------------

1、docker-compose简介

　　Compose 是用于定义和运行多容器 Docker 应用程序的工具。通过 Compose，您可以使用 YML 文件来配置应用程序需要的所有服务。然后，使用一个命令，就可以从 YML 文件配置中创建并启动所有服务。

2、安装

Linux

Linux 上我们可以从 Github 上下载它的二进制包来使用，最新发行的版本地址：https://github.com/docker/compose/releases。

运行以下命令以下载 Docker Compose 的当前稳定版本：

$ sudo curl -L "https://github.com/docker/compose/releases/download/1.29.0/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose

要安装其他版本的 Compose，请替换 1.29.0。

将可执行权限应用于二进制文件：

$ sudo chmod +x /usr/local/bin/docker-compose

创建软链：

$ sudo ln -s /usr/local/bin/docker-compose /usr/bin/docker-compose

测试是否安装成功：

$ docker-compose --version
cker-compose version 1.24.1, build 4667896b

注意： 对于 alpine，需要以下依赖包： py-pip，python-dev，libffi-dev，openssl-dev，gcc，libc-dev，和 make。
macOS

Mac 的 Docker 桌面版和 Docker Toolbox 已经包括 Compose 和其他 Docker 应用程序，因此 Mac 用户不需要单独安装 Compose。
windows PC

Windows 的 Docker 桌面版和 Docker Toolbox 已经包括 Compose 和其他 Docker 应用程序，因此 Windows 用户不需要单独安装 Compose。
3、准备

使用Compose基本上需要三个步骤：

    用Dockerfile定义应用程序的环境，以便可以在任何地方复制。

    在docker-compose.yml 中定义组成应用程序的服务，所以它们可以在一个孤立的环境中运行

    运行 docker-compose up 和Compose启动并运行整个应用程序。

docker-compose.yml 标准格式：

```
version: "3.8"
services:
  web:　　　　　　　　　　　　　　　　　　#运行的服务1：web
    build: .
    ports:
      - "5000:5000"
    volumes:
      - .:/code
      - logvolume01:/var/log
    links:　　　　　　　　　　　　　　　　#与哪个容器进行相连
      - redis
  redis:　　　　　　　　　　　　　　　　　#运行的服务2：redis
    image: redis
volumes:
  logvolume01: {}<br>（每一行空两格）
```
　
4、使用和运行

1、准备
创建一个测试目录：

$ mkdir composetest
$ cd composetest

在测试目录中创建一个名为 app.py 的文件，并复制粘贴以下内容：

composetest/app.py 文件代码
```
import time
 
import redis
from flask import Flask
 
app = Flask(__name__)
cache = redis.Redis(host='redis', port=6379)
 
 
def get_hit_count():
    retries = 5
    while True:
        try:
            return cache.incr('hits')
        except redis.exceptions.ConnectionError as exc:
            if retries == 0:
                raise exc
            retries -= 1
            time.sleep(0.5)
 
 
@app.route('/')
def hello():
    count = get_hit_count()
    return 'Hello World! I have been seen {} times.\n'.format(count)
```

在此示例中，redis 是应用程序网络上的 redis 容器的主机名，该主机使用的端口为 6379。

在 composetest 目录中创建另一个名为 requirements.txt 的文件，内容如下：

flask
redis　
2、创建 Dockerfile 文件

在 composetest 目录中，创建一个名为的文件 Dockerfile，内容如下：

```
FROM python:3.7-alpine　　　　　　　　　　　　　　　　　　　#从 Python 3.7 映像开始构建镜像。
WORKDIR /code　　　　　　　　　　　　　　　　　　　　　　　　 #将工作目录设置为 /code。
ENV FLASK_APP app.py
ENV FLASK_RUN_HOST 0.0.0.0　　　　　　　　　　　　　　　　　#设置 flask 命令使用的环境变量。
RUN apk add --no-cache gcc musl-dev linux-headers　　　#安装 gcc，以便诸如 MarkupSafe 和 SQLAlchemy 之类的 Python 包可以编译加速。
COPY requirements.txt requirements.txt　　　　　　　　　　
RUN pip install -r requirements.txt　　　　　　　　　　　　#复制 requirements.txt 并安装 Python 依赖项。
COPY . .　　　　　　　　　　　　　　　　　　　　　　　　　　　　#将 . 项目中的当前目录复制到 . 镜像中的工作目录。
CMD ["flask", "run"]　　　　　　　　　　　　　　　　　　　　　#容器提供默认的执行命令为：flask run。
```

3、创建 docker-compose.yml

在测试目录中创建一个名为 docker-compose.yml 的文件，然后粘贴以下内容：
docker-compose.yml 配置文件

	
# yaml 配置
```
version: '3'
services:
  web:
    build: .
    ports:
     - "5000:5000"
  redis:
    image: "redis:alpine"
```
该 Compose 文件定义了两个服务：web 和 redis。

    web：该 web 服务使用从 Dockerfile 当前目录中构建的镜像。然后，它将容器和主机绑定到暴露的端口 5000。此示例服务使用 Flask Web 服务器的默认端口 5000 。
    redis：该 redis 服务使用 Docker Hub 的公共 Redis 映像。

4、使用 Compose 命令构建和运行您的应用

在测试目录中，执行以下命令来启动应用程序：
	
docker-compose up

如果想在后台执行该服务可以加上 -d 参数：
	
docker-compose up -d