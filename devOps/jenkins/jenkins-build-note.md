#jenkins-build-note.md
----

## install && prepare 

Jenkins Debian Packages

wget -q -O - https://pkg.jenkins.io/debian-stable/jenkins.io.key | sudo apt-key add -

Then add the following entry in your /etc/apt/sources.list: 

deb https://pkg.jenkins.io/debian-stable binary/


Update your local package index, then finally install Jenkins:


sudo apt-get update
sudo apt-get install jenkins
  
You will need to explicitly install a Java runtime environment, because Oracle's Java RPMs are incorrect and fail to register as providing a java dependency. Thus, adding an explicit dependency requirement on Java would force installation of the OpenJDK JVM.

2.164 (2019-02) and newer: Java 8 or Java 11
2.54 (2017-04) and newer: Java 8
1.612 (2015-05) and newer: Java 7



## learning route map
[1]基于jenkins构建持续集成环境(https://shimo.im/doc/n60fya8SozwljRip?r=2OONj0)


课程大纲

jenkins原理讲解，通过git,maven gitlab ,tomcat 构建持续集成环境
jenkins部署
jenkins项目配置与管理


gitlab(私服创建）获取我们打包的源码





jenkins 部署
https://jenkins.io/download
下载对应war包两种启动方式

1.直接基于任何servlet容器（jetty\tomcat)等即可启动
2.基于java -jar 命令启动
java -jar jenkins.war --ajp13Port=-1 --httpPort=8888

关于Jenkins插件安装

1.首先选择默认推荐的插件安装完成
2.进入插件管理页面安装如下插件
#maven 管理插件