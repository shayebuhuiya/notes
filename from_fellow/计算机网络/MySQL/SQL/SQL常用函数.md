## SQL常用函数

1. 字符串函数：
   - CONCAT(s1,s2,...,sn)：把传入的参数连接成一个字符串。
   - INSERT(str,x,y,instr)：将字符串str从第x位置开始，y个字符长的字串替换为instr。
   - LOWER(str)、UPPER(str)：将字符串换成小写或大写。
   - LEFT(str,x)、RIGHT(str,x)：返回最左边的x个字符，最右边的x个字符。
   - LPAD(str,n,pad)、RPAD(str,n,pad)：用pad对str最左边和最右边填充。
   - LTRIM(str)和RTRIM(str)：去掉字符串str左侧和右侧空格。
   - REPEAT(str,x)：返回str重复x次的结果。
   - REPLACE(str,a,b)：用字符串b替换字符串str中所有出现的字符串a。
   - STRCMP(s1,s2)：比较字符串s1和s2的ASCLL码的大小。
   - TRIM(str)：去掉开头和结尾的空格。
   - SUBSTRING(str,x,y)返回字符串str中第x位置起y长度的字符串。
2. 数值函数：
   - ABS(x)：返回x的绝对值。
   - CELL(x)：返回大于x的最小整数值。
   - FLOOR(x)：返回小于x的最大整数值。
   - MOD(x,y)：返回x/y的模。
   - ROUND(x,y)：返回参数x的四舍五入的y位小数的值。
   - TRUNCATE(x,y)：返回数字x截断y位小数的结果。
3. 日期和时间函数：
   - CURDATE()：返回当前日期。
   - CURTIME()：返回当前时间。
   - NOW()：返回当前的日期和时间。
   - UNIX_TIMESTAMP：返回日期date的UNIX时间戳。
   - FROM_UNIXTIME：返回UNIX时间戳的日期值。
   - WEEK(date)：返回日期date为一天中的第几周。
   - YEAR(date)：返回日期date的年份。
   - HOUR(time)：返回time的小时值。
   - MINUTE(time)：返回time的分钟值。
   - MONTHNAME(date)：返回date的月份名。
   - DATE_FORMAT(date,fmt)：返回按字符串fmt格式化日期date值。
   - DATE_ADD(date,INTERRVAL expr type)：返回一个加上时间间隔的时间值。
   - DATEDIFF(expr,expr2)：返回起始时间expr和结束时间expr2之间的天数。

