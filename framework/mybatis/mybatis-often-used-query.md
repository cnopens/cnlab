# mybatis-often-used-query.md
---

## 转译符

　　1、特殊字符转译

　　<　　<　　小于

　　>　　>　　大于

　　&　　&　　与

　　'　　’　　单引号

　　"　　"　　双引号

　　需要注意的是分号是必不可少的。 比如 a > b 我们就写成 a > b

　　(分号需为英文状态下的，应为英文分号会将转译符直接显示为对应的符号，所以本文都是中文下的)

　　2、在mybatis中这种符号将不会解析。
   

## 如常用的sql语句写法

---

　　1、模糊查询

　　user_name like CONCAT("%",#{userName},"%") and

　　2、月份查询

　　输入月份(2019-01)，查找属于这个月份的记录

　　DATE_FORMAT(start_time,'%Y-%m') DATE_FORMAT(#{theMonth},'%Y-%m')

　　and

　　DATE_FORMAT(end_time,'%Y-%m') = ]]>DATE_FORMAT( #{theMonth},'%Y-%m')

　　and

　　2019-01-01 00:00:00 >= '2019-01’不成立。

　　因为数据库数据是一月一号，2019-01-01当然比2019-01大，所以直接查找是找不到数据的，因此需要用DATE_FORMAT函数将时间格式化为2019-01-00 00:00:00 	再去对比。2019-01-00 00:00:00 >= ‘2019-01’ 成立

　　DATE_FORMAT常用的正则表达式(%Y-%m-%d %H:%i:%S)

　　3、时间区间查找

　　查找数据库记录的创建时间在要查找的时间区间内的数据

　　create_time =]]>#{startTime}

　　and

　　create_time #{endTime}

　　如果数据库的时间和输入查询条件的时间精度不一致时也需要如上格式化

　　4、批量添加

　　parameterType="com.safety.exam.entity.MessageReceive"

　　useGeneratedKeys="true" keyProperty="id">

　　insert into

　　user

　　name,

　　age

　　values

　　separator=",">

　　#{item.name},

　　#{item.age},

　　相当于insert into user (name,age)values (张，20),(李，21),(王，22)·····

　　5、批量更新

　　parameterType="com.safety.exam.entity.StaffAccount"

　　useGeneratedKeys="true" keyProperty="id">

　　update user set

　　name =

　　open="case id" close="end">

　　when #{item.id} then #{item.name}

　　，

　　age =无锡人流多少钱 http://www.bhnfkyy.com/

　　open="case id" close="end">

　　when #{item.id} then #{item.age}

　　where id in

　　separator="," open="(" close=")">

　　#{item.id}

　　注意： set关键字只有一个;每个foreach之间有个逗号。

　　最后sql是这样：

　　UPDATE categories SET

　　display_order = CASE id

　　WHEN 1 THEN 3

　　WHEN 2 THEN 4

　　WHEN 3 THEN 5

　　END,

　　title = CASE id

　　WHEN 1 THEN 'New Title 1'

　　WHEN 2 THEN 'New Title 2'

　　WHEN 3 THEN 'New Title 3'

　　END

　　WHERE id IN (1,2,3)

## mybatis pagination

PageHelper.startPage(pageNum, pageSize, orderby);
final List<Ip> list = (List<Ip>) this.ipDao.selectListbyPage(keywords);
return (ApiPage<Ip>) ApiPage.restPage((List) list);