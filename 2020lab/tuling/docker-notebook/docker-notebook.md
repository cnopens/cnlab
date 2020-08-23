#docker-notebook.md

common usage images run :


docker run -d --name tomcat01 -p 9090:8080 -v /tmp/cnmall:/usr/local/tomcat/webapps/cnmall tomcat


customalble volume name: mysql01_vol
docker run -d --name mysql01 -v mysql01_vol:/var/lib/mysql -e MYSQL_ROOT_PASSWORD=123.com mysql


## ha cluster (pxc) percona-xtradb-cluster

mysql镜像的一个封装，方便搭建pxc,强一致性高可用的集群，image-->Ha mysql 搭建 

docker pull percona/percona-xtradb-cluster:5.7.21

1. 网络 
pxc-net 172.18.0.0/24

docker network ls

docker network create --subnet=172.18.0.0/24 pxc-net


2. 持久化
docker volume create --name v1 --mysql1 
v2-mysql02 
v3-mysql03

docker volume create --name v1
docker volume create --name v2
docker volume create --name v3

3. 容器如何创建 MY = SQL_ROOT_PASSWORD
`docker run -d -p 3301:3306 -v v1:/var/lib/mysql -e MYSQL_ROOT_PASSWORD=123.com -e CLUSTER_NAME=PXC -e XTRABACKUP_PASSWORD=123.com --name node1 --net=pxc-net 172.18.0.2 pxc

create container command:
node1: 

docker run -d -p 3301:3306 -v v1:/var/lib/mysql -e MYSQL_ROOT_PASSWORD=123.com -e CLUSTER_NAME=PXC -e XTRABACKUP_PASSWORD=123.com --privileged --name=node1 --net=pxc-net --ip 172.18.0.2 pxc

node2:

docker run -d -p 3302:3306 -v v2:/var/lib/mysql -e MYSQL_ROOT_PASSWORD=123.com -e CLUSTER_NAME=PXC -e XTRABACKUP_PASSWORD=123.com -e CLUSTER_JOIN=node1 --privileged --name=node2 --net=pxc-net --ip 172.18.0.3 pxc

node3:

docker run -d -p 3303:3306 -v v3:/var/lib/mysql -e MYSQL_ROOT_PASSWORD=123.com -e CLUSTER_NAME=PXC -e XTRABACKUP_PASSWORD=123.com -e CLUSTER_JOIN=node1 --privileged --name=node2 --net=pxc-net --ip 172.18.0.4 pxc


如何将3个node互通？

-e CLUSTER_JOIN=node1 这个参数很关建！

pxc 原理？


## Cluster deploy

Haproxy pull

1. docker pull haproxy







HaProxy container:

1. pxc container <-> 2. pxc container <-> 3. pxc container

1. <--> 3. 

haproxy.cfg


  1 global 
  2         chroot /usr/local/etc/haproxy 
  3         log 127.0.0.1 local5 info 
  4         daemon 
  5 defaults 
  6         log global 
  7         mode    http 
  8         option  httplog 
  9         timeout connect 5000 
 10         timeout client  50000
 11                                                  1,1           All
