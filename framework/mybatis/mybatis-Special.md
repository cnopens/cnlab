# mybatis-Special.md

## mybatis 方法select 

mybatis 方法select 





## 生产策略
再用mybatis时，在插入数据时，有时会用到他的主键回填功能，即获取数据库插入的主键值并将该值赋给pojo中的某一个主键属性


今天又了解到除此之外mybatis还支持自定义主键功能，如数据库中并没有定义主键自增功能，比如现有如下需求：在插入数据时，如果表中没有记录，则主键为1，否则主键自增2，这时需用到selectkey元素进行处理，具体代码如下

<insert id="insertRole" parameterType="role" useGeneratedKeys="true" keyProperty="id">

<selectKey keyProperty="id" resultType="int" order="BEFORE">

select if(max(id) is null , 1 , max(id) + 2) as newId from t_role

</selectKey>

insert into t_role (id,role_name,note) 

values( #{id},#{roleName},#{note})

</insert>



##自动生成 uuid
---
### mybatis 注解方式插入，主键由uuid函数生成

    /**
     * keyProperty: 表示将select返回值设置到该属性中
     * resultType: 返回类型
     * before: 是否在insert之前执行
     * statement: 自定义子查询
     * @param userBase
     */
    @SelectKey(keyProperty = "userBase.id",resultType = String.class, before = true,
            statement = "select replace(uuid(), '-', '')")
    @Options(keyProperty = "userBase.id", useGeneratedKeys = true)
    @Insert("insert into user_base(id, " +
            "name, " +
            "passwd, " +
            "phone " +
            ") values (#{userBase.id}, " +
            "#{userBase.name}, " +
            "#{userBase.password}, " +
            "#{userBase.phone}" +
            ") "
    )
    public void insertForReg(@Param("userBase")UserBase userBase);

### mybatis获取insert插入之后的id,用bean接收

<insert id="insert" parameterType="com.xxx.entity.Event"
        useGeneratedKeys="true" keyProperty="id" keyColumn="id">
  insert into evt_event (id, create_time, modify_time, 
    create_user_id, modify_user_id, db_status, 
    remark, event_name
    )
  values (#{id,jdbcType=BIGINT}, #{createTime,jdbcType=TIMESTAMP}, #{modifyTime,jdbcType=TIMESTAMP}, 
    #{createUserId,jdbcType=BIGINT}, #{modifyUserId,jdbcType=BIGINT}, #{dbStatus,jdbcType=TINYINT},
    #{remark,jdbcType=VARCHAR}
    )
  <selectKey keyProperty="id" resultType="long" order="AFTER">
    SELECT LAST_INSERT_ID();
  </selectKey>
</insert>

关键在于：获取插入后id，这里是自增id
 <selectKey keyProperty="id" resultType="long" order="AFTER">
   SELECT LAST_INSERT_ID();
 </selectKey>

如果是uuid：可以自己代码先生成或者这里获取
<selectKey keyProperty="id" order="BEFORE"  resultType="java.lang.String">
   select uuid()
</selectKey>
