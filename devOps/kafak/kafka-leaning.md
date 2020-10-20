#kafka-leaning.md

1. linux后台永久启动Kafka、Flume命令
$ nohup kafka-server-start.sh /home/espai/kafka/config/server.properties 1>/dev/null 2>&1 &

2. 后台启动Flume:  首位也要加上nohup 
apache-flume-1.8.0-bin/bin/flume-ng agent --conf --conf-file apache-flume1.8.0-bin/conf/flume-conf.properties --name a1 -Dflume.root.logger=INFO.console 1>/dev/null 2>&1 &


注：

```
 a. 1>/dev/null  2>&1 是将命令产生的输入和错误都输入到空设备，也就是不输出的意思。/dev/null 代表空设备，“/”不要漏掉，否则会报 -bash: dev/null not a file or dirctory 的错误 ；

 b.  /home/espai/kafka/ 指的是config/server.properties存在的路径，可以根据自己安装的路径进行修改。

 c.  启动命令首位加上nohup，即使停掉crt，kafka、flume依然可以在后台执行，这样就不用每次登陆，重新运行启动命令了。如果需要停掉服务，只需运行 kill -9 [程序运行的号即可，比如上面kafka的是15568]   --> kill -9 15568

```

