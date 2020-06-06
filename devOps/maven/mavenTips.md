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
mvn install:install-file -Dfile=D:\ds-projects\ojdbc16-12.1.0.1.jar -DgroupId=com.oracle -DartifactId=ojdbc16 -Dversion=12.1.0.1 -Dpackaging=jar

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