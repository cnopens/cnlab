# bigscreen-sql-function.md

---
SUM
用法：SUM(x)
说明：累加求和

COUNT
用法：COUNT(x)
说明：计数

COUNT DISTINCT
用法：COUNT(DISTINCT x)
说明：去重计数

AVG
用法：AVG(x)
说明：均值

MAX
用法：MAX(x)
说明：最大值

MIN
用法：MIN(x)
说明：最小值

IF
用法：IF(expr,v1,v2)
说明：如果表达式expr成立，返回结果v1；否则，返回结果v2

CASE WHEN
用法：CASE WHEN e1 THEN v1 WHEN e2 THEN e2 ... ELSE vn END
说明：CASE表示函数开始，END表示函数结束。如果e1成立，则返回v1,如果e2成立，则返回v2，当全部不成立则返回vn，而当有一个成立之后，后面的就不执行

CEIL
用法：CEIL(x)
说明：返回大于或等于x的最小整数

FLOOR
用法：FLOOR(x)
说明：返回小于或等于x的最大整数

ROUND
用法：ROUND(x, y)
说明：保留x小数点后y位的值，进行了四舍五入

TRUNCATE
用法：TRUNCATE(x, y)
说明：截断x小数，只保留y位，比如0的话就直接去掉小数部分

CONCAT
用法：CONCAT(s1,s2,...)
说明：将字符串s1,s2等多个字符串合并为一个字符串

REPLACE
用法：REPLACE(s,s1,s2)
说明：将字符串s2替代字符串s中的字符串s1

SUBSTRING
用法：SUBSTRING(s,n,len)
说明：获取从字符串s中的第n个位置开始长度为len的字符串

UNIX_TIMESTAMP
用法：UNIX_TIMESTAMP(d)
说明：将时间d以UNIX时间戳的形式返回

FROM_UNIXTIME
用法：FROM_UNIXTIME(d)
说明：将UNIX时间戳的时间转换为普通格式的时间

DATEDIFF
用法：DATEDIFF(d1,d2)
说明：计算日期d1至d2之间相隔的天数

ADDDATE
用法：ADDDATE(d,n)
说明：计算起始日期d加上n天的日期

DATE_FORMAT
用法：DATE_FORMAT(d,f)
说明：按表达式f的要求显示日期d，如：SELECT DATE_FORMAT(NOW(),'%Y-%m-%d')