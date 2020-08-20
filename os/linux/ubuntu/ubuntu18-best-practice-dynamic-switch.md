#ubuntu18-best-practice-dynamic-switch.md

## for jdk and rel other configure

init:
sudo mkdir /usr/lib/jvm
sudo tar -zxvf jdk-7u60-linux-x64.gz -C /usr/lib/jvm

1. this is my basic config
#export JAVA_HOME=/home/cnopens/servers/jdk/jdk11
export JAVA_HOME=/home/cnopens/servers/jdk/jdk1.8.0_181
#change jdk position:
#export JAVA_HOME=/usr/lib/jvm/jdk-11.0.7
export JAVA_HOME=/usr/lib/jvm/jdk1.8.0_181
export JRE_HOME=${JAVA_HOME}/jre
export CLASSPATH=.:${JAVA_HOME}/lib:${JRE_HOME}/lib:$CLASSPATH
export JAVA_PATH=${JAVA_HOME}/bin:${JRE_HOME}/bin
export M2_HOME=/home/cnopens/servers/apache-maven-3.6.3
export PATH=$PATH:${JAVA_PATH}:$M2_HOME/bin

2. effect
source ~/.bashrc

3. registry jdk 
使用 update-alternatives 命令把Java安装到系统上。

for example

Oracle Java JDK 11
```

sudo update-alternatives --install /usr/bin/java java /usr/jdk-11.*/bin/java 2

for example:
sudo update-alternatives --install /usr/bin/java java /usr/lib/jvm/jdk-11.0.7/bin/java 300

```

Oracle Java JDK 8

```

sudo update-alternatives --install /usr/bin/java java /usr/jdk1.8.*/bin/java 2

for exmaple:
sudo update-alternatives --install /usr/bin/java java /usr/lib/jvm/jdk1.8.0_181/bin/java 300

```

jdk version Swich

sudo update-alternatives –config java