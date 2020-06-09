CentOS7常用命令操作.md

CentOS 7.0默认使用的是firewall作为防火墙

查看防火墙状态

firewall-cmd --state

停止firewall

systemctl stop firewalld.service

禁止firewall开机启动

systemctl disable firewalld.service 

转自：CentOS 6和CentOS 7防火墙的关闭

关闭selinux 
进入到/etc/selinux/config文件

vi /etc/selinux/config

将SELINUX=enforcing改为SELINUX=disabled

配制免密登录的命令

ssh-keygen -t rsa 
ssh-copy-id root@master


scp -P 22 /Users/che/Downloads/jdk-8u161-linux-x64.rpm root@192.168.1.100:/software/

 安装: yum install -y jdk-8u161-linux-x64.rpm
 设置环境变量
 cd /usr/java/

 /usr/java/jdk1.8.0_161

 vi /etc/profile

 export JAVA_HOME=/usr/java/jdk1.8.0_161
 export PATH=$JAVA_HOME/bin:$PATH

安装hadoop 
下载安装包 
hadoop-26.5.tar.gz

cd /usr/
mv /software/hadoop-2.6.5.tar.gz  ./
解压
tar -xzvf ./hadoop-2.6.5.tar.gz

创建目录
   namenode目录：/data/hadoop/namenode
   data目录：    /data/hadoop/data
   tmp目录：     /data/hadoop/tmp
   mkdir -p /data/hadoop/namenode

配制
   core-site.xml
   hdfs-site.xml
   mapred-site.xml.template   mapred-site.xml
   cp mapred-site.xml.template   mapred-site.xml
   yarn-site.xml 

   slaves   (slave1,slave2)
   masters  (master)

   hadoop-env.sh
   export JAVA_HOME=/usr/java/jdk1.8.0_161

   将master的配制copy到slave1、slave2
   scp -r ./* root@slave1:/software/hadoop-2.6.5/etc/hadoop/

 格式化
   cd hadoop-2.6.5/bin
   ./hdfs namenode -format

 启动
   cd ../sbin/

   ./start-dfs.sh

   ssh-copy-id root@master 

   jps(检查一下)

   ./start-yarn.sh

   cd ../bin

   hadoop fs -ls /
   hadoop fs -mkdir /user

   /software/hadoop-2.6.5/bin

   vi/etc/profile

   export HADOOP_HOME=/software/hadoop-2.6.5
   export PATH=$JAVA_HOME/bin:$HADOOP_HOME/bin:$PATH

   source /etc/profile

   hadoop fs -put /software/hadoop-2.6.5.tar.gz /user

   测试2：
   cd share/hadoop/mapreduce
   hadoop jar ./hadoop-mapreduce-examples-2.6.5.jar pi 5 10

   192.168.1.100:50070
   192.168.1.100:8088

   修改ssh端口
   vim /etc/ssh/sshd_config  

设置ntp时间同步服务 
1、安装ntp 
yum install -y ntp 
2、设置NTP服务开机启动 
chkconfig ntpd on 
service nptd start