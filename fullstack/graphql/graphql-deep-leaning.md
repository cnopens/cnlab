# graphql-deep-leaning.md

----

## Knnow it
在项目中使用GraphQL有两种方式，一种是API方式，一种是资源文件方式。


### lab ->demo
<dependency>
	<groupId>com.graphql-java</groupId>
	<artifactId>graph-spring-boot-starter</artifactId>
	<version>5.0.2</version>
</dependency>
<dependency>
	<groupId>com.graphql-java</groupId>
	<artifactId>graphql-java-tools</artifactId>
	<version>5.2.4</version>
</dependency>
<dependency>
	<groupId>com.graphql-java</groupId>
	<artifactId>graphiql-spring-boot-starter</artifactId>
	<version>5.2.4</version>
</dependency>


*code:

https://blog.csdn.net/peter7_zhang/article/details/105648979












我个人喜欢使用第二种，首先遵循我个人软件设计方法论中的“分离原则”，符合“分离原则”中的“配置与运行分离”。

Demo如下：

想在原有Controller上使用GraphQL，也比较简单，只需要继承GraphQLQueryResolver接口就可以了：

通过GraphQL解决原有Rest API升级不友好，维护不友好的问题之后。其实我们整个API在管理上还存在另一个挑战，就是API泛滥。

API泛滥之后，我们就很难知道哪些接口提供了哪些能力，甚至是哪些字段。常见的管理方式是类似于wiki的管理，但是wiki在哪？怎么找到又是另一个问题。

所以API文档管理不仅仅是“写死”在文档上，而是如何让API“动”起来。

什么意思呢？就是说一个API落地之后，是否可以按照后续迭代的过程不断去进行相应的API维护，让整个生命周期“动”起来。

关于如何处理这个“动”，其实有两种方案，一种是靠人，一种是靠工具。

对于我这种信奉工具与自动化的人来说，肯定是靠工具了。

那么如何通过工具将API接口“动”起来呢？

方式就有很多了，比如编译期，部署期，流量期等不同生命周期进行控制。

其实再往后想一想，客户端哪个View联动哪个API这个应该怎么搞呢？

当然是主动下发+被动上报了，这样我们可以将API管理进行流量闭环，从而完成对于API的生命周期管理，以及实现API“动”起来的目标了。
