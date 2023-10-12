## SQL简介

1. SQL(Structure Query Language,结构化查询语言)

2. SQL语言分类：

   - DDL：Data Definition Languages，数据定义语言，定义了不同的数据段、数据库、表、列、索引等数据库对象。create，drop，alter。
   - DML：Data Manipulation Language，数据操纵语言，添加、删除、更新、查询数据库，检查数据库完整性。insert，delete，update，select。
   - DCL：Data Control Language：数据控制语句，控制不同数据段直接的许可和访问级别。grant，revoke。

3. DDL语言

   - 创建数据库：**CREATE database dbname**

     查询所有的数据库：**show databases**

   - 删除数据库： **DROP database dbname**

   - 创建表：

     ```sql
     create table tablename(
     column_name_1 column_type_1 constraints
     column_name_2 column_type_2 constraints
     ……
     column_name_n column_type_n constraints)
     ```

     column_name是列的名字，column_type是列的数据类型，constraints是约束条件

     **desc tablename** 查看表定义

     **show create table emp** 查看创建表的SQL语句

   - 删除表：**drop table tablename**

4. DML语言

   - 插入记录：**INSERT INTO tablename (field,field2,...fieldn) values(value1,value2,...,valuen)**

     也可以不指定字段名称 **insert into tablename values(value1,value2,...,valuen)**

     没有写的字段可以自动设置为NULL

     还可以一次插入多条记录：

     ```sql
     insert into tablename (field,field2,...fieldn)
     values(value1,value2,...,valuen),
     values(value1,value2,...,valuen),
     ...
     values(value1,value2,...,valuen),
     ```

   - 更新记录：**UPDATE tablename SET field1=value1,field=value2... [WHERE CONDITION]**
   
   - 删除记录：**DELETE FROM tablename [WHERE CONDITION]**
   
   - 查询记录：**SELECT * FROM tablename [WHERE CONDITION]**
   
5. 查询语言技巧：

   - 查询不重复的记录：SELECT * **distinct** FROM tablename

   - 条件查询：SELECT * FROM tablename where **[CONDITION]**

   - 排序：SELECT * FROM tablename [ORDER BY field1 DESC/ASC]

     DESC表示降序，ASC表示升序，如果第一个字段相同，按照下一个字段进行排序。

     不同的列都可以设置不同的排序规则

   - 限制：SELECT ... LIMIT offset_start,row_count

     offset_start表示起始偏移量，row_count表示显示的行数。

     可以只写一个数字，默认表示只显示前N行

   - 聚合：SELECT * fun_name FROM tablename

     - 在emp表中统计总人数：select **count(1)** from emp

     - 统计各个部门人数：select deptno,count(1) from emp **group by** deptn

       统计结果会按照deptn分类显示

     - 既要统计各部门人数，又要统计总人数：select deptno,count(1) from emp **group by** deptn **with rollup**

       with rollup是可选项，显示分类聚合的总结果。

     - 在统计结果后面加条件：elect deptno,count(1) from emp **group by** deptn **having** count(1)>1

       having和where的区别：where是在聚合前就对记录进行过滤，having是对聚合后结果进行过滤，应该尽可能先使用where过滤，提高聚合的效率。

     - 统计函数还有sum()，max()，min()。

   - 表连接：

     - 内连接：选出两张表中互相匹配的记录。
       
       - select col1,col2 from table1,table2 **where table1.id=table2.id**
       
         ![image-20221101152136468](https://gitee.com/absolutelyd/picgo_gitee/raw/master/img/image-20221101152136468.png)
     - 外连接：与内连接主要区别在于也会包含没有匹配成功的记录。
       - 左连接：**包含所有的左边表**中的记录甚至是右边表中**没有和它匹配**的记录。
       
       - 右连接：**包含所有的右边表**中的记录甚至是左边表中没有和它匹配的记录。
       
         ![image-20221101152215345](https://gitee.com/absolutelyd/picgo_gitee/raw/master/img/image-20221101152215345.png)
     
   - 子查询：在select的结果上进行select

     - SELECT * FROM tablename WHERE col IN( SELECT * FROM tablename)
     - 子查询可以转化成为表连接操作，这样做效率更好。

   - 记录联合：将两个查询的结果合并到一起显示出来。

     - SELECT ...  **UNION|UNION ALL**  SELECT...
     - UNION ALL是将两个查询结果直接合并，UNION会对结果进行一次DISTINCT，去除重复记录。

6. DCL语句

   - DCL语句主要用于管理对象权限。

