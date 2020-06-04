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