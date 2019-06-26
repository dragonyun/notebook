## SQL语法学习

### 1 SQL 简介

- SQL是用于访问和处理数据库的标准的计算机语言
- SQL，结构化查询语言，全称是Structured Query Language
- SQL，用于访问和处理数据库
- SQL，是一种ANSI标准的计算机语言

---

### 2 SQL语法基本概念

- 数据库上执行的大部分工作都是由SQL语句完成

- SQL对大小写不敏感：SELECT和select是相同的

- SQL语句末尾加分号？标准用法需要加分号，很多数据库不加分号不影响使用，建议最好加上。

- 列举一些常见的SQL命令

- ```sql
  SELECT - 从数据库中提取数据
  UPDATE - 更新数据库中的数据
  DELETE - 从数据库中删除数据
  INSERT INTO - 向数据库中插入新数据
  CREATE DATABASE - 创建新数据库
  ALTER DATABASE - 修改数据库
  CREATE TABLE - 创建新表
  ALTER TABLE - 变更（改变）数据库表
  DROP TABLE - 删除表
  CREATE INDEX - 创建索引（搜索键）
  DROP INDEX - 删除索引
  ```

----

### 3 基本语法

#### 3.1 SELECT

- 功能：从数据库中选取数据

- 返回：结果存储在一个结果表中，称为结果集

- ```sql
  语法：
  SELECT column_name,column_name
  FROM table_name;
  与
  SELECT * FROM table_name;
  ```

  ```sql
  语法解释：
  1.SELECT name,country FROM Websites;
  从表Websites中选取name和country两列
  2.SELECT * FROM Websites;
  从表Websites中选取所有列
  ```

- 说明：自己跑一下代码，最清楚。

#### 3.2 SELECT DISTINCT

- 功能：用于返回唯一不同的值

- 说明：同一列中可能包含多个重复值，只列出不同的值

- ```sql
  语法：
  SELECT DISTINCT column_name,column_name
  FROM table_name;
  ```

  ```sql
  语法解释：
  1.SELECT DISTINCT country FROM Websites;
  从表Websites的country列中选取唯一不同的列
  2.SELECT DISTINCT country,id FROM Websites;
  若选择多列，优先保证不同列多的那一列
  ```

- 说明：待定

#### 3.3 WHERE

- 功能：过滤记录

- 返回：能满足指定标准的记录

- ```sql
  语法：
  SELECT column_name,column_name
  FROM table_name
  WHERE column_name operator value;
  ```

- ```sql
  语法解释：
  1.SELECT * FROM Websites WHERE country='CN';
  从表Websites中选取country为CN的所有记录
  比较运算符包括：= > < >=,<=,!=,<>(不等于)
  2.Select * from emp where sal > 2000 and sal < 3000;
  3.Select * from emp where sal > 2000 or comm > 500;
  4.select * from emp where not sal > 1500;
  逻辑运算and、or、not
  优先级为()  not   and   or
  5.Select * from emp where comm is null;
  空值判断：is null
  6.Select * from emp where sal between 1500 and 3000;
  在 之间的值：between and，包括上下限
  7.Select * from emp where sal in (5000,3000,1500);
  取sal等于5000,3000,1500的值
  8.Select * from emp where ename like 'M%';
  模糊查找
  查询 EMP 表中 Ename 列中有 M 的值，M 为要查询内容中的模糊信息。
  	% 表示多个字值，_ 下划线表示一个字符；
   	M% : 为能配符，正则表达式，表示的意思为模糊查询信息为 M 开头的。
   	%M% : 表示查询包含M的所有内容。
   	%M_ : 表示查询以M在倒数第二位的所有内容。
  9.SELECT studentNO FROM student WHERE 0
  10.SELECT studentNO FROM student WHERE 1
  隐式转换
  0为false，返回空集，每一行记录where都返回false
  1为true，返回所有studentNO的值，每一行记录where都返回true
  ```

- 说明：文本用单引号环绕，大部分支持双引号，推荐单引号

- 运算符

  | 运算符  | 描述                 |
  | ------- | -------------------- |
  | =       | 等于                 |
  | <>      | 不等于，部分版本同!= |
  | >       | 大于                 |
  | <       | 小于                 |
  | >=      | 大于等于             |
  | <=      | 小于等于             |
  | BETWEEN | 在某个范围内         |
  | LIKE    | 模糊匹配             |
  | IN      | 指定多个可能值       |

#### 3.4 AND & OR

- 功能：通过逻辑运算符对记录进行过滤

- 返回：满足逻辑运算符的过滤结果

- ```sql
  语法：
  1.SELECT column_name,column_name FROM table_name
  WHERE column_name1 = value
  AND column_name2 > value;
  2.SELECT column_name,column_name FROM table_name
  WHERE column_name1 = value
  OR column_name1 = value;
  ```

- ```sql
  语法解释
  1.SELECT * FROM Websites WHERE country='CN' AND alexa > 50;
  从表Websites中选取country为CN，alexa大于50的所有记录
  2.SELECT * FROM Websites WHERE country='USA' OR country='CN';
  从表Websites中选取country为USA或者CN的所有记录
  3.select * from emp where not sal > 1500;
  取反
  4.SELECT * FROM Websites WHERE alexa > 15 AND (country='CN' OR country='USA');
  混合使用
  ```

- 说明：逻辑运算混合使用优先级为()  not   and   or

#### 3.5 OREDR BY

- 功能：对结果集进行排序

- 返回：对结果集安装一个列或者多个列进行排序，升序

- ```sql
  语法：
  SELECT column_name,column_name
  FROM table_name
  ORDER BY column_name,column_name ASC|DESC;
  ```

- ```sql
  语法解释：
  1.SELECT * FROM Websites ORDER BY alexa;
  从表Websites中选取所有记录，按照alexa列排序（默认升序）
  2.SELECT * FROM Websites ORDER BY alexa DESC;
  从表Websites中选取所有记录，按照alexa列排序（降序）
  3.SELECT * FROM Websites ORDER BY country,alexa;
  从表Websites中选取所有记录，按照country，alexa两列排序，先排前者（country）
  4.SELECT * FROM Websites ORDER BY country DESC,alexa;
  coutry降序，alexa升序
  ```

- 说明：列后方追加的ASC或DESC只对它紧跟着的第一个列名有效，其它不受影响，仍然是默认的升序。

#### 3.6 INSERT INTO

- 功能：插入新记录

- 返回：插入之后的完整表

- ```sql
  语法：
  1.INSERT INTO table_name VALUES (value1,value2,value3,...);
  2.INSERT INTO table_name (column1,column2,column3,...) VALUES (value1,value2,value3,...);
  ```

- ```sql
  语法解释：
  1.INSERT INTO Websites (name, url, alexa, country) VALUES ('百度','https://www.baidu.com/','4','CN');
  向Websites插入新行，指定列插入对应的数据
  2.INSERT INTO Websites (name, url, country) VALUES ('stackoverflow', 'http://stackoverflow.com/', 'IND');
  向Websites插入新行，只在name，url，country插入数据，其它为默认值
  ```

- 说明：增删改查，插入是一个很常用的操作

#### 3.7 UPDATE

- 功能：更新表中已存在的记录

- 返回：更新之后的完整表

- ```sql
  语法：
  UPDATE table_name SET column1=value1,column2=value2,... 
  WHERE some_column=some_value;
  ```

- ```sql
  语法解释：
  1.UPDATE Websites SET alexa='5000', country='USA' WHERE name='菜鸟教程';
  将name为菜鸟教程的记录中的alexa改为5000，country改为USA
  ```

- <font color=red >**警告**</font>：更新时不可忽略`WHRER`子句，否则<font color=red size=5>**所有数据**</font>都会更新。

#### 3.8 DELETE

- 功能：删除表中的行

- 返回：删除之后的完整表

- ```sql
  语法：
  DELETE FROM table_name
  WHERE some_column=some_value;
  ```

- ```sql
  语法解释：
  1.DELETE FROM Websites WHERE name='百度' AND country='CN';
  从表Websites中删除name为百度，country为CN的记录
  2.DELETE FROM table_name;
  3.DELETE * FROM table_name;
  在不删除表的情况下，删除表中所有的行。表结构、属性、索引保持不变
  ```

- <font color=red >**警告**</font>：删除时不可忽略`WHRER`子句，否则<font color=red size=5>**所有数据**</font>都会删除。

---

### 4 进阶语法

#### 4.1 SELECT TOP

- 功能：指定返回记录的数目

- 返回：返回指定数量的记录

- 补充：不是所有数据库系统都支持该语法

- ```sql
  SQL Server/MS Access语法：
  SELECT TOP number|percent column_name(s) FROM table_name;
  MySQL语法：
  SELECT column_name(s) FROM table_name LIMIT number;
  Oracle语法：
  SELECT column_name(s) FROM table_name WHERE ROWNUM <= number;
  ```

- ```sql
  语法解释：
  1.SELECT * FROM Persons LIMIT 5;
  （MySQL）从表Persons中取前两条记录
  2.SELECT TOP 50 PERCENT * FROM Websites;
  （Microsoft SQL Server）从表Websites中取前百分之50的记录
  3.select * from Websites order by id desc limit 5；
  （MySQL）从表Websites中根据字段id降序取后5条记录
  ```

- 说明：该语法不通用

#### 4.2 LIKE

- 功能：在WHERE子句中模糊搜索

- 返回：符合搜索条件的记录

- ```sql
  语法：
  SELECT column_name(s) FROM table_name WHERE column_name LIKE pattern;
  ```

- ```sql
  语法解释：
  1.Select * from emp where ename like 'M%';
  模糊查找
  查询 EMP 表中 Ename 列中有 M 的值，M 为要查询内容中的模糊信息。
  	% 表示多个字值，_ 下划线表示一个字符；
   	M% : 为能配符，正则表达式，表示的意思为模糊查询信息为 M 开头的。
   	%M% : 表示查询包含M的所有内容。
   	%M_ : 表示查询以M在倒数第二位的所有内容。
  ```

- 说明：LIKE与WHERE同时使用

#### 4.3 通配符

- 功能：用于替代字符串中的任何其他字符串

- 说明：与LIKE操作符同时使用

  | 通配符                       | 描述                       |
  | ---------------------------- | -------------------------- |
  | %                            | 替代 0 个或多个字符        |
  | _                            | 替代一个字符               |
  | [*charlist*]                 | 字符列中的任何单一字符     |
  | [^*charlist*]或
[!*charlist*] | 不在字符列中的任何单一字符 |

- ```sql
  语法示例：
  1.SELECT * FROM Websites WHERE name REGEXP '^[GFs]';
  从表Websites中取name以G、F、s开头的记录
  结合REGEXP或NOT REGEXP运算符(或RLIKE和NOT RLIKE)来操作正则表达式。
  ```

- 说明：。。。

#### 4.4 IN

- 功能：在WHERE子句中规定多个值

- ```sql
  语法：
  SELECT column_name(s) FROM table_name WHERE column_name IN (value1,value2,...);
  ```

- ```sql
  语法解释：
  1.SELECT * FROM Websites WHERE name IN ('Google','菜鸟教程');
  从表Websites中取name为Google或菜鸟教程的所有记录
  把=的单个值筛选变成了  （）多个值筛选
  ```

- 说明：=为1个，IN为多个

#### 4.5 BETWEEN

- 功能：范围取值

- 返回：介于两个值之间的数据范围内的值（数值、文本、日期）

- ```sql
  语法：
  SELECT column_name(s) FROM table_name WHERE column_name BETWEEN value1 AND value2;
  ```

- ```sql
  语法解释：
  1.SELECT * FROM Websites WHERE alexa BETWEEN 1 AND 20;
  从表Websites中取alexa介于1和20之间的值（包含上下限值）
  2.SELECT * FROM Websites WHERE alexa NOT BETWEEN 1 AND 20;
  从表Websites中取alexa该范围之外的值（不包含上下限值）
  3.SELECT * FROM Websites WHERE (alexa BETWEEN 1 AND 20)
  AND country NOT IN ('USA', 'IND');
  从表Websites在取alexa介于1和20之间但是country不为USA和IND的所有记录
  语法混合使用
  4.SELECT * FROM Websites WHERE name BETWEEN 'A' AND 'H';
  文本上下限操作
  从表Websites中取name介于A和H之间字母开始的所有记录
  5.SELECT * FROM access_log WHERE date BETWEEN '2016-05-10' AND '2016-05-14';
  时间上下限操作
  ```

- 说明：<font color=red>上下限值是否包含取决于数据库</font>，MySQL包含上下限值

#### 4.6 别名

- 功能：为表名称或者列名称指定别名，可读性更强，简介

- ```sql
  语法：
  1.SELECT column_name AS alias_name FROM table_name;
  列的 SQL 别名语法
  2.SELECT column_name(s) FROM table_name AS alias_name;
  表的 SQL 别名语法
  ```

- ```sql
  语法解释：
  1.SELECT name, CONCAT(url, ', ', alexa, ', ', country) AS site_info FROM Websites;
  把三个列进行合并，看到的结果就是三列合并为一列
  2.SELECT w.name, w.url, a.count, a.date 
  FROM Websites AS w, access_log AS a 
  WHERE a.site_id=w.id and w.name="菜鸟教程";
  这才是很常用的做法，对表指定别名，方便组合语法，更加简介
  ```

- 说明：在以下情况下，使用别名很有用

- - 在查询中涉及超过一个表
  - 在查询中使用了函数
  - 列名称很长或者可读性差
  - 需要把两个列或者多个列结合在一起

#### 4.7 连接（JOIN）

- 功能：把多个表的行结合起来

- 有LEFT JOIN、RIGHT JOIN、INNER JOIN、FULL JOIN、OUTER JOIN等7种用法

  ![1558702821494](E:\文件\Film\lingyundata\数据库相关\IMG\JOIN.png)

- | 语法       | 说明                                     |
  | ---------- | ---------------------------------------- |
  | INNER JOIN | 如果表中有至少一个匹配，则返回行         |
  | LEFT JOIN  | 即使右表中没有匹配，也从左表返回所有的行 |
  | RIGHT JOIN | 即使左表中没有匹配，也从右表返回所有的行 |
  | FULL JOIN  | 只要其中一个表中存在匹配，则返回行       |

- 说明：JOIN可以理解为多个表的列拼接，不同的用法对应不同的场景

#### 4.8 INNER JOIN

- 功能：在表中存在至少一个匹配时返回行

- ```sql
  语法：
  1.SELECT column_name(s) FROM table1
  INNER JOIN table2
  ON table1.column_name=table2.column_name;
  2.SELECT column_name(s) FROM table1
  JOIN table2
  ON table1.column_name=table2.column_name;
  两者等价
  ```

- ```sql
  语法解释：
  1.SELECT Websites.name, access_log.count, access_log.date
  FROM Websites
  INNER JOIN access_log
  ON Websites.id=access_log.site_id
  ORDER BY access_log.count;
  关联了两张表，过滤条件是表1的id字段等于表2的site_id字段，返回name，count，date拼接的记录
  2.SELECT table1.a,table2.b,table3.c
  FROM table1
  INNER JOIN table2 ON table1.id=table2.site_id
  INNER JOIN table3 ON table1.id=table3.web_id
  关联了三张表，同理可以关联更多，也可以加更多的过滤条件
  ```

#### 4.9 LEFT JOIN

- 功能：从左表（table1）返回所有的行，即使右表（table2）中没有匹配。如果右表中没有匹配，则结果为 NULL。

- 说明：左表一定会返回所有的行，右表对不上就为NULL，返回哪些字段需要指定

- ```sql
  语法：
  SELECT column_name(s) FROM table1
  LEFT JOIN table2
  ON table1.column_name=table2.column_name;
  ```

- ```sql
  语法解释：
  1.SELECT Websites.name, access_log.count, access_log.date
  FROM Websites
  LEFT JOIN access_log
  ON Websites.id=access_log.site_id
  ORDER BY access_log.count DESC;
  表Websites为左表，access_log为右表
  ```

- 补充：。。。

#### 4.10 RIGHT JOIN

- 功能：从右表（table2）返回所有的行，即使左表（table1）中没有匹配。如果左表中没有匹配，则结果为 NULL。

- 说明：右表一定会返回所有的行，左表对不上就为NULL，返回哪些字段需要指定

- ```sql
  语法：
  SELECT column_name(s)
  FROM table1
  RIGHT JOIN table2
  ON table1.column_name=table2.column_name;
  ```

- ```sql
  语法解释：
  1.SELECT Websites.name, access_log.count, access_log.date
  FROM access_log
  RIGHT JOIN Websites
  ON access_log.site_id=Websites.id
  ORDER BY access_log.count DESC;
  表access_log为左表，表Websites为右表
  ```

- 补充：。。。

#### 4.11 FULL JOIN

- 功能：关键字只要左表（table1）和右表（table2）其中一个表中存在匹配，则返回行.

- 返回：关键字结合了 LEFT JOIN 和 RIGHT JOIN 的结果。

- ```sql
  语法
  SELECT column_name(s)
  FROM table1
  FULL OUTER JOIN table2
  ON table1.column_name=table2.column_name;
  ```

- 说明：**MySql**不支持该语法

#### 4.12 UNION

- 功能：合并两个或多个 SELECT 语句的结果。
- 返回：合并两个或多个 SELECT 语句的结果集。
- 注意：UNION 内部的每个 SELECT 语句必须拥有相同数量的列。列也必须拥有相似的数据类型。同时，每个 SELECT 语句中的列的顺序必须相同。
- 说明：意思是把不同的表的列做了合并，合并的内容***数量类型顺序***要相同

- ```sql
  语法
  1.SELECT column_name(s) FROM table1
  UNION
  SELECT column_name(s) FROM table2;
  说明：UNION 操作符选取不同的值。如果允许重复的值，请使用 UNION ALL。
  	就是说合并之后，重复的内容会去掉，保留重复的话用 UNION ALL。
  2.SELECT column_name(s) FROM table1
  UNION ALL
  SELECT column_name(s) FROM table2;
  说明：保留重复；UNION 结果集中的列名总是等于 UNION 中第一个 SELECT 语句中的列名。
  ```

- ```sql
  语法解释
  1.SELECT country FROM Websites
  UNION
  SELECT country FROM apps
  ORDER BY country;
  从Websites和apps中选取所有不同的country（取不同！）
  2.SELECT country FROM Websites
  UNION ALL
  SELECT country FROM apps
  ORDER BY country;
  从Websites和apps中选取所有不同的country（取所有！）
  3.SELECT country, name FROM Websites
  WHERE country='CN'
  UNION ALL
  SELECT country, app_name FROM apps
  WHERE country='CN'
  ORDER BY country;
  加上where再复杂一点
  ```

- 说明：合并列，可以用于统计

#### 4.13 SELECT INTO

- 功能：从一个表复制信息到另一个表

- 返回：返回执行信息

- 注意：该语法**MySQL**不支持，要求**新表不存在**，新表自动创建

- ```sql
  语法：
  1.SELECT *
  INTO newtable [IN externaldb]
  FROM table1;
  复制所有的列插入到新表
  2.SELECT column_name(s)
  INTO newtable [IN externaldb]
  FROM table1;
  复制所选的列插入到新表中
  ```

- ```sql
  语法说明：
  1.SELECT *
  INTO WebsitesBackup2016
  FROM Websites;
  会创建新表，所以是做Websites的备份附件
  2.SELECT name, url
  INTO WebsitesBackup2016
  FROM Websites;
  只复制制定的列到新表中
  3.SELECT *
  INTO WebsitesBackup2016
  FROM Websites
  WHERE country='CN';
  只复制中国的网站插入到新表中：
  4.SELECT Websites.name, access_log.count, access_log.date
  INTO WebsitesBackup2016
  FROM Websites
  LEFT JOIN access_log
  ON Websites.id=access_log.site_id;
  复制多个表中的数据插入到新表中：
  ```

- 说明：用于自动创建新表复制，当然也可以用来创建新表

#### 4.14 INSERT INTO SELECT

- 功能：从一个表复制到另一个表（新表是已有表）

- 返回：执行时间等信息

- 注意：新表为已有表

- ```sql
  语法：
  1.INSERT INTO table2
  SELECT * FROM table1;
  从一个表中复制所有的列插入到另一个已存在的表中
  2.INSERT INTO table2
  (column_name(s))
  SELECT column_name(s)
  FROM table1;
  只复制希望的列插入到另一个已存在的表中
  ```

- ```sql
  语法说明：
  1.INSERT INTO Websites (name, country)
  SELECT app_name, country FROM apps;
  指定某些列进行复制
  2.INSERT INTO Websites (name, country)
  SELECT app_name, country FROM apps
  WHERE id=1;
  指定某些列进行复制，可以再加一些条件
  ```

- 说明：**相当于追加数据，不用保证列名相同，也不用复制所有列**

#### 4.15 CREATE DATABASE

- 功能：创建数据库

- ```sql
  语法
  CREATE DATABASE dbname;
  ```

- 说明：一般有三层 **数据库->schema->表**，这里创建的是**schema**

#### 4.16 CREATE TABLE

- 功能：创建数据库中的表

- 返回：执行结果

- ```sql
  语法
  CREATE TABLE table_name
  (
  column_name1 data_type(size),
  column_name2 data_type(size),
  column_name3 data_type(size),
  ....
  );
  创建表，指定列以及其类型长度
  ```

- ```sql
  语法说明：
  CREATE TABLE Persons
  (
  PersonID int,
  LastName varchar(255),
  FirstName varchar(255),
  Address varchar(255),
  City varchar(255)
  );
  创建一个名为 "Persons" 的表，包含五列：PersonID、LastName、FirstName、Address 和 City
  ```

- 说明：三层 **数据库->schema->表**，这里是表

#### 4.17 约束

- 功能：规定表中的数据规则

- 返回：执行结果

- 注意：一般用在新建表，加一些约束条件

- ```sql
  语法
  CREATE TABLE table_name
  (
  column_name1 data_type(size) constraint_name,
  column_name2 data_type(size) constraint_name,
  column_name3 data_type(size) constraint_name,
  ....
  );
  ```

- ```sql
  语法说明
  1.CREATE TABLE Persons
  (
      Id_P int NOT NULL,
      LastName varchar(255) NOT NULL,
      FirstName varchar(255),
      Address varchar(255),
      City varchar(255),
      PRIMARY KEY (Id_P)  //PRIMARY KEY约束
  )
  2.CREATE TABLE Persons
  (
      Id_P int NOT NULL PRIMARY KEY,   //PRIMARY KEY约束
      LastName varchar(255) NOT NULL,
      FirstName varchar(255),
      Address varchar(255),
      City varchar(255)
  )
  新建表，并且将Id_P设为主键
  ```

  | 名称            | 内容                                                         |
  | --------------- | ------------------------------------------------------------ |
  | **NOT NULL**    | 指示某列不能存储 NULL 值                                     |
  | **UNIQUE**      | 保证某列的每行必须有唯一的值                                 |
  | **PRIMARY KEY** | NOT NULL 和 UNIQUE 的结合。确保某列（或两个列多个列的结合）有唯一标识，有助于更容易更快速地找到表中的一个特定的记录。 |
  | **FOREIGN KEY** | 保证一个表中的数据匹配另一个表中的值的参照完整性             |
  | **CHECK**       | 保证列中的值符合指定的条件                                   |
  | **DEFAULT**     | 规定没有给列赋值时的默认值                                   |

- 说明：我在建立数据库脚本的时候用过，新建表，加入一些约束条件

- 说明2：约束用在两个地方

  - **约束可以在创建表时规定（通过 CREATE TABLE 语句）**
  - **在表创建之后规定（通过 ALTER TABLE 语句）**

##### NOT NULL

```sql
1.CREATE TABLE Persons
(
P_Id int NOT NULL,
LastName varchar(255) NOT NULL,
FirstName varchar(255),
Address varchar(255),
City varchar(255)
)
创建表时，加入约束
2.alter table x modify column_name null;
alter table x modify column_name not null;
已有表，删除约束
```

##### UNIQUE

```sql
1.CREATE TABLE Persons
(
P_Id int NOT NULL,
LastName varchar(255) NOT NULL,
FirstName varchar(255),
Address varchar(255),
City varchar(255),
UNIQUE (P_Id)
)
新建表，加约束
2.ALTER TABLE Persons ADD UNIQUE (P_Id)
3.ALTER TABLE Persons ADD CONSTRAINT uc_PersonID UNIQUE (P_Id,LastName)
已有表，加约束
4.ALTER TABLE Persons DROP INDEX uc_PersonID
已有表，撤销约束
```

##### PRIMARY KEY

```sql
1.CREATE TABLE Persons
(
P_Id int NOT NULL,
LastName varchar(255) NOT NULL,
FirstName varchar(255),
Address varchar(255),
City varchar(255),
PRIMARY KEY (P_Id)
)
2.ALTER TABLE Persons ADD PRIMARY KEY (P_Id)
3.ALTER TABLE Persons ADD CONSTRAINT pk_PersonID PRIMARY KEY (P_Id,LastName)
4.ALTER TABLE Persons DROP PRIMARY KEY
```

##### FOREIGN KEY

```sql
1.CREATE TABLE Orders
(
O_Id int NOT NULL,
OrderNo int NOT NULL,
P_Id int,
PRIMARY KEY (O_Id),
FOREIGN KEY (P_Id) REFERENCES Persons(P_Id)
)
2.ALTER TABLE Orders ADD FOREIGN KEY (P_Id)
REFERENCES Persons(P_Id)
3.ALTER TABLE Orders DROP FOREIGN KEY fk_PerOrders
```

##### CHECK

```sql
1.CREATE TABLE Persons
(
P_Id int NOT NULL,
LastName varchar(255) NOT NULL,
FirstName varchar(255),
Address varchar(255),
City varchar(255),
CHECK (P_Id>0)
)
2.ALTER TABLE Persons ADD CHECK (P_Id>0)
3.ALTER TABLE Persons
ADD CONSTRAINT chk_Person CHECK (P_Id>0 AND City='Sandnes')
4.ALTER TABLE Persons DROP CHECK chk_Person
```

##### DEFAULT

```SQL
1.CREATE TABLE Persons
(
    P_Id int NOT NULL,
    LastName varchar(255) NOT NULL,
    FirstName varchar(255),
    Address varchar(255),
    City varchar(255) DEFAULT 'Sandnes'
)
2.ALTER TABLE Persons ALTER City SET DEFAULT 'SANDNES'
3.ALTER TABLE Persons ALTER City DROP DEFAULT
```

#### 4.18 CREATE INDEX

- 功能：在表中创建索引

- 返回：执行结果

- 说明：不同类型的数据库关于索引的语法有差异

- ```sql
  语法：
  1.CREATE INDEX index_name ON table_name (column_name)
  在表上创建一个简单的索引。允许使用重复的值
  2.CREATE UNIQUE INDEX index_name ON table_name (column_name)
  在表上创建一个唯一的索引。不允许使用重复的值：唯一的索引意味着两个行不能拥有相同的索引值。
  ```

- ```sql
  语法说明：
  1.CREATE INDEX PIndex ON Persons (LastName)
  在 "Persons" 表的 "LastName" 列上创建一个名为 "PIndex" 的索引
  2.CREATE INDEX PIndex ON Persons (LastName, FirstName)
  索引不止一个列
  ```

- 补充：索引创建，注意不同数据库的区别

#### 4.19 DROP

- 功能：撤销/删除（索引，表，数据库）

- 返回：执行结果

- 说明：一般跑数据库脚本的时候，先drop删除，再插表进去

- ```sql
  语法
  1.DROP INDEX 语句（删除索引）
  用于MySQL的DROP INDEX
  ALTER TABLE table_name DROP INDEX index_name
  2.DROP TABLE 语句（删除表）
  DROP TABLE table_name
  3.DROP DATABASE 语句（删除数据库）
  DROP DATABASE database_name
  4.TRUNCATE TABLE 语句
  TRUNCATE TABLE table_name
  仅仅删除表内的数据，但并不删除表本身
  ```

- 补充：跑脚本的时候挺常用的，数据库软件跑指令不怎么用

#### 4.20 ALTER TABLE

- 功能：已有表中添加、删除或修改列。