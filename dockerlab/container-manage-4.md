# container-manage.md

## deploy flow type one 

    1. deploy tools
---
vagrant + virtualbox

    2. deploy archtype
---
    swarm-manager
    swarm-worker1
    swarm-worker2
    3. test 
    vagrant status
    vagrant swarm-manager
    vagrant ssh swarm-manager

docker swarm init --advertise-addr=ip (ip=> manager node )

add worker node to swarm manager:
vagrant ssh swarm-worker1
sudo service docker start
docker swarm join --token TOKEN ip1:2377
vagrant ssh swarm-worker2
sudo service docker start
docker swarm join --token TOKEN ip2:2377

view status
docker node ls

## docker-manchine +virtualbox

##paly with docker (https://labs.play-with-docker.com/) --simple
labs.play-with-docker.com
 

