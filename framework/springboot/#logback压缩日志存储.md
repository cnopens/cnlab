#logback压缩日志存储.md
---
java 中使用logback日志，并实现日志按天分类压缩保存。

以maven项目作为构建工具为例，首先引入使用logback需要的3个依赖，需要注意使用logback是需要引入slf4j-api的，因为logback是基于slf4j的

<!--logback-->
<dependency>
  <groupId>ch.qos.logback</groupId>
  <artifactId>logback-core</artifactId>
  <version>1.2.3</version>
</dependency>
<dependency>
  <groupId>ch.qos.logback</groupId>
  <artifactId>logback-classic</artifactId>
  <version>1.2.3</version>
</dependency>
<dependency>
  <groupId>org.slf4j</groupId>
  <artifactId>slf4j-api</artifactId>
  <version>1.7.25</version>
</dependency>

引入依赖之后下面，我们可以在classpath下创建logback.xml文件，并按需要进行相应的配置。

下面的配置案例中实现的是按照每天将debug info error的日志进行压缩，并分别存储到对应的文件夹下

<?xml version="1.0" encoding="UTF-8"?>
<configuration scan="true" scanPeriod="60 seconds" debug="false">


    <!-- 日志级别
    trace<debug<info<warn<error
    若定义的日志级别为info，则不会打印出 trace和debug的相关日志。
     -->

    <!-- 定义全局参数常量 -->
    <property name="log.level" value="debug"/>
    <property name="log.maxHistory" value="30"/><!-- 30表示30个 -->
    <!-- 日志的存放位置 -->                    <!--catalina.base表示tomcat的根路径  -->
    <property name="log.filePath" value="${catalina.base}/logs/webapps"/>
    <!-- 日志的展现格式 -->
    <property name="log.pattern" value="%d{yyyy-MM-dd : HH:mm:ss.SSS}[%thread]%-5level%logger{50}-%msg%n"/>

    <!-- 定义appender (日志的输出和存放位置等). -->
    <!-- 控制台设置 -->
    <appender name="consoleAppender" class="ch.qos.logback.core.ConsoleAppender">
        <encoder>
            <pattern>${log.pattern}</pattern>  <!-- 控制台日志输出格式 -->
        </encoder>
    </appender>

    <!-- DEBUG -->
    <appender name="debugAppender" class="ch.qos.logback.core.rolling.RollingFileAppender"><!-- 日志文件会滚动 -->
        <!-- 文件路径 -->
        <file>${log.filePath}/debug.log</file><!-- 当前的日志文件存放路径 -->
        <!-- 日志滚动策略 -->
        <rollingPolicy class="ch.qos.logback.core.rolling.TimeBasedRollingPolicy">
            <!-- 历史日志文件的存放路径和名称 -->
            <fileNamePattern>${log.filePath}/debug/debug.%d{yyyy-MM-dd}.log.gz</fileNamePattern>
            <!-- 日志文件最大的保存历史 数量-->
            <maxHistory>${log.maxHistory}</maxHistory>
        </rollingPolicy>
        <encoder>
            <pattern>${log.pattern}</pattern>  <!-- 日志文件中日志的格式 -->
        </encoder>
        <filter class="ch.qos.logback.classic.filter.LevelFilter">
            <level>DEBUG</level>
            <onMatch>ACCEPT</onMatch>  <!-- 用过滤器，只接受DEBUG级别的日志信息，其余全部过滤掉 -->
            <onMismatch>DENY</onMismatch>
        </filter>
    </appender>

    <!-- INFO -->
    <appender name="infoAppender" class="ch.qos.logback.core.rolling.RollingFileAppender"><!-- 日志文件会滚动 -->
        <!-- 文件路径 -->
        <file>${log.filePath}/info.log</file><!-- 当前的日志文件存放路径 -->
        <!-- 日志滚动策略 -->
        <rollingPolicy class="ch.qos.logback.core.rolling.TimeBasedRollingPolicy">
            <!-- 历史日志文件的存放路径和名称 -->
            <fileNamePattern>${log.filePath}/info/info.%d{yyyy-MM-dd}.log.gz</fileNamePattern>
            <!-- 日志文件最大的保存历史 数量-->
            <maxHistory>${log.maxHistory}</maxHistory>
        </rollingPolicy>
        <encoder>
            <pattern>${log.pattern}</pattern>  <!-- 日志文件中日志的格式 -->
        </encoder>
        <filter class="ch.qos.logback.classic.filter.LevelFilter">
            <level>INFO</level>
            <onMatch>ACCEPT</onMatch>  <!-- 用过滤器，只接受INFO级别的日志信息，其余全部过滤掉 -->
            <onMismatch>DENY</onMismatch>
        </filter>
    </appender>

    <!-- ERROR-->
    <appender name="errorAppender" class="ch.qos.logback.core.rolling.RollingFileAppender"><!-- 日志文件会滚动 -->
        <!-- 文件路径 -->
        <file>${log.filePath}/error.log</file><!-- 当前的日志文件存放路径 -->
        <!-- 日志滚动策略 -->
        <rollingPolicy class="ch.qos.logback.core.rolling.TimeBasedRollingPolicy"> <!-- TimeBased默认是一天更新一次 -->
            <!-- 历史日志文件的存放路径和名称 -->
            <fileNamePattern>${log.filePath}/error/error.%d{yyyy-MM-dd}.log.gz</fileNamePattern>
            <!-- 日志文件最大的保存历史 数量-->
            <maxHistory>${log.maxHistory}</maxHistory>
        </rollingPolicy>
        <encoder>
            <pattern>${log.pattern}</pattern>  <!-- 日志文件中日志的格式 -->
        </encoder>
        <filter class="ch.qos.logback.classic.filter.LevelFilter">
            <level>ERROR</level>
            <onMatch>ACCEPT</onMatch>  <!-- 用过滤器，只接受ERROR级别的日志信息，其余全部过滤掉 -->
            <onMismatch>DENY</onMismatch>
        </filter>
    </appender>

    <!-- logger标签用于和appender进行绑定，并告诉logback哪些包（name属性）的日志信息需要记录 -->
    <!--logger将会继承root标签，在加上additivity="true"的属性后 root标签中的level将会被logger的level覆盖-->
　　

　　<!--
　　　　additivity
　　　　它是 子Logger 是否继承 父Logger(这里为root) 的 输出源（appender）的标志位。
　　　　具体说，默认情况下子Logger会继承父Logger的appender，也就是说子Logger会在父Logger的appender里输出。
　　　　若是additivity设为false，则子Logger只会在自己的appender里输出，而不会在父Logger的appender里输出。
　　　　子Logger继承父Logger后，level以子logger配置的level为准
　　-->

<logger name="com.me" level="${log.level}" additivity="true">
　　<!-- level表示只记录哪一个级别以上的日志 --> 
　　<!-- 与appender进行绑定 --> 
　　<appender-ref ref="debugAppender"/>
 　 <appender-ref ref="infoAppender"/> 
　　<appender-ref ref="errorAppender"/> 
</logger> 

<root level="info"> 
　　<appender-ref ref="consoleAppender"/>
</root> 

</configuration>
