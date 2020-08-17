#docker-sources-mirrors.md

在国内访问国外的Docker镜像源通常都是非常慢的，特别是最近GFW升级后，就变得更加慢了，因为要使用Docker中的镜像，这个时候最好就是将镜像指向国内的资源。

国内亲测可用的几个镜像源：

    Docker 官方中国区：https://registry.docker-cn.com
    网易：http://hub-mirror.c.163.com
    中国科技大学：https://docker.mirrors.ustc.edu.cn
    阿里云：https://y0qd3iq.mirror.aliyuncs.com

增加Docker的镜像源配置文件 /etc/docker/daemon.json，如果没有配置过镜像该文件默认是不存的，在其中增加如下内容：

    {
      "registry-mirrors": ["https://y0qd3iq.mirror.aliyuncs.com"]
    }

其中的URL就是指定的镜像源，可以将其设置为上面说的四个镜像源中的任何一个。

然后重启Docker服务：

service docker restart

然后通过以下命令查看配置是否生效：

docker info|grep Mirrors -A 1

可以看到如下的输出：

    Registry Mirrors:
     https://y0qd3iq.mirror.aliyuncs.com/

就表示镜像配置成功，然后再执行docker pull操作，就会很快了。

 
修改pip镜像源

首先在当前用户目录下建立文件夹.pip，然后在文件夹中创建pip.conf文件，再将源地址加进去即可。

mkdir ~/.pip
vim ~/.pip/pip.conf
# 然后将下面这两行复制进去就好了
[global]
index-url = https://mirrors.aliyun.com/pypi/simple

#--------------------------------------------------------------------
国内其他pip源

    清华：https://pypi.tuna.tsinghua.edu.cn/simple
    中国科技大学 https://pypi.mirrors.ustc.edu.cn/simple/
    华中理工大学：http://pypi.hustunique.com/
    山东理工大学：http://pypi.sdutlinux.org/
    豆瓣：http://pypi.douban.com/simple/

注意：不管你用的是pip3还是pip，方法都是一样的，都是创建pip文件夹。



"insecure-registries" : ["http://registry.cn-hangzhou.aliyuncs.com","192.168.31.132","192.168.31.136"]

"insecure-registries" : ["https://y0qd3iq.mirror.aliyuncs.com","192.168.31.132","192.168.31.136"]