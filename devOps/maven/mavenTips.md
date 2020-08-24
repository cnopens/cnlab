#Maven common Use manual

##Resources

[https://mvnrepository.com/repos]

## Pom config repository

---
<p>
Pom.xml 配置优先级选择：
<repositories>
        <repository>
            <id>maven-ossez</id>
            <name>OSSEZ Repository</name>
            <url>https://maven.ossez.com/repository/internal</url>
        </repository>
</repositories>
<pluginRepositories>
        <pluginRepository>
            <id>maven-ossez</id>
            <name>OSSEZ Repository</name>
            <url>https://maven.ossez.com/repository/internal</url>
            <snapshots>
                <enabled>true</enabled>
            </snapshots>
        </pluginRepository>
</pluginRepositories>

<repositories>
        <repository>
            <id>spring-snapshots</id>
            <url>http://repo.spring.io/snapshot</url>
            <snapshots><enabled>true</enabled></snapshots>
        </repository>
        <repository>
            <id>spring-milestones</id>
            <url>http://repo.spring.io/milestone</url>
        </repository>
    </repositories>
    <pluginRepositories>
        <pluginRepository>
            <id>spring-snapshots</id>
            <url>http://repo.spring.io/snapshot</url>
        </pluginRepository>
        <pluginRepository>
            <id>spring-milestones</id>
            <url>http://repo.spring.io/milestone</url>
        </pluginRepository>
    </pluginRepositories>

</p>


## mvn 本地安装自定义jar
mvn install:install-file -Dfile=/home/cnopens/workspace/lib/ojdbc16-12.1.0.1.jar -DgroupId=com.oracle -DartifactId=ojdbc16 -Dversion=12.1.0.1 -Dpackaging=jar

## 打包

method1:
mvn package -DskipTests 或 mvn package -Dmaven.test.skip=true
method2:
<plugin>
<groupId>org.apache.maven.plugins</groupId>
<artifactId>maven-surefire-plugin</artifactId>
<version>2.18.1</version>
  <configuration>
  <skipTests>true</skipTests>
  </configuration>
</v-container>
</plugin>



## setttings.xml config

mvn install:install-file -Dfile=/home/cnopens/workspace/lib/ojdbc16-12.1.0.1.jar -DgroupId=com.oracle -DartifactId=ojdbc16 -Dversion=12.1.0.1 -Dpackaging=jar

export M2_HOME=/home/cnopens/servers/apache-maven-3.6.3

intellij idea:maven config
Runner VM Options -Xms1024m -Xmx1024m



## Maven工具之 GPG

-What? -> Maven使用GPG对文件进行签名加密.
~~~

    <plugin>
        <groupId>org.apache.maven.plugins</groupId>
        <artifactId>maven-gpg-plugin</artifactId>
        <executions>
            <execution>
                <id>sign-artifacts</id>
                <phase>verify</phase>
                <goals>
                    <goal>sign</goal>
                </goals>
            </execution>
        </executions>
    </plugin>

~~~

Question: Could not find artifact org.apache.maven.plugins:maven-gpg-plugin:pom: in alimaven ?

<build>
   <plugins>

      <plugin></plugin>

      ...

       <plugin></plugin>

    </plugins>

</build>

Change : 

<build>
   <pluginManagement> ##显示声明处理
   <plugins>

      <plugin></plugin>

      ...

       <plugin></plugin>

    </plugins> 
</pluginManagement>
</build>

## Maven pom lifecycle


## Maven plugins Version -> jDK




## Maven plugins knowlegues

1. maven-assembly-plugin (package plugin)

2. maven-failsafe-plugin (failsafe实现集成测试-integrated test)


## mavan Qa?

1. Maven: jdk.tools:jdk.tools:1.8 报错不存在？

A:


