# gradle-install.md
---

## download 
http://gradle.org/install/#manually

## install 
unzip /jdk/

## configure

1. 配置环境变量
	1) GRADLE_HOME 
	~/severs/gradle5.x
	2) GRADLE_USER_HOME
	~/m2_repository
	3) PATH
	%GRADLE_HOME%/bin;$GRADLE_HOME/bin
2. 配置GRADLE仓库源
在Gradle安装目录下的 init.d 文件夹下，新建一个 init.gradle 文件，里面填写以下配置。
---
	allprojects {
	    repositories {
	        maven { url 'file:///C:/Java/maven_repository'}
	        mavenLocal()
	        maven { name "Alibaba" ; url "https://maven.aliyun.com/repository/public" }
	        maven { name "Bstek" ; url "http://nexus.bsdn.org/content/groups/public/" }
	        mavenCentral()
	    }

	    buildscript { 
	        repositories { 
	            maven { name "Alibaba" ; url 'https://maven.aliyun.com/repository/public' }
	            maven { name "Bstek" ; url 'http://nexus.bsdn.org/content/groups/public/' }
	            maven { name "M2" ; url 'https://plugins.gradle.org/m2/' }
	        }
	    }
	}

repositories 中写的是获取 jar 包的顺序。先是本地的 Maven 仓库路径；接着的 mavenLocal() 是获取 Maven 本地仓库的路径，应该是和第一条一样，但是不冲突；第三条和第四条是从国内和国外的网络上仓库获取；最后的 mavenCentral() 是从Apache提供的中央仓库获取 jar 包。
