# gradle-basic.md

idea :设置堆内存 

-Xmx1024m/2018m

---

## gradle tips

	1) version 
	5.6+ -> 6.5.1 +

	脚本结构发生变化
	

	2) knoweluge




## gradle init project
   
   gradle init

## configure 
key file: gradle-wrapper.properties
方法1：
distributionBase=GRADLE_USER_HOME
distributionPath=wrapper/dists
zipStoreBase=GRADLE_USER_HOME
zipStorePath=wrapper/dists
#distributionUrl=https\://services.gradle.org/distributions/gradle-6.2.2-all.zip
distributionUrl=file\:////home/cnopens/servers/gradle-5.6-all.zip

方法2：
启用idea的离线Gradle


## import idea
通过以上项目初始化，即可以导入


## gradle如何启动

build.gradle配置
---
dependencies {
    def tomcatVersion = '7.0.59'
    tomcat "org.apache.tomcat.embed:tomcat-embed-core:${tomcatVersion}",
            "org.apache.tomcat.embed:tomcat-embed-logging-juli:${tomcatVersion}",
            "org.apache.tomcat.embed:tomcat-embed-jasper:${tomcatVersion}"
}

## gradle plugins id 更换下载源
### 添加springboot
plugins {
  id 'org.springframework.boot' version '2.1.0.RELEASE'
}
### apply plugin
buildscript {
	ext {
		springBootVersion = '2.1.0.RELEASE'
	}
	repositories {
		mavenCentral()
	}
	dependencies {
		classpath("org.springframework.boot:spring-boot-gradle-plugin:${springBootVersion}")
	}
}
apply plugin: 'org.springframework.boot'
