# docker-common-usage-commands.md


1. docker 查看端口占用列表

netstat -nlp |grep docker-proxy|awk '{print $4}'|sort



2. docker 查看容器开放的端口

docker  port  容器

3. 如何将docker指定端口开放

