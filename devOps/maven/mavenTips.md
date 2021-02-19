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




## 开发配置类

### Tomcat嵌入式配置
<plugins>
    <!-- 指定jdk1.7编译，否则maven update 可能调整jdk -->
    <plugin>
        <groupId>org.apache.maven.plugins</groupId>
        <artifactId>maven-compiler-plugin</artifactId>
        <version>2.3.2</version>
        <configuration>
            <source>1.7</source>
            <target>1.7</target>
            <encoding>UTF-8</encoding>
        </configuration>
    </plugin>
    <!-- tomcat7插件。使用方式：tomcat7:run -->
    <plugin>
        <groupId>org.apache.tomcat.maven</groupId>
        <artifactId>tomcat7-maven-plugin</artifactId>
        <version>2.2</version>
        <configuration>
            <update>true</update>
            <port>8080</port>
            <uriEncoding>UTF-8</uriEncoding>
            <server>tomcat7</server>
            <!-- tomcat虚拟映射路径 -->
            <staticContextPath>/store</staticContextPath>
            <staticContextDocbase>d:/file/store/</staticContextDocbase>
            <contextReloadable>false</contextReloadable>
            <useTestClasspath>true</useTestClasspath>
        </configuration>
    </plugin>
</plugins>

### 其他类型
