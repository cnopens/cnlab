#Jenkins集成SonarQube-Scanner.md

1.   安装Jenkins

下载安装包，这里我们下载war包

https://jenkins.io/download/

运行jenkins.war的方式有两种：

第一种：将其放到tomcat中运行（放到webapps目录下，启动tomcat）

第二种：直接执行  java -jar jenkins.war --httpPort=8080

https://jenkins.io/doc/pipeline/tour/getting-started/

这里我们选择第一种方式

启动tomcat（bin/startup.sh）以后访问 http://localhost:8080/jenkins/

至此，jenkins安装完成

 

2.  安装SonarQube Scanner插件

2.1.  安装插件

https://plugins.jenkins.io/sonar

重启jenkins

 

2.1.  配置SonarQube

首先，在SonarQube中生成一个Token（PS：用token代替输入用户名和密码）

然后，在Jenkins中配置连接sonarqube服务器的地址，这里用到的token就是刚才在sonarqube中创建的那个token

最后，配置全局工具配置

 

3.  创建任务

最最重要的是，配置SonarQube analysis properties

可以将其单独写到一个配置文件（sonar-project.properties）里面，也可以像这样每次都写一遍
复制代码

sonar.projectKey=ks-cms-unicorn
sonar.projectName=ks-cms-unicorn
sonar.projectVersion=1.0

sonar.language=java
sonar.sourceEncoding=UTF-8

sonar.sources=$WORKSPACE
sonar.java.binaries=$WORKSPACE

复制代码

其中，sonar.java.binaries属性至关重要，笔者也是试了好多次

相关文档在这里：

https://github.com/SonarSource/sonar-scanning-examples/blob/master/sonarqube-scanner/sonar-project.properties

https://docs.sonarqube.org/display/PLUG/Java+Plugin+and+Bytecode