# springboot-config-log.md

*Note: master /slave config yml file handle(include)


## application.yml ->main
---
```
multipart:
  max-file-size: 10Mb
  max-request-size: 15Mb

spring:
  profiles:
    include: log
  application:
    name: test-service


## application-log.yml

```
```#test environment
spring:
  profiles: test

logging:
  config: classpath:config/logback-spring.xml

---
#production environment
spring:
  profiles: prod

logging:
  config: config/logback-spring.xml
```

## logback-spring.xml:

```
	<configuration debug="false">
	    <!--定义日志文件的存储地址 勿在 LogBack 的配置中使用相对路径-->
	    <property name="LOG_HOME" value="/opt/iaas-api/logs" />
	    <!-- 文件输出格式 -->
	    <property name="CONSOLE_LOG_PATTERN" value="[%d{MM/dd HH:mm:ss.SSS}][%-10.10thread][%highlight(%-5level)][%-40.40c{1}:%5line]:[%15method] || %cyan(%m%n)" />
	    <property name="FILE_LOG_PATTERN" value="[%d{MM/dd HH:mm:ss.SSS}][%-10.10thread][%-5level][%-40.40c{1}:%5line]:[%15method] || %m%n" />
	    <property name="FILE" value="app_config" />

	    <!-- 控制台输出 -->
	    <appender name="STDOUT" class="ch.qos.logback.core.ConsoleAppender">
	        <encoder class="ch.qos.logback.classic.encoder.PatternLayoutEncoder">
	            <!--格式化输出：%d表示日期，%thread表示线程名，%-5level：级别从左显示5个字符宽度%msg：日志消息，%n是换行符-->
	            <pattern>${FILE_LOG_PATTERN}</pattern>
	        </encoder>
	    </appender>
	    <!-- 按照每天生成日志文件 -->
	    <appender name="FILE"  class="ch.qos.logback.core.rolling.RollingFileAppender">
	        <rollingPolicy class="ch.qos.logback.core.rolling.SizeAndTimeBasedRollingPolicy">
	            <!--日志文件输出的文件名-->
	            <fileNamePattern>${LOG_HOME}/${FILE}.%d{yyyy-MM-dd}-%i.log.gz</fileNamePattern>
	            <!--日志文件最大的大小-->
	            <maxFileSize>10MB</maxFileSize>
	            <!--日志文件保留天数-->
	            <maxHistory>30</maxHistory>
	        </rollingPolicy>
	        <encoder class="ch.qos.logback.classic.encoder.PatternLayoutEncoder">
	            <!--格式化输出：%d表示日期，%thread表示线程名，%-5level：级别从左显示5个字符宽度%msg：日志消息，%n是换行符-->
	            <pattern>${FILE_LOG_PATTERN}</pattern>
	        </encoder>
	    </appender>

	    <!-- 输出 level为 ERROR 日志 -->
	    <appender name="ERROR_FILE"  class="ch.qos.logback.core.rolling.RollingFileAppender">
	        <rollingPolicy class="ch.qos.logback.core.rolling.TimeBasedRollingPolicy">
	            <!--日志文件输出的文件名-->
	            <FileNamePattern>${LOG_HOME}/${FILE}_error.%d{yyyy-MM-dd}.log</FileNamePattern>
	            <!--日志文件保留天数-->
	            <MaxHistory>30</MaxHistory>
	        </rollingPolicy>
	        <encoder class="ch.qos.logback.classic.encoder.PatternLayoutEncoder">
	            <!--格式化输出：%d表示日期，%thread表示线程名，%-5level：级别从左显示5个字符宽度%msg：日志消息，%n是换行符-->
	            <pattern>${FILE_LOG_PATTERN}</pattern>
	        </encoder>
	        <!--日志文件最大的大小-->
	        <triggeringPolicy class="ch.qos.logback.core.rolling.SizeBasedTriggeringPolicy">
	            <MaxFileSize>10MB</MaxFileSize>
	        </triggeringPolicy>
	        <!-- 此日志文件只记录ERROR级别的 -->
	        <filter class="ch.qos.logback.classic.filter.ThresholdFilter">
	            <level>ERROR</level>
	        </filter>
	    </appender>

	    <!-- 异步输出 -->
	    <appender name ="ASYNC_FILE" class= "ch.qos.logback.classic.AsyncAppender">
	        <!-- 不丢失日志.默认的,如果队列的80%已满,则会丢弃TRACT、DEBUG、INFO级别的日志 -->
	        <discardingThreshold >0</discardingThreshold>
	        <!-- 更改默认的队列的深度,该值会影响性能.默认值为256 -->
	        <queueSize>512</queueSize>
	        <includeCallerData>true</includeCallerData> <!-- Copy caller data to event -->
	        <!-- 添加附加的appender,最多只能添加一个 -->
	        <appender-ref ref ="FILE"/>
	    </appender>

	    <!-- 测试环境 -->
	    <springProfile name="test">
	        <appender name="STDOUT" class="ch.qos.logback.core.ConsoleAppender">
	            <encoder class="ch.qos.logback.classic.encoder.PatternLayoutEncoder">
	                <!--格式化输出：%d表示日期，%thread表示线程名，%-5level：级别从左显示5个字符宽度%msg：日志消息，%n是换行符-->
	                <pattern>${CONSOLE_LOG_PATTERN}</pattern>
	            </encoder>
	        </appender>
	        <root level="INFO">
	            <appender-ref ref="STDOUT" />
	        </root>
	        <logger name="druid.sql.Statement" level="DEBUG" additivity="false">
	            <appender-ref ref="STDOUT" />
	        </logger>
	    </springProfile>

	    <!-- 生产环境 -->
	    <springProfile name="prod">
	        <appender name="STDOUT" class="ch.qos.logback.core.ConsoleAppender">
	            <encoder class="ch.qos.logback.classic.encoder.PatternLayoutEncoder">
	                <!--格式化输出：%d表示日期，%thread表示线程名，%-5level：级别从左显示5个字符宽度%msg：日志消息，%n是换行符-->
	                <pattern>${FILE_LOG_PATTERN}</pattern>
	            </encoder>
	        </appender>
	        <!-- 日志输出级别 -->
	        <root level="INFO">
	            <!--生产环境去掉控制台输出-->
	            <appender-ref ref="STDOUT" />
	            <appender-ref ref="ASYNC_FILE" />
	            <appender-ref ref="ERROR_FILE" />
	        </root>

	    </springProfile>

	</configuration>
