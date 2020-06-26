#notebook

指定端口启动 java-jar*.jar--server.port=9090



## springboot jar -资源访问
pwsh  /opt/test/dsicloud-rsmgr-1.0-SNAPSHOT.jar!/BOOT-INF/classes!/datacenterlist.ps1 192.168.31.33 administrator@vsphere.local 1qaz@WSX



## springboot 集成作业管理
方案 



##


## spirngboot 资源(resources) 访问问题
/opt/iaas-api/iaas_server-1.0-SNAPSHOT.jar!/BOOT-INF/classes!/datacenterlist.ps1


## springboot restcontroller how to Fetch outputstream ?



## springboot 手动事务处理方式

springboot 开启事务以及手动提交事务

需要在服务类上加上两个注解


@Autowired
DataSourceTransactionManager dataSourceTransactionManager;
@Autowired
TransactionDefinition transactionDefinition;

手动开启事务
TransactionStatus transactionStatus = dataSourceTransactionManager.getTransaction(transactionDefinition);
手动提交事务
dataSourceTransactionManager.commit(transactionStatus);//提交
手动回滚事务
dataSourceTransactionManager.rollback(transactionStatus);//最好是放在catch 里面,防止程序异常而事务一直卡在哪里未提交