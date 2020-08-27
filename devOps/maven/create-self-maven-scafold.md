# create-self-maven-scafold.md

## Maven archetype
什么是archetype

简单来说maven archetype插件就是创建项目的脚手架,你可以通过命令行或者IDE集成简化项目创建的工作.

我们在使用IDEA其实就看到很多archetype.

由以下模块组成:

    maven-archetype-pluginArchetype插件.通过该插件,开发者可以在Maven中使用Archetype.
    archetype-packaging用于描述archetype的生命周期与构建项目软件包
    archetype-models用于描述类与引用
    archetype-common核心类
    archetype-testing 用于测试Maven Archetype的内部组件

archetype 插件
http://maven.apache.org/archetype/maven-archetype-plugin/index.html

![archetype plugin goal](http://maven.apache.org/archetype/maven-archetype-plugin/images/archetype-overview.png)


archetype:create(不推荐)

从archetype 中创建一个Maven项目.

archetype:generate

从archetype 中创建一个Maven项目,需要开发人员在指定archetype,插件会从远程仓库中自动获取.

archetype:create-from-project

从已有的项目中生成archetype.

archetype:crawl

搜索并更新仓库中的archetype.


##创建archetype

    首先找一个作为模板的项目

git clone https://github.com/allennotes/webserver

    在项目根目录下执行,与主pom.xml的同级目录

mvn archetype:create-from-project 

将生产的target目录移动到我们需要的目录打开gitbash进行如下操作

    删除idea的相关文件

    rm -rf ./idea
    find . -name "*.iml" -type f -print -exec rm -rf {} \; 

    删除不需要的实例代码

    find . -name "UserMain*" -type f -print -exec rm -rf {} \; 

![如图:](https://s2.ax1x.com/2020/03/03/3hS9Wq.png)


## archetype的组成


    prototype files原型文件

        位于src/main/resources/archetype-resource目录下.prototype files 原型文件可以理解为多模块中的子模块或是单模块工程中的源文件.这些原型文件在使用对应archetype生成项目时被生成

    archetype-metadata.xml

        位于src/main/resources/META-INF/maven/目录下.该配置文件中主要列出了原型文件以及使用archetype生成模板工程需要的参数

    prototype pom

        位于src/main/resources/archetype-resources目录下.这个pom文件会出现在archetype创建的模板工程中,如果是单模块工程,则是对整个项目的依赖管理;如果是多模块工程,该pom是总pom文件,该文件中会定义项目的子模块以及对子模块的依赖进行管理等,子模块pom定义在子模块下,子模块pom文件只管理子模块的依赖.

    archetype pom

        位于自定义archetype工程的根目录下.这是archetype工程项目的pom文件, install时从中获取本地参数

## 将archetype安装到本地

    在archetype目录下执行, 注意与pom.xml同级

mvn install 

    执行成功后,执行crawl命令,在本地仓库的根目录生成archetype-catalog.xml骨架配置文件:

    mvn archetype:crawl //更新索引 可不执行

```
<?xml version="1.0" encoding="UTF-8"?>
<archetype-catalog xsi:schemaLocation="http://maven.apache.org/plugins/maven-archetype-plugin/archetype-catalog/1.0.0 http://maven.apache.org/xsd/archetype-catalog-1.0.0.xsd"
    xmlns="http://maven.apache.org/plugins/maven-archetype-plugin/archetype-catalog/1.0.0"
    xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">
  <archetypes>
    <archetype>
      <groupId>com.barm.archetypes</groupId>
      <artifactId>webserver-archetype</artifactId>
      <version>1.0.0</version>
      <description>Demo project for Spring Boot</description>
    </archetype>
  </archetypes>
</archetype-catalog>
```
## IDEA archeType构建项目


## 通过mvn archetype:generate构建项目

从本地archeType模板中创建项目,执行

mvn archetype:generate -DarchetypeCatalog=local

然后依次选择模板序号和groupId,artifactId,version和package信息


建议拿过来就可以用不需要改什么,而且改了也不方便模板项目二次生成脚手架的更新.

脚手架github 源码https://github.com/allennotes/webserver-archetype
