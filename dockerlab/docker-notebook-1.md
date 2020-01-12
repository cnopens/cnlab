# docker-notebook.md

* Base Knowlegue

网络分层
    ISO/OST
    TCP/IP




* Docker 底层核心
    1.  namespace
    2. group
    3. container


* Container
    1. 


    sudo ip netns list 
sudo ip netns delete/add name1

* between Container and container 
sudo ip link add veth-test1 type veth peer name veth-test2
sudo ip netns exec test1 ip addr add 192.168.1.1/24 dev veth-test1
sudo ip netns exec test2 ip addr add 192.168.1.2/24 dev veth-test2 

up operation:
sudo ip netns exec test1 ip link (start)

view info :
sudo ip netns exec test1 ip a

ping operation:
sudo ip netns exec test1 ping 192.168.1.2
sudo ip netns exec test1 ping 192.168.1.2

* container port mapping 


command usage :
sudo docker ps

Inner service expose:
curl ip
sudo docker stop web
sudo docker rm web
port maping:=>service expose 
sudo docker run --name web -d -o 80:80 nginx 

* Container communication 
sudo docker run -d --link redis --name flask-redis -e  REDIS_HOST=redis xiaopeng163/flask-redis (set env variable -e )
sudo docker run -d -p 5000:5000 --link redis --name flask-redis -e  REDIS_HOST=redis xiaopeng163/flask-redis

* Container network
sudo docker network ls
sudo docker run -d --name test1 --network none|host busybox /bin/sh -C "while true; do sleep 3600; done"
view network cmd:
sudo docker network inspect none

## docker persistent
### Volume type:
* Managed data Volume ,create in backend auto
* Binded mount Volumne ,where may the user decide to mount 

### Usage command-volume type:
* sudo docker volume rm id(spec)
* sudo docker run -d --name mysql1 -e MYSQL_ALLOW_EMPTY_PASSWORD=true mysql
* view volume info:sudo docker volume ls,sudo docker volumen inspect volumenId
Note :volume name is friendly
sudo docker run -d -v mysql:/var/lib/mysql --name mysql1 -e  MYSQL_ALLOW_EMPTY_PASSWORD=true  mysql (add volumen named)
Note tip: force rm running container:
sudo docker rm -f mysql1 

### Usage command bind mounting type:
* sudo docker run -v /home/aaa:/root/aaa
for example lab:
docker run -d -p 80:80 --name web xioapeng/mynginx
docker run -d -v $(pwd):/usr/share/nginx/html -p 80:80 --name web xioapeng/mynginx


###  dk-bind-mount.md
* sych fold
sudo docker run -d -p 80:5000 -v $(pwd)=/skeleton --name flask xiaoming/flask-skeleton
Note tips:
this is the first Step before devops







