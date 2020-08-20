#docker-log-opera.md

---
1.显示所有log

docker logs [OPTIONS] <CONTAINER>   #显示某个容器的所有log

docker-compose logs  #显示启动的所有容器的log

2.显示实时log（此效果和Linux的tail -f filename）一样，可以把最新的内容刷新到屏幕上）

docker logs -f <CONTAINER>

3.使用tail查看log尾部

docker logs --tail 20 <CONTAINER>  #查看容器最新的20条信息

4.使用grep过滤log

docker logs |grep error

5.根据时间查看log

docker logs --since 2020-03-26T15:00:00 <CONTAINER>  #查看2020-03-26T15:00:00之后的日志

docker logs --since 2020-03-26T15:00:00 --until 2020-03-27T11:00:00 <CONTAINER>  #查看2020-03-26T15:00:00到2020-03-27T11:00:00时间段的日志

 6.组合使用

docker logs --tail 10 <CONTAINER> | grep info

docker logs -f --since xxx --tail 10 <CONTAINER>

7.把日志写入文件

docker logs -t <CONTAINER> | grep error >> logs_error.txt
