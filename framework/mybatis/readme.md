# source analysis






# basic knowlogue

### 关于Mybatis的@Param注解 
---

### params type 


## mybatis /Example 参数传递方式
---
    public List<TableA> query(String name) {
        Example example = new Example(TableA.class);
        example.createCriteria()
                .andLike("name", "%"+ name +"%");
        example.setOrderByClause("CREATE_TIME DESC");

        return this.tableAMapper.selectByExample(example);
    }
    ---

    ## Mybatis 全局转义处理SQL
    ---

    <![CDATA[  sql 处理部分 ]]>


    mybatis动态sql：

Mapper.xml文件配置sql如下：

<!-- 根据条件查询用户 -->
<select id="queryUserByWhere" parameterType="user" resultType="user">
    SELECT id, username, birthday, sex, address FROM `user`
    WHERE sex = #{sex} AND username LIKE
    '%${username}%'
</select>
改进1：

<!-- 根据条件查询用户 -->
<select id="queryUserByWhere" parameterType="user" resultType="user">
    SELECT id, username, birthday, sex, address FROM `user`
    WHERE 1=1
    <if test="sex != null and sex != ''">
        AND sex = #{sex}
    </if>
    <if test="username != null and username != ''">
        AND username LIKE
        '%${username}%'
    </if>
</select>
改进2：

<!-- 根据条件查询用户 -->
<select id="queryUserByWhere" parameterType="user" resultType="user">
    SELECT id, username, birthday, sex, address FROM `user`
<!-- where标签可以自动添加where，同时处理sql语句中第一个and关键字 -->
    <where>
        <if test="sex != null">
            AND sex = #{sex}
        </if>
        <if test="username != null and username != ''">
            AND username LIKE
            '%${username}%'
        </if>
    </where>
</select>