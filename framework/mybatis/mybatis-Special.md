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