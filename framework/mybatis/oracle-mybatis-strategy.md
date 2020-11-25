#oracle-mybatis-strategy.md

oracle mybatis 批量插入和mysql差异

batch insert
---
  <insert id="batchInsert" parameterType="java.util.List" useGeneratedKeys="false">
    insert into DSI_VM_DEPLOY_LOG_SUBTASK(STAGE_ID,SUBJECT,RESULT,STATUS,CREATE_TIME,CONSUME_TIME,ID,UPDATE_TIME)
    VALUES
    <foreach collection="list" item="item" separator="union all"> <!--mysql ,-->
      (#{item.stageId}, #{item.subject},#{item.result},#{item.status},#{item.createTime},#{item.consumeTime},#{item.id},#{item.updateTime})
    </foreach>
  </insert>

batch update
---

 <update id="batchBatch" parameterType="java.util.List">
    update DSI_VM_DEPLOY_LOG_SUBTASK
    <foreach collection="list" item="item" separator="union all">
      select #{item.stageId}, #{item.subject},#{item.result},#{item.status},#{item.createTime},#{item.consumeTime},#{item.id},#{item.updateTime} from dual
    </foreach>
    where STAGE_ID = #{stageId,jdbcType=VARCHAR}
  </update>



---
*Mybatis3全注解开发配置及主键生成策略总结


    import org.mybatis.spring.SqlSessionFactoryBean;
    import org.mybatis.spring.annotation.MapperScan;
    import org.springframework.beans.factory.annotation.Autowired;
    import org.springframework.context.annotation.Bean;
    import org.springframework.context.annotation.ComponentScan;
    import org.springframework.context.annotation.Configuration;
    import org.springframework.jdbc.datasource.DataSourceTransactionManager;
    import org.springframework.transaction.annotation.EnableTransactionManagement;
    import org.springframework.web.servlet.view.InternalResourceViewResolver;
     
    import com.easy.demo.config.property.DbcpProperty;
     
    @Configuration
    @ComponentScan(basePackages = "*")
    @EnableTransactionManagement
    @MapperScan(basePackages = "*.model")
    public class AppConfig {
    	@Autowired
    	private DbcpProperty dbcpProperty;
     
    	@Bean
    	public InternalResourceViewResolver viewResolver() {
    		InternalResourceViewResolver viewResolver = new InternalResourceViewResolver();
    		viewResolver.setPrefix("/views/");
    		viewResolver.setSuffix(".html");
    		return viewResolver;
    	}
     
    	@Bean
    	public BasicDataSource getBasicDataSource() {
    		BasicDataSource dbcp = new BasicDataSource();
    		dbcp.setUsername(dbcpProperty.getUsername());
    		dbcp.setPassword(dbcpProperty.getPassword());
    		dbcp.setDriverClassName(dbcpProperty.getDriverClassName());
    		dbcp.setUrl(dbcpProperty.getUrl());
    		dbcp.setMaxTotal(dbcpProperty.getMaxTotal());
    		dbcp.setMaxIdle(dbcpProperty.getMaxIdle());
    		dbcp.setMinIdle(dbcpProperty.getMinIdle());
    		dbcp.setInitialSize(dbcpProperty.getInitialSize());
    		dbcp.setMaxWaitMillis(dbcpProperty.getMaxWaitMillis());
    		return dbcp;
    	}
     
    	@Bean
    	public SqlSessionFactoryBean getSqlSessionFactoryBean(BasicDataSource basicDataSource) {
    		SqlSessionFactoryBean sqlSessionFactoryBean = new SqlSessionFactoryBean();
    		sqlSessionFactoryBean.setDataSource(basicDataSource);
    		sqlSessionFactoryBean.setTypeAliasesPackage("*.model.entity");
    		return sqlSessionFactoryBean;
     
    	}
     
    	@Bean
    	public DataSourceTransactionManager getDataSourceTransactionManager(BasicDataSource basicDataSource) {
    		DataSourceTransactionManager dataSourceTransactionManager = new DataSourceTransactionManager();
    		dataSourceTransactionManager.setDataSource(basicDataSource);
    		return dataSourceTransactionManager;
    	}
    }

 这里着重说一下oracle的主键配置策略，因为我们说的关系数据库一般有三种，分别是mysql,sqlserver.oracle其中，前面两种都支持自增主键，而oracle则不行，oracle需要通过序列化来实现自增主键，所以我们今天主要来说说oracle的方式，前两种通过@options注解配置即可，而oracle是通过selectKey来获取，他的原理就是在执行我们的插入语句之前先执行一下获取序列化的操作，然后将查询出来的结果赋值给对应的KeyProperty,然后我们在拼接SQL的时候，直接获取对应的KeyProperty就可以获取到主键的值了，还有注意的地方是before，是在Sql之前执行还是之后执行。

    @Mapper
    public interface TestMapper {
     
    	@InsertProvider(type = TestProvider.class, method = "insertTest")
    	@SelectKey(keyProperty = "tid", resultType = int.class, before = true, statement = {
    			"select TEST_SEQ.NEXTVAL  from dual" })
    	public void addTest(TestPO testPO);
     
    }
    public class TestProvider {
     
    	public String insertTest(TestPO testPO) {
    		String insert = "insert into test (id,name,age) values(#{tid},#{name},#{age})";
    		return insert;
    	}
     
    }
    @Mapper
    public interface UserMapper {
    	@SelectProvider(type = UserProvider.class, method = "queryUser")
    	public UserPO queryUser(String loginName);
     
    	@SelectProvider(type = UserProvider.class, method = "queryRoles")
    	public List<String> queryRoles(String loginName);
     
    	@SelectProvider(type = UserProvider.class, method = "queryPermissions")
    	public List<String> queryPermissions(String loginName);
    }
