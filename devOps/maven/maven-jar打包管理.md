# maven-jar打包管理.md

在Maven中，主要有3个插件可以用来打包：

    1.maven-jar-plugin，默认的打包插件，用来打普通的project JAR包；
     
    2.maven-shade-plugin，用来打可执行JAR包，也就是所谓的fat JAR包；
     
    3.maven-assembly-plugin，支持自定义的打包结构，也可以定制依赖项等。

使用maven-assembly-plugin插件可以定制依赖：

    <!-- maven 打包集成插件 -->
    <plugin>
    	<artifactId>maven-assembly-plugin</artifactId>
    	<executions>
    		<execution>
    			<!-- 绑定到package生命周期 -->
    			<phase>package</phase>
    			<goals>
    				<!-- 只运行一次 -->
    				<goal>single</goal>
    			</goals>
    		</execution>
    	</executions>
    	<configuration>
    		<descriptorRefs>
    			<!-- 将依赖一起打包到 JAR -->
    			<descriptorRef>jar-with-dependencies</descriptorRef>
    		</descriptorRefs>
    		<archive>
    			<manifest>
    				<!-- 配置主程序 java -jar 默认Class -->
    				<addClasspath>true</addClasspath>
    				<classpathPrefix>lib/</classpathPrefix>
    				<mainClass>com.xx.xx.xx</mainClass>
    			</manifest>
    		</archive>
    	</configuration>
    </plugin>


## 使用方法
 <build>
        <plugins>

            <plugin>
                <artifactId>maven-assembly-plugin</artifactId>
                <configuration>
                    <appendAssemblyId>false</appendAssemblyId>
                    <descriptorRefs>
                        <descriptorRef>jar-with-dependencies</descriptorRef>
                    </descriptorRefs>
                    <archive>
                        <manifest>
                            <!-- 此处指定main方法入口的class -->
                            <mainClass>com.xxx.uploadFile</mainClass>
                        </manifest>
                    </archive>
               ta </configuration>
                <executions>
                    <execution>
                        <id>make-assembly</id>
                        <phase>package</phase>
                        <goals>
                            <goal>assembly</goal>
                        </goals>
                    </execution>
                </executions>
            </plugin>

        </plugins>
    </build>


1. mvn package

2. 运行jar
java -jar test.jar
也可以通过如下命令
mvn assembly:assembly

3. 跳过测试 
mvn -Dmaven.test.skip=true  assembly:assembly

