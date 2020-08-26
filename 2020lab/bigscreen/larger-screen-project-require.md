#larger-screen-project-require.md
---

1. 支持直连MySQL、SQL Server、PostgreSQL、Greenplum、Oracle、SAP HANA、Hive、Kylin等数据源，也可上传本地Excel/Csv文件或通过API连接数据

2. 我们提供65+种基于百度ECharts和D3的可视化图表，以及10+种过滤组件，支持专业的GIS可视化效果和自定义图表效果，充分满足您多样化的可视化需求

3. 拖拽式编辑
使用可视化编辑器，所见即所得，拖拽生成可视化页面，让您轻松编辑，不写代码

4. 我们将BI报表和大屏功能融合，提供了多套酷炫的大屏模板，让你快速搭建科技感爆棚的大屏

5. 报表与大屏均支持数据表建模、拖拽字段绑定数据，以及过滤、下钻和联动等灵活交互，让您对数据轻松进行深度分析挖掘

6. 企业组织级别管理，项目空间数据隔离，基于角色和用户的授权机制，充分保证您的数据安全

Organ->projectSpace->Role&User

## Suger-app研究：
------------
添加数据源->
创建数据模型->(build model for pie,line.....)

-报表
制作报表并分析数据->可视化组件{自助}数据分析
数据管理

-展现
制作大屏->组件实现低成本cool Screen
数据管理


重点难点：
SQL查询引擎设计（语法纠错提示）-标准SQL转义处理


##数据管理

-数据源管理->选数据源（json)->数据输入->数据导入->保存数据链接

-数据模型 ->设计关系即：也是转义过程


测试：验证nosql 分析 --o


 The recommended fetch size is 1000-2000.

1. 找exasol 相关实例
How to build your own Virtual Schema Adapter – Part 1？

暂时处理。


2. 找sql转义引擎（sql)










--------------------------------------------------------------
数据绑定联动方式：
API后端获取下钻参数
API后端获取联动参数：
API后端获取URL额外参数

空间数目、报表个数、大屏个数、公开分享的大屏和报表个数不做任何限制。
--------------------------------------------------------------


6Rhqp7sM

use strict ";
module.exports={up:function(queryInterface,Sequelize){return queryInterface.renameColumn("
sugar_groups ","
key ","
group_key ")},down:function(queryInterface,Sequelize){return queryInterface.renameColumn("
sugar_groups ","
group_key ","
key ")}}


创建大屏产生的数据集：


登录记录：

jQV9Awuc


##Sugar-app主要功能

docker run :

docker run --ulimit nofile=65100:65100 --restart unless-stopped -d -p 8000:8580 --name sugar -v ~/dashuo/sugar-log:/sugar-app/log --env-file env sugarbi/sugar:2.3.2


sudo docker run --restart unless-stopped -d -p 9090:9090 --name prometheus -v /home/cnopens/data/prometheus:/data  prom/prometheus --config.file=/data/prometheus.yml


##Dashuo Smart Screen 项目



#Exasol can do what?

In-memory-analytic_database.

Exasol is a clustered RDBMS using a shared nothing architecture. Availability is achieved with spare nodes, like in this diagram:


 1. Vitual Scheme
 2. New Analytical Functions 



Exasol workflow.
---


版本选择



BucketFS

//======================================================
surgar-app:rel technology
1.nodejs
2.y2go
3.darwin (xx)
4.python+supervisord(进程管理工具)
5.redis
6.vertx
7.Athena(distinct query engineer)
8.facebook presto (sql query enginee)
======================================================
``java+springboot

liblz4-java.so --> only for java compression
jpountz->Java ports and bindings of the LZ4 compression algorithm and the xxHash hashing algorithm 

sqlj.runtime ->oracle

mysql :export specify database



-------------------------------------------------------------------
用户在租户内的角色:0:系统管理员 1:管理员  2:普通用户  3:审计用户

租户类型（当做空间）： 0: 系统默认租户;  1: 业务租户（组织）;  2: 私人租户（普通）  (普通、组织) --、demo

//；3：大屏租户

support driver: oracle ,mysql 

接口：

1. 创建大屏用户接口（用户创建去重）默认租户内角色：普通用户；默认租户类型：2

2. 大屏管理： 
   
   -列表
   -批量删除
   -查看大屏页面
   -编辑大屏页面
   -更改页面属性
   -删除

3. 数据管理： 
    
    -数据源管理
    
    -数据模型

    -数据表预览

4.  系统管理
    
    -轮播管理：暂时不要

    -用户管理：用户列表（大屏用户类型）用户新增、编辑、删除

    创建用户：

    -租户空间设置：
    logo,名称，
    访问地址，
    租户类型：0: 系统默认租户 1: 业务租户（团队，组织）;  2:私人租户（创建用户默认，独立的）
    简介

    角色： 
    用户在租户内的角色:0:系统管理员 1:管理员  2:普通用户  3:审计用户

    1. list
    租户相当于组织；

    空间类型：普通（默认）、组织、demo(admin配置报表) 


````
在空间中，空间管理员可以通过用户与角色来管理用户的权限，从而控制用户可以查看空间中哪些报表（大屏目前没有权限管控，空间中的所有用户都可以查看空间中的所有大屏），以确保数据的安全性和保密性。
````






    删除空间后，与当前空间相关的所有数据(报表、大屏、轮播、角色、用户、数据源、SQL模型、用户收藏的内容等)都会删除，并且不可恢复，请谨慎操作！















EXAPooledConnection
-----------------------------------------------------------------------------
1.
    <dependencies>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-data-ldap</artifactId>
        </dependency>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-security</artifactId>
        </dependency>
        <dependency>
            <groupId>org.mybatis.spring.boot</groupId>
            <artifactId>mybatis-spring-boot-starter</artifactId>
            <version>2.1.3</version>
        </dependency>

        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-devtools</artifactId>
            <scope>runtime</scope>
            <optional>true</optional>
        </dependency>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-configuration-processor</artifactId>
            <optional>true</optional>
        </dependency>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-test</artifactId>
            <scope>test</scope>
            <exclusions>
                <exclusion>
                    <groupId>org.junit.vintage</groupId>
                    <artifactId>junit-vintage-engine</artifactId>
                </exclusion>
            </exclusions>
        </dependency>
        <dependency>
            <groupId>org.springframework.security</groupId>
            <artifactId>spring-security-test</artifactId>
            <scope>test</scope>
        </dependency>

    </dependencies>










What ?

research:

vertx package:

import io.vertx.core.json.JsonArray;
import io.vertx.core.json.JsonObject;
import io.vertx.ext.jdbc.impl.actions.JDBCStatementHelper;


Entry
-----------------------------------------------------------------------------
public class Main {
  public static void main(String[] args) {
    String portStr = System.getenv("sugar_javakit_port");
    int portInt = 8110;
    if (portStr != null)
      portInt = Integer.parseInt(portStr); 
    Spark.port(portInt);
    int maxThreads = 100;
    int minThreads = 2;
    int timeOutMillis = 30000;
    Spark.threadPool(maxThreads, minThreads, timeOutMillis);
    Spark.post("/jdbc", (req, res) -> {
          res.header("content-type", "application/json; charset=utf-8");
          JsonObject postData = (JsonObject)Json.decodeValue(req.body());
          Integer timeoutQuery = postData.getInteger("timeout");
          int timeout = 60000;
          if (timeoutQuery != null)
            timeout = timeoutQuery.intValue(); 
          ExecutorService executor = Executors.newSingleThreadExecutor();
          Future<JsonObject> future = executor.submit(new QueryData(postData, timeout));
          JsonObject result = new JsonObject();
          try {
            result = future.get(timeout, TimeUnit.MILLISECONDS);
          } catch (TimeoutException ex) {
            result.put("status", Integer.valueOf(4));
            result.put("msg", "timeout");
          } catch (Exception e) {
            result.put("status", Integer.valueOf(5));
            result.put("msg", e);
          } finally {
            future.cancel(true);
            executor.shutdownNow();
          } 
          return result.encode();
        });
  }
}
-----------------------------------------------------------------------------






====================================================================


project--空间 。。。n

主要功能：
1. 多数据源处理
2. 数据模型处理 left ,right ,full
3. 图表初始化
4. 大屏资源配置：宽，高，背景，


模块：
数据管理

	-空间接口：或者Group->
	-大屏管理接口(数据)
	-数据源接口
	-模型接口
	-数据表管理接口

基础功能
-----------
系统设置：
轮播接口
角色管理接口
用户管理
导入导出
空间设置->当前空间


基础功能：
1. 角色
2. 用户
3. 导入、导出（项目）--这个不一定要

不可能做的：
1. 涉及加密的
2. license
3. 涉及空间管理的


20200818->数据设计->创建项目

	- 初始化图元素数据（json-在项目中加载)


	- 权限数据

	- 组织数据


项目名称：dss

```技术架构
目标： 实时性，api设计，

业务研究方向：
空间-group









````

Nodejs:
- api框架设计
- 代码规范
- 设计模式：mvc(express/sails)
- 全栈(Meteor)
- Rest Api(Loopback)企业级



## Exasol->





##技术架构选型
