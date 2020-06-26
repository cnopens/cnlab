#mybatis-plus.md

mybatis-plus id主键生成的坑

由于mybatis-plus会自动插入一个id到实体对象, 不管你封装与否, 所以有时候导致一些意外的情况发生

    默认是生成一个长数字字符串(编码不同可能结尾带有字母)

错误

ested exception is org.apache.ibatis.reflection.ReflectionException: Could not set property 'id' of 'class com.xxx' with value '1110423703487479810' Cause: java.lang.IllegalArgumentException: java.lang.ClassCastException@14041406

大致就是由于自动生成了一个id1110423703487479810, 但是无法放入到integer中
解决方案一
1. 修改id字段类型

将id字段类型改为long, 这样就能保证有足够位数放入生成的id
2. 调整数据库id字段类型

将数据库的id字段的长度(改为20位)
解决方案二

如果想要使用id自增的, 就需要把mybatis-plus这个id生成的功能给关掉
添加注解

在id字段上加上如下注解即可
 @TableId(value = "id",type = IdType.AUTO)

*其他type类型介绍

AUTO : AUTO(0, “数据库ID自增”),
INPUT : INPUT(1, “用户输入ID”),
ID_WORKER : ID_WORKER(2, “全局唯一ID”),
UUID : UUID(3, “全局唯一ID”),
NONE : NONE(4, “该类型为未设置主键类型”),
ID_WORKER_STR : ID_WORKER_STR(5, “字符串全局唯一ID”);
