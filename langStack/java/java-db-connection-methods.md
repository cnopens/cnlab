#java-db-connection-methods.md

---
##数据库三种连接

PooledConnection,XAConnection,Connection

现在来分别说说这三种DataSource:

### public interface DataSource

该工厂用于提供到此 DataSource 对象表示的物理数据源的连接。作为 DriverManager 设施的替代项，DataSource 对象是获取连接的首选方法。实现 DataSource 接口的对象通常在基于 JavaTM Naming and Directory Interface (JNDI) API 的命名服务中注册。

DataSource 接口由驱动程序供应商实现。共有三种类型的实现：

    基本实现 - 生成标准 Connection 对象
    连接池实现 - 生成自动参与连接池的 Connection 对象。此实现与中间层连接池管理器一起使用。
    分布式事务实现 - 生成一个 Connection 对象，该对象可用于分布式事务，并且几乎始终参与连接池。此实现与中间层事务管理器一起使用，并且几乎始终与连接池管理器一起使用。 

DataSource 对象的属性在需要时可以修改。例如，如果将数据源移动到另一个服务器，则可更改与服务器相关的属性。其优点是，因为可以更改数据源的属性，所以任何访问该数据源的代码都无需更改。

通过 DataSource 对象访问的驱动程序不会向 DriverManager 注册。通过查找操作检索 DataSource 对象，然后使用该对象创建 Connection 对象。使用基本的实现，通过 DataSource 对象获取的连接与通过 DriverManager 设施获取的连接相同。


### public interface ConnectionPoolDataSource

PooledConnection 对象的工厂。实现此接口的对象通常在基于 JavaTM Naming and Directory Interface (JNDI) 的命名服务中注册。 


### public interface XADataSource

在内部使用的 XAConnection 对象的工厂。实现 XADataSource 接口的对象通常在使用 Java Naming and Directory InterfaceTM (JNDI) 的命令服务中注册。


三种连接：

与特定数据库的连接（会话）。在连接上下文中执行 SQL 语句并返回结果。

Connection 对象的数据库能够提供信息描述其表、所支持的 SQL 语法、存储过程和此连接的功能等。此信息是使用 getMetaData 方法获得的。

注：默认情况下，Connection 对象处于自动提交模式下，这意味着它在执行每个语句后都会自动提交更改。如果禁用自动提交模式，为了提交更改，必须显式调用 commit 方法；否则无法保存数据库更改。

使用 JDBC 2.1 核心 API 创建的 Connection 对象有一个与之关联的最初为空的类型映射表。用户可以为此类型映射表中的 UDT 输入一个自定义映射关系。在使用 ResultSet.getObject 方法从数据源中检索 UDT 时，getObject 方法将检查该连接的类型映射表，以查看是否有对应该 UDT 的项。如果有，那么 getObject 方法会将该 UDT 映射到所指示的类。如果没有项，则会使用标准映射关系映射该 UDT。

用户可以创建一个新的类型映射表，该映射表是一个 java.util.Map 对象，可在其中创建一个项，并将该项传递给可以执行自定义映射关系的 java.sql 方法。在这种情况下，该方法将使用给定的类型映射表，而不是与连接相关联的映射表。



public interface PooledConnection

为连接池管理提供挂钩的对象。PooledConnection 对象表示到数据源的物理连接。该连接在应用程序使用完后可以回收而不用关闭，从而减少了需要建立连接的次数。

应用程序员不直接使用 PooledConnection 接口，而是通过一个管理连接池的中间层基础设施使用。

当应用程序调用 DataSource.getConnection 方法时，它取回 Connection 对象。如果连接池已完成，则该 Connection 对象实际上是到 PooledConnection 对象的句柄，这是一个物理连接。

连接池管理器（通常为应用程序服务器）维护 PooledConnection 对象的池。如果在池中存在可用的 PooledConnection 对象，则连接池管理器返回作为到该物理连接的句柄的 Connection 对象。如果不存在可用的 PooledConnection 对象，则连接池管理器调用 PooledConnection 方法 getConnection 创建新的物理连接并返回一个到它的句柄。

当应用程序关闭连接时，它调用 Connection 方法 close。完成连接池时，连接池管理器将得到通知；因为它曾使用 ConnectionPool 方法 addConnectionEventListener 作为 ConnectionEventListener 对象注册它自身。连接池管理器释放到 PooledConnection 对象的句柄，并将 PooledConnection 对象返回到连接池，以便再次使用。因此，当应用程序关闭其连接时，基础物理连接会被回收而不是被关闭。

在连接池管理器调用 PooledConnection 方法 close 之前，物理连接不会被关闭。调用此方法通常是为了按顺序关闭服务器，也可能在发生了严重错误，导致连接变得不可用时调用。


public interface XAConnection

extends PooledConnection

为分布式事务提供支持的对象。可以通过 XAResource 对象在分布式事务中利用 XAConnection 对象。事务管理器（通常为中间层服务器的一部分）通过 XAResource 对象管理 XAConnection 对象。

应用程序员不直接使用此接口，而是通过运行于中间层服务器的事务管理器使用。 

