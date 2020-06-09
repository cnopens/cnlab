#Network Communication framework

<title>Mina</title>
<p>
Apache Mina Server 是一个网络通信应用框架，为开发高性能和高可用性的网络应用程序提供了非常便利的框架。
特点：异步的NIO框架,将UDP当成"面向连接"的协议
</p>

###一、组件管理
Mina的底层依赖的主要是Java NIO库，上层提供的是基于事件的异步接口
(1)IoService(最底层[起点])
作用：隐藏底层IO的细节，对上提供统一的基于事件的异步IO接口
IOSocketAcceptor和IOSocketChannel，分别对应TCP协议下的服务端和客户端的IOService，low-level IO经过IOService层后变成IO Event
(2)IoProcessor
作用：负责检查是否有数据在通道上读写
IoProcessor 负责调用注册在IoService 上的过滤器，并在过滤器链之后调用IoHandler
(3)IoFilter(拦截器[关键])
作用：日志输出、黑名单过滤、数据的编码（write 方向）与解码（read 方向）等功能
(4)IoHandler(业务逻辑处理[终点])
作用：接收、发送数据
每个IoService都需要指定一个IoHandler
(5)IoSession
作用：是对底层连接（服务器与客户端的特定连接，该连接由服务器地址、端口以及客户端地址、端口来决定）的封装

###二、服务端流程：
1、通过SocketAcceptor 同客户端建立连接；
2、连接建立之后 I/O的读写交给了I/O Processor线程，I/O Processor是多线程的；
3、通过I/O Processor 读取的数据经过IoFilterChain里所有配置的IoFilter，IoFilter进行消息的过滤，格式的转换，在这个层面可以制定一些自定义的协议；
4、最后IoFilter将数据交给 Handler  进行业务处理，完成了整个读取的过程；
写入过程也是类似，只是刚好倒过来，通过IoSession.write 写出数据，然后Handler进行写入的业务处理，处理完成后交给IoFilterChain，进行消息过滤和协议的转换，最后通过 I/O Processor 将数据写出到 socket 通道

###三、服务端代码实现
1、编写IoService（主要功能：设置过滤器，设置handler，绑定端口）

复制代码
public class MinaServer {
    private static Logger logger = Logger.getLogger(MinaServer.class);
    public static void main(String[] args) {
            Configuration config = Configuration.getInstance();
            try {
                config.readConfiguration();
                config.initDataSource();
                config.initServiceDef();

                // 启动FTP读文件，PE积分后台执行
                //此处为内部逻辑,通过FTP读取文件,跳过
            } catch (Exception e1) {
                e1.printStackTrace();
                System.exit(1);
            }
            try {
                config.initInterfaceConfig();
                // 启动接口调用互斥map维护线程
                InterfaceBusThread interfaceBusThread = new InterfaceBusThread();
                interfaceBusThread.start();
            } catch (Exception e) {
                logger.info("InterfaceBus error!", e);
            }

            NioSocketAcceptor acceptor = null; // 创建连接
            try {
                // 创建一个非阻塞的server端的Socket
                acceptor = new NioSocketAcceptor(Runtime.getRuntime()
                        .availableProcessors() + 1);// ioprocesser线程数，一般为cpu数量+1

                // 建立工作线程池
                Executor threadPool = Executors.newFixedThreadPool(Constant.threadCount);// 设置20个handler线程

                // 解决在LINUX服务器上kill掉了，但是端口仍然被占用，要过一段时间才能释放
                acceptor.setReuseAddress(true);

                // 添加消息过滤器
                acceptor.getFilterChain().addLast("codec",
                        new ProtocolCodecFilter(new XMLObjectCodecFactory()));
                //
                // 设置服务器能够接收的最大连接数
                acceptor.setBacklog(Constant.maxSocketConn);
                acceptor.getFilterChain().addLast("executor",
                        new ExecutorFilter(threadPool));
                // 设置读取数据的缓冲区大小
                acceptor.getSessionConfig().setReadBufferSize(2048);
                // 读写通道10秒内无操作进入空闲状态
                acceptor.getSessionConfig().setIdleTime(IdleStatus.BOTH_IDLE, 10);
                // 绑定逻辑处理器
                acceptor.setHandler(new MinaServerHandler()); // 添加业务处理
                // 绑定端口
                acceptor.bind(new InetSocketAddress(Constant.minaServerPort));
                logger.info("server start success... port："
                        + Constant.minaServerPort);

            } catch (Exception e) {
                logger.error("server start error....", e);
                e.printStackTrace();
                System.exit(1);
            }
    }
}

2、编写过滤器(即上面代码的添加消息过滤器，这里使用了Mina自带的换行符编解码器工厂，注意：要在acceptor.bind()方法之前执行)

3、编写IoHandler

public class MinaServerHandler extends IoHandlerAdapter {
    private final static Logger log = LoggerFactory.getLogger(TCPServerHandler.class);

    @Override
    public void messageReceived(IoSession session, Object message) throws Exception {
        //业务代码在这里编写处理
        String str = message.toString();
        System.out.println("The message received is [" + str + "]");
        if (str.endsWith("quit")) {
            session.close(true);
            return;
        }
    }

    @Override
    public void sessionCreated(IoSession session) throws Exception {
        System.out.println("server session created");
        super.sessionCreated(session);
    }

    @Override
    public void sessionOpened(IoSession session) throws Exception {
        System.out.println("server session Opened");
        super.sessionOpened(session);
    }

    @Override
    public void sessionClosed(IoSession session) throws Exception {
        System.out.println("server session Closed");
        super.sessionClosed(session);
    }
    
}

4、将IoHandler 注册到IoService(即上面代码的绑定逻辑处理器，注意：要在acceptor.bind()方法之前执行）

5、运行MinaServer中的main 方法，可以看到控制台一直处于阻塞状态，等待客户端连接

###四、Maven的pom.xml配置

<!-- MINA集成 -->
<dependency>
    <groupId>org.apache.mina</groupId>
    <artifactId>mina-core</artifactId>
    <version>2.0.7</version>
</dependency>
<dependency>
    <groupId>org.apache.mina</groupId>
    <artifactId>mina-integration-spring</artifactId>
    <version>1.1.7</version>
</dependency>

###五、测试类(模拟客户端请求)

public class AutoTest {
    private final static String IP = "localhost";
    private final static int PORT = 9002;
    public static void main(String[] args) {
        String msg = "<root><check>testEquityInsertRev</check><method>equityinsertrev</method><param><usernumber>13632599010</usernumber><equityid>1</equityid><statedate>20130620</statedate></param></root>";
        System.out.println("testEquityInsertRev request msg:"+msg);
        System.out.println("testEquityInsertRev response msg:"+testApi(msg));
}