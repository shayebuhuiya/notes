## SQL数据类型

1. 整数类型：TINYINT(1)、SMALLINT(2)、MEDIUMINT(3)、INT(4)、BIGINT(8)各自占用不同的字节数，表示范围越来越大。

   ![image-20221101154517608](https://gitee.com/absolutelyd/picgo_gitee/raw/master/img/image-20221101154517608.png)

   零填充：zerofill()，在数字位数不够的空间用字符'0'填充。

   自增属性：AUTO_INCREMENT，每行自动增加1，应定义为NOT_NULL,并定义为PRIMARY_KEY键或者是UNIQUE键。

2. 小数表示分为**浮点数**和**定点数**。

   ![image-20221103142326127](https://gitee.com/absolutelyd/picgo_gitee/raw/master/img/image-20221103142326127.png)

   - 浮点数包括float(单精度)和double(双精度)。

     如果插入数的精度超过了该定义类型的实际精度，插入值会被四舍五入再插入，**不会进行警告**。

   - 定点数只有decimal一种类型，适合用来表示货币等精度较高的数据。

     定点数实际上是以字符串形式存放的，如果插入数的精度高于定义的精度，MySQL**会进行警告**。

     decimal如果不写明精度，会按照默认值(10,0)操作。decimal可以按照十进制的方式处理数据，更适合现实中的一些业务。

3. 日期时间类型

   ![image-20221103142425144](https://gitee.com/absolutelyd/picgo_gitee/raw/master/img/image-20221103142425144.png)

   - DATE：表示年月日；

     DATETIME：表示年月日时分秒；

     TIME：表示时分秒；

     YEAR：表示年；

     TIMESTAMP：YYYY-MM-DD HH:MM:SS固定的19个字符串。

     问题一：TIMESTAMP和时区相关，插入日期时先转换为本地时区存放，取出时转换为本地时区显示。

     问题二：TIMESTAMP可以表示的范围并不大，一些情况下需要注意。

   - now()函数插入当前日期，DATETIME类。

   - YYYY-MM-DD HH:MM:SS格式的字符串，任何标点符号都可以充当这个间隔符。

4. 字符串类型

   ![image-20221103142454447](https://gitee.com/absolutelyd/picgo_gitee/raw/master/img/image-20221103142454447.png)

   - CHAR与VARCHAR：

     CHAR长度固定为创建时声明的长度，长度在0-255之间。会删除字符串多余的空格。

     VARCHAR长度可变，长度在0-65535之间，值的长度+1字节。会保留字符的空格。

     CHAR由于是固定长度的，存取速度更快，但是空间利用不充分。

   - TEXT与BLOB：

     保存较大文本使用TEXT或者BLOB。BLOB能用来保存二进制数据，比如照片;TEXT只能保存字符数据。

     BLOB和TEXT会在删除时导致一些空洞碎片,对性能有影响。在删除后**需要使用OPTIMIZE table对空间碎片进行整理。**

   - BINARY与VARBINARY：

     区别与CHAR与VARCHAR类似，但是BINARY只保存一串二进制数据。

   - ENUM：枚举类型，取值的范围在创建表时显式给定，一次只能选择其中的一个值。

   - SET：和ENUM类似也是一个字符串对象，但是一次可以选取多个成员。

