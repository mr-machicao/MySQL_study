# MySQL_study
以前学习mysql的一些笔记
目录：
1、查询语句(SELECT、DISTINCT)
2、使用WHERE提取满足标准的记录：
3、WHERE子句之逻辑运算符:AND、OR、NOT
4、WHERE子句之特殊条件：is null、between and、in、like
5、插入语句
6、更新语句
7、删除语句
8、数据库操作
9、ORDER BY
10、SELECT TOP,LIMIT,ROWNUM子句
11、通配符
12、操作符——IN、BETWEEN、UNION
13、别名
14、连接（JOIN）
15、SELECT INTO 语句
16、INSERT INTO SELECT
17、约束
18、索引
19、DROP
20、ALTER TABLE
21、AUTO INCREMENT
22、视图（Views）
23、日期
24、NULL
25、Aggregate 函数
26、GROUP BY语句
27、HAVING子句
28、Scalar函数
1、查询语句(SELECT、DISTINCT)：
------查询表中数据的语句
SELECT name,country
FROM websites;

SELECT * FROM websites；

------返回列中唯一不同的值，即去除重复值
SELECT DISTINCT name,country
FROM websites;

2、使用WHERE提取满足标准的记录：
---数值字段，请不要用引号
SELECT name,country
FROM websites
WHERE id=1;

---文本字段，使用单引号
SELECT *
FROM websites
WHERE country='CN';

----比较运算符
SELECT *
FROM websites
WHERE id >=2 and id != 4;

3、WHERE子句之逻辑运算符:AND、OR、NOT,优先级：()、not、and、or（别忘了这些是可以结合使用的哦）
SELECT * FROM websites WHERE id >= 2 and id != 4;
SELECT * FROM websites WHERE id >= 2 or name ='淘宝'；
SELECT * FROM websites WHERE not id < 2；---满足不包涵该条件的值

4、WHERE子句之特殊条件：is null、between and、in、like
SELECT *
FROM websites
WHERE country is null;---查询 websites表中 country列中的空值

SELECT *
FROM websites
WHERE id between 2  and 5;--- 查询websites表中 id 列中大于等于 2 的小于等于 5 的值

SELECT *
FROM websites
WHERE id in (2,4,6); --- 查询 websites 表 id 列中等于2，4，6 的值

SELECT *
FROM websites
WHERE name like 'm%';--- 查询 websites 表中 name 列中有 m的值，m 为要查询内容中的模糊信息。

	*  % 表示多个字值，_ 下划线表示一个字符；
	*  M% : 为能配符，正则表达式，表示的意思为模糊查询信息为 M 开头的。
	*  %M% : 表示查询包含M的所有内容。
	*  %M_ : 表示查询以M在倒数第二位的所有内容。



5、插入语句：
INSERT INTO websites (name,url,country)
VALUES('stackoverflow','http://stackoverflow.com/','IND');-- 向指定的"name"、"url" 和 "country" 列插入数据

-----没有指定要插入数据的列名的时候，需要把列出插入行的每一列数据
INSERT INTO table_name
VALUES (value1,value2,value3....);


6、更新语句:
*****如果不使用WHERE 子句，时请慎重，
*****因为这样会把整个表中的数据都更新了
UPDATE websites
SET alexa=5000,country='USA'
WHERE NAME = '菜鸟教程';---- 把 "菜鸟教程" 的 alexa 排名更新为 5000，country 改为 USA


7、删除语句：
DELETE FROM websites
WHERE name='百度' AND country='CN';--- 从 "Websites" 表中删除网站名为 "百度" 且国家为 CN 的网站 

---- 在不删除表的情况下，删除表中所有的行。这意味着表结构、属性、索引将保持不变
DELETE FROM websites;
等价于：
DELETE * FROM websites；

8、数据库操作：
CREATE DATABASE dbname; 
use databaseName;  -----选中使用的数据库
set names utf8;-----设置使用的字符集

CREATE TABLE Persons
(
     PersonID int,
     LastName varchar(255),
     FirstName varchar(255),
     Address varchar(255),
     City varchar(255)
);---- 创建一个名为 "Persons" 的表，包含五列：PersonID、LastName、FirstName、Address 和 City


9、ORDER BY----对结果集按照一列或者多列进行排序，默认为升序(ASC)，可以是降序（DESC）。按照多列排序时，先按照第一个条件排，再按照第二个排

SELECT * 
FROM websites
ORDER BY country,alexa ASC;--- 从 "Websites" 表中选取所有网站，并按照 "country" 和 "alexa" 列排序 



10、SELECT TOP,LIMIT,ROWNUM子句：
(不同数据库的用法不同，mysql与oracle中该子句用法相同)
SELECT * FROM websites LIMIT 2;--- 从 "Websites" 表中选取头两条记录

11、通配符：
%:替代0个或多个字符
_ ： 替代一个字符
[charlist] : 字符序列中的任何单一字符
[^charlist]或[!charlist] ： 不在字符序列中的任何单一字符

---- MySQL 中使用 REGEXP 或 NOT REGEXP 运算符 (或 RLIKE 和 NOT RLIKE) 来操作正则表达式
SELECT * FROM websites
WHERE  name REGEXP '^[GFs]';---  SQL 语句选取 name 以 "G"、"F" 或 "s" 开始的所有网站

SELECT * FROM websites
WHERE name REGEXP '^[^A-H]'; ---选取 name 不以 A 到 H 字母开头的网站


12、操作符——IN、BETWEEN、UNION：

IN：
---IN允许在WHERE子句中规定多个值
---IN与=的相同点：均在WHERE中使用作为筛选条件之一、均是等于的含义
---不同点：IN可以规定多个值，等于规定一个值。
SELECT * FROM websites
WHERE name IN ('Google','百度'); --- 选取 name 为 "Google" 或 "百度" 的所有网站

BETWEEN：
SELECT * FROM websites
WHERE alexa NOT BETWEEN 1 AND 20;--- 选取 alexa 不在 1 和 20 之间的所有网站

SELECT * FROM websites
WHERE (alexa BETWEEN 1 AND 20)
AND NOT country IN ('USA','IND');--- 选取alexa介于 1 和 20 之间但 country 不为 USA 和 IND 的所有网站

SELECT * FROM websites
WHERE name NOT BETWEEN 'A' AND 'H';--- 选取 name 不介于 'A' 和 'H' 之间字母开始的所有网站

SELECT * FROM access_log
WHERE date BETWEEN '2016-05-10' AND '2016-05-14';--- 选取 date 介于 '2016-05-10' 和 '2016-05-14' 之间的所有访问记录

UNION：
SELECT country FROM websites
UNION
SELECT country FROM apps
ORDER BY COUNTRY;--- 从 "Websites" 和 "apps" 表中选取所有不同的country（只有不同的值）

SELECT country,name FROM websites WHERE cuntry='CN'
UNION ALL 
SELECT country,app_name FROM apps WHERE country='CN'
ORDER BY country;---  从 "Websites" 和 "apps" 表中选取所有的中国(CN)的数据（也有重复的值）

13、别名：

---表的别名
SELECT w.name,w.url,a.count,a.date
FROM websites AS w,access_log AS a
WHERE a.site_id=w.id and w.name = "菜鸟教程";--- 使用 "Websites" 和 "access_log" 表，并分别为它们指定表别名 "w" 和 "a"（通过使用别名让 SQL 更简短）

---列的别名
SELECT name AS n,country AS c
FROM websites;

14、连接：
----join用于把来自两个或多个表的行结合起来

----INNER JOIN：如果表中有至少一个匹配，则返回行,INNER JOIN 与 JOIN是相同的
SEKECT websites.id,websites.name,access_log.count,access_log.date
FROM websites
INNER JOIN access_log
ON websi  tes.id=access_log.site_id
ORDER BY access_log.count;--- "Websites" 表中的 "id" 列指向 "access_log" 表中的字段 "site_id"。这两个表是通过 "site_id" 列联系起来的

-----LEFT JOIN：即使右表中没有匹配，也从左表返回所有的行,右表没匹配记录也会显示(null)
SELECT websites.name,access_log.count,access_log.date
FROM websites
LEFT JOIN access_log
ON websites.id=access_log.site_id
ORDER BY access_log.count DESC;

----RIGHT JOIN：即使左表中没有匹配，也从右表返回所有的行,,右表没匹配记录也会显示(null)
SELECT websites.name,access_log.count,access_log.date
FROM access_log
RIGHT JOIN websites
ON access_log.site_id=websites.id
ORDER BY access_log.count DESC;


FULL JOIN：只要其中一个表中存在匹配，则返回行(MySQL中不支持FULL OUTER JOIN)
SELECT websites.name,access_log.count,access_log,date
FROM websites
FULL OUTER JOIN access_log
ON websites.id=access_log.site_id
ORDER BY access_log.count DESC;


15、SELECT INTO--- 从一个表复制数据，然后把数据插入到另一个新表中

SELECT name,url
INTO websitesBackup2017
FROM websites;---- 只复制一些列插入到新表中

SELECT websites.name,access_log.count,access_log.date
INTO websitesBackup2016
FROM websites
LEFT JOIN access_log
ON  websites.id=access_log.site_id;---- 复制多个表中的数据插入到新表中

SELECT *
INTO newtable
FROM table1
WHERE 1=0;--- 添加促使查询没有数据返回的 WHERE 子句，来实现创建一个空表

16、INSERT INTO SELECT--- 从一个表复制数据，然后把数据插入到一个已存在的表中

INSERT INTO websites (name,country)
SELECT app_name,country FROM APPS
WHERE id=1;---复制 "apps" 中的id=1的数据插入到 "Websites" 中


17、约束（Constraints）
-----SQL约束用于规定表中的数据规则。
-----如果存在违反约束的数据行为，行为会被终止
-----约束可以在穿件表时规定（通过CREATE TABLE 语句），或者在表创建之后规定（通过ALTER TABLE 语句）。

-----NOT NULL - 指示某列不能存储 NULL 值。
CREATE TABLE Persons
(
     P_Id int NOT NULL,
     LastName varchar(255) NOT NULL,
     FirstName varchar(255),
     Address varchar(255),
     City varchar(255)
);--- 强制 "P_Id" 列和 "LastName" 列不接受 NULL 值

-----UNIQUE - 保证某列的每行必须有唯一的值。每个表只能有一个PRIMARY KEY约束，但可以有多个UNIQUE约束，而PRIMARY KEY约束拥有自定义的UNIQUE约束
CREATE TABLE Persons
(
     P_Id int NOT NULL,
     LastName varchar(255) NOT NULL,
     FirstName varchar(255),
     LastName varhcar(255),
     Address varchar(255),
     City varchar(255),
     UNIQUE(P_Id)
);---  在 "Persons" 表创建时在 "P_Id" 列上创建 UNIQUE 约束

CTRATE TABLE Persons
(
     P_Id int NOT NULL,
     FirstName varchar(255) NOT NULL,
     LastName varchar(255) ,
     Address varchar(255),
     City varchar(255)
     CONSTRAINT uc_PersonID UNIQUE (P_Id,LastName)
);--- 命名 UNIQUE 约束，并定义多个列的 UNIQUE 约束

ALTER TABLE Persons
ADD UNIQUE (P_Id);--- 当表已被创建时，在 "P_Id" 列创建 UNIQUE 约束

ALTER TABLE Persons
ADD CONSTRAINT uc_PersonID UNIQUE (P_Id,lastName);--- 命名 UNIQUE 约束，并定义多个列的 UNIQUE 约束

ALTER TABLE Persons
DROP INDEX uc_PersonID;--- 撤销 UNIQUE 约束


-----PRIMARY KEY - NOT NULL 和 UNIQUE 的结合。确保每条记录（某列或两个列多个列的结合）有唯一标识，每个表都应该有唯一的、不能为null的且只能有一个的主键，有助于更容易更快速地找到表中的一个特定的记录。
CREATE TABLE Persons
(
     P_Id int NOT NULL,
     FirstName varchar(255) NOT NULL,
     LastName varchar(255),
     Address varchar(255),
     City varchar(255),
     PRIMARY KEY (P_Id)
);--- 在 "Persons" 表创建时在 "P_Id" 列上创建 PRIMARY KEY 约束

CREATE TABLE Persons
(
     P_Id int NOT NULL,
     FirstName varchar(255) NOT NULL,
     LastName varchar(255),
     Address varchar(255),
     City varchar(255),
     CONSTRAINT pk_PersonID PRIMARY KEY (P_Id,LastNaem)
);--- 在该实例中，只有一个主键 PRIMARY KEY（pk_PersonID）。然而，pk_PersonID 的值是由两个列（P_Id 和 LastName）组成的

ALTER TABLE Persons
ADD PRIMARY KEY (P_Id);--- 当表已被创建时，在 "P_Id" 列创建 PRIMARY KEY 约束

ALTER TABLE Persons
ADD CONSTRAINT pk_PersonID PRIMARY KEY (P_Id,LastName);--- 命名 PRIMARY KEY 约束，并定义多个列的 PRIMARY KEY 约束, 如果使用 ALTER TABLE 语句添加主键，必须把主键列声明为不包含 NULL 值（在表首次创建时）

ALTER TABLE Persons
DROP PRIMARY KEY;--- 撤销 PRIMARY KEY 约束


-----FOREIGN KEY - 一个表中的FOREIGN KEY指向另一个表中的PRIMARY KEY。 预防破坏表之间连接的行为， 防止非法数据插入外键列，保证一个表中的数据匹配另一个表中的值的参照完整性。
CREATE TABEL Orders
(
     O_Id int NOT NULL,
     OrderNo int NOT NULL,
     P_Id int,
     PRIMARY KEY (O_Id),
     FOREIGN KEY (P_Id) REFERENCES Persons(P_Id)
);--- 在 "Orders" 表创建时在 "P_Id" 列上创建 FOREIGN KEY 约束

CREATE TABLE Orders
(
     O_Id int NOT NULL,
     OrderNo int NOT NULL,
     P_Id int,
     PRIMARY KEY (O_Id),
     CONSTRAINT fk_PerOrders FOREIGN KEY (P_Id)
     REFERENCES Persons (P_Id)
);--- 命名 FOREIGN KEY 约束，并定义多个列的 FOREIGN KEY 约束

ALTER TABLE Orders
ADD FOREIGN KEY (P_Id)
REFERENCES Persons(P_Id);--- 当 "Orders" 表已被创建时，在 "P_Id" 列创建 FOREIGN KEY 约束

ALTER TABLE Orders
ADD CONTRAINT fk_PerOrders 
FOREIGN KEY (P_Id)
REFERENCES Persons(P_Id);--- 命名 FOREIGN KEY 约束，并定义多个列的 FOREIGN KEY 约束

ALTER TABLE Orders
DROP FOREIEN KEY fk_PerOrders;--- 撤销 FOREIGN KEY 约束

-----CHECK - 限制列中的值得范围。如果对单个列定义CHECK约束，那么该列只允许特定的值；如果对一个表定义CHECK约束，那么此约束会基于行中其他列的值在特定的列中对值进行限制。
CREATE TABLE Persons
(
     P_Id int NOT NULL,
     LastName varchar(255) NOT NULL,
     FirstName varchar(255),
     Address varchar(255),
     City varchar(255),
     CHECK (P_Id > 0)
);--- 在 "Persons" 表创建时在 "P_Id" 列上创建 CHECK 约束。CHECK 约束规定 "P_Id" 列必须只包含大于 0 的整数

CREATE TABLE Persons
(
     P_Id int NOT NULL,
     LastName varchar(255) NOT NULL,
     FirstName varchar(255),
     Address varhcar(255),
     City varchar(255),
     CONSTRAINT chk_Person CHECK (P_Id > 0 AND City='Sandnes')
);--- 命名 CHECK 约束，并定义多个列的 CHECK 约束

ALTER TABLE Persons
ADD CHECK (P_Id>0);--- 当表已被创建时，在 "P_Id" 列创建 CHECK 约束

ALTER TABLE Persons
ADD CONSTRAINT chk_Person CHECK (P_Id > 0 AND City='Sandnes');--- 命名 CHECK 约束，并定义多个列的 CHECK 约束

ALTER TABLE Persons
DROP CHECK chk_Person；---撤销CHECK约束

-----DEFAULT - 给列设置默认值。

CHECK TABLE Persons
(
     P_Id int NOT NULL,
     FirstName varchar(255) NOT NULL,
     LastName varchar(255) ,
     Address varchar(255) ,
     City varchar(255) DEFAULT 'Sandnes'
);--- 在 "Persons" 表创建时在 "City" 列上创建 DEFAULT 约束

CREATE TABLE Oresers
(
     O_Id int NOT NULL,
     OrderNo int NOT NULL,
     P_Id int,
     OrderDate date DEFAULT GETDATE()
);--- 使用类似 GETDATE() 这样的函数，DEFAULT 约束也可以用于插入系统值

ALTER TABLE Persons
ALTER City SET DEFAULT 'SANDNES';--- 当表已被创建时，在 "City" 列创建 DEFAULT 约束

ALTER TABLE Persons
ALTER City DROP DEFAULT;--- 撤销 DEFAULT 约束


18、索引

CREATE INDEX PIndex
ON Persons (Lastname);--- 在 "Persons" 表的 "LastName" 列上创建一个名为 "PIndex" 的索引

CREATE INDEX PIndex
ON Persons (LastName,FirstName);--- 如果希望索引不止一个列，可以在括号中列出这些列的名称，用逗号隔开

CREATE UNIQUE INDEX index_name
ON table_name (column_name);--- 在表上创建一个唯一的索引。不允许使用重复的值：唯一的索引意味着两个行不能拥有相同的索引值

ALTER TABLE table_name
DROP INDEX index_name;--- 删除表中的索引

19、DROP
---- 通过使用 DROP 语句，可以轻松地删除索引、表和数据库

ALTER TABLE table_name
DROP INDEX index_name;--- 删除表中的索引

DROP TABLE table_name; --- 删除表

DROP DATABASE database_name;--- 删除数据库

TRUNCATE TABLE table_name;---只删除表内的数据，但并不删除表本身


20、ALTER TABLE
---- ALTER TABLE 语句用于在已有的表中添加、删除或修改列

ALTER TABLE table_name
ADD column_name datatype;--- 在表中添加列

ALTER TABLE table_name
DROP COLUMN column_name;--- 删除表中的列

ALTER TABLE table_name
MODIFY COLUMN column_name datatype;--- 改变表中列的数据类型

ALTER TABLE Persons
ALTER COLUMN DateOfBirth year--- 改变 "Persons" 表中 "DateOfBirth" 列的数据类型(原先的类型为date)


21、AUTO INCREMENT
--- Auto-increment 会在新记录插入表中时生成一个唯一的数字. 我们通常希望在每次插入新记录时，自动地创建主键字段的值
CREATE TABLE Persons
(
     ID int NOT NULL AUTO_INCREMENT,
     LastName varchar(255) NOT NULL,
     FirstName varchar(255),
     Address varchar(255),
     City varchar(255),
     PRIMARY KEY (ID)
);--- 把 "Persons" 表中的 "ID" 列定义为 auto-increment 主键字段

ALTER TABLE Persons
AUTO_INCREMENT=100;---AUTO_INCREMENT 的开始值是 1，每条新记录递增 1。
设置 AUTO_INCREMENT 序列的初始值为100


22、视图（Views）
--- 视图总是显示最新的数据！每当用户查询视图时，数据库引擎通过使用视图的 SQL 语句重建数据
语法：
CREATE VIEW view_name AS
SELECT column_name(s)
FROM table_name
WHERE condition

CREATE VIEW [Current Product List] AS
SELECT ProductID,ProductName
FROM Products
WHERE Discontinued=No;--- 视图 "Current Product List" 会从 "Products" 表列出所有正在使用的产品（未停产的产品）

SELECT * FROM [Current Product List];--- 查询上面这个视图

CREATE VIEW [Products Above Average Price] AS
SELECT ProductName,UnitPrice
FROM Products
WHERE UnitPrice>(SELECT AVG(UnitPrice) FROM Products);--- 选取 "Products" 表中所有单位价格高于平均单位价格的产品

CREATE VIEW [Current Product List] AS
SELECT ProductID,ProductName,Category
FROM Products
WHERE Discountinued=No;--- 向 "Current Product List" 视图添加 "Category" 列

DROP VIEW view_name;--- 删除视图


23、日期
MySQL使用下列数据类型在数据库中存储日期或日期时间值：

DATE - 格式：YYYY-MM-DD

DATETIME - 格式：YYYY-MM-DD HH:MM:SS

TIMESTAMP - 格式：YYYY-MM-DD HH:MM:SS

YEAR - 格式：YYYY 或 YY

即便 DATETIME 和 TIMESTAMP 返回相同的格式，它们的工作方式很不同。在 INSERT 或 UPDATE 查询中，TIMESTAMP 自动把自身设置为当前的日期和时间。TIMESTAMP 也接受不同的格式，比如 YYYYMMDDHHMMSS、YYMMDDHHMMSS、YYYYMMDD 或 YYMMDD。

SELECT * FROM Orders WHERE OrderDate='2008-11-11';--- 选取 OrderDate 为 "2008-11-11" 的记录

* *如果您希望使查询简单且更易维护，那么请不要在日期中使用时间部分！* *
如果希望使得查询简单且更易维护，那么请不要在日期中使用时间部分！因为使用时间部分我们不好查询！

24、NULL
--- NULL 值代表遗漏的未知数据。默认的表的列可以存放null值
--- 无法比较 NULL 和 0；它们是不等价的

无法使用比较运算符来测试NULL值，必须使用IS NULL和IS NOT NULL操作符

SELECT LastName,FirstName,Address FROM Persoins
WHERE Address IS NULL;--- 选取在 "Address" 列中带有 NULL 值的记录

SEKECT LastName,FirstName,Address FROM Persons
WHERE Address IS NOT NULL;--- 选取在 "Address" 列中不带有 NULL 值的记录

----使用IFNULL()函数解决结果中如果是NULL值时的问题，使用IFNULL()规定当为NULL时返回0。（也可以使用COALESCE()函数）
SELECT ProductName,UnitPrice*(UnitsInStock + IFNULL(UnitsOnOrder,0))
FROM Products;--- 如果 "UnitsOnOrder" 是 NULL，则不会影响计算，因为如果值是 NULL 则 ISNULL() 返回 0



25、Aggregate 函数
SQL Aggregate函数：
--- SQL Aggregate 函数计算从列中取得的值，返回一个单一的值。
--- 有用的 Aggregate 函数：

----AVG() - 返回平均值
SELECT AVG(column_name) FROM table_name;

SELECT site_id,count FROM access_log
WHERE count > (SELECT AVG(count) FROM access_log);--- 选择访问量高于平均访问量的 "site_id" 和 "count"

----COUNT() - 返回行数
SELECT COUNT(column_name) FROM table_name;--- COUNT(column_name) 函数返回指定列的值的数目（NULL 不计入）
SELECT COUNT(ALEXA) FROM websites;


SELECT COUNT(*) FROM table_name;--- COUNT(*) 函数返回表中的记录数
SELECT COUNT(*) FROM access_log;

SELECT COUNT(DISTINCT column_name) FROM table_name;--- COUNT(DISTINCT column_name) 函数返回指定列的不同值的数目
SELECT COUNT(DISTINCT country) FROM websites;

----FIRST() - 返回第一个记录的值,MySQL没有该函数
SELECT column_name FROM table_name
ORDER BY column_name ASC
LIMIT 1;

SELECT name AS FirstSite 
FROM websites LIMIT 1;--- 选取 "Websites" 表的 "name" 列中第一个记录的值

----LAST() - 返回最后一个记录的值,MySQL没有该函数
SELECT column_name FROM table_name
ORDER BY column_name DESC 
LIMIT 1;

SELECT name FROM websites
ORDER BY id DESC
LIMIT 1;--- 选取 "Websites" 表的 "name" 列中最后一个记录的值

----MAX() - 返回最大值
SELECT MAX(column_name) FROM table_name;

SELECT MAX(alexa) AS max_alexa FROM websites;--- 从 "Websites" 表的 "alexa" 列获取最大值

----MIN() - 返回最小值
SELECT MIN(column_name) FROM table_name;

SELECT MIN(alexa) AS min_alexa FROM websites;--- 从 "Websites" 表的 "alexa" 列获取最小值

----SUM() - 返回总和
SELECT SUM(column_name) FROM table;

SELECT SUM(count) AS nums FROM access_log;--- 查找 "access_log" 表的 "count" 字段的总数


26、GROUP BY语句
---- GROUP BY 语句用于结合聚合函数，根据一个或多个列对结果集进行分组
SELECT column_name,aggregate_function(column_name)
FROM table_name
WHERE column_name operator value
GROUP BY column_name;

SELECT site_id,SUM(access_log.count) AS nums
FROM access_log
GROUP BY site_id;---统计 access_log 各个 site_id 的访问量(count).这条SQL的意思就是根据site_id进行分组，然后统计每组site_id对应的不同count的总和

SELECT websites.name,COUNT(access_log.aid) AS nums
FROM access_log
LEFT JOIN websites
ON access_log.site_id = websites.id
GROUP BY websites.name;


27、HAVING子句
----- 在 SQL 中增加 HAVING 子句原因是，WHERE 关键字无法与聚合函数一起使用。
----- HAVING 子句可以让我们筛选分组后的各组数据。

SELECT column_name,aggregate_function(column_name)
FROM table_name
WHERE column_name operator value
GROUP BY column_name
HAVING aggregate_function(column_name) operator value;

SELECT websites.name,websites.url,SUM(access_log.count) AS nums
FROM (access_log
INNER JOIN websites
ON access_log.site_id=websites.id)
GROUP BY websites.name
HAVING SUM(access_log.count) >200;----- 查找总访问量大于 200 的网站

SELECT websites.name,SUM(access_log.count) AS nums
FROM websites
INNER JOIN access_log
ON websites.id=access_log.site_id
WHERE websites.alexa < 200
GROUP BY websites.name
HAVING SUM(access_log.count) > 200;----- 查找总访问量大于 200 的网站，并且 alexa 排名小于 200, 在 SQL 语句中增加一个普通的 WHERE 子句


28、Scalar函数
----- SQL Scalar 函数基于输入值，返回一个单一的值。

-----UCASE() - 将某个字段转换为大写
SELECT UCASE(name) AS site_title,url
FROM websites;--- 从 "Websites" 表中选取 "name" 和 "url" 列，并把 "name" 列的值转换为大写

-----LCASE() - 将某个字段转换为小写
SELECT LCASE(name) AS site_title,url
FROM WEBSITES;--- 从 "Websites" 表中选取 "name" 和 "url" 列，并把 "name" 列的值转换为小写

-----MID() - 从某个文本字段提取字符，MySql 中使用
SELECT MID(name,1,4) AS ShortTitle
FROM websites;--- 从 "Websites" 表的 "name" 列中提取前 4 个字符

-----LEN() - 返回某个文本字段的长度
SELECT name,LENGTH(url) as LengthofURL
FROM websites;--- 从 "Websites" 表中选取 "name" 和 "url" 列中值的长度

-----ROUND() - 对某个数值字段进行指定小数位数的四舍五入
SELECT ROUND(1.289,1); ---返回参数X(即1.289)的四舍五入的有 D(即1) 位小数的一个数字。如果D为0，结果将没有小数点或小数部分

-----NOW() - 返回当前的系统日期和时间
SELECT name,url,now() AS date
FROM websties;--- 从 "Websites" 表中选取 name，url，及当天日期

-----FORMAT() - 格式化某个字段的显示方式
SELECT FORMAT(column_name,format) FROM table_name;

SELECT name,url,DATE_FORMAT(Now(),'%Y-%m-%d') AS date
FROM websites;--- 从 "Websites" 表中选取 name, url 以及格式化为 YYYY-MM-DD 的日期


29、正则表达式

^
匹配输入字符串的开始位置。如果设置了 RegExp 对象的 Multiline 属性，^ 也匹配 '\n' 或 '\r' 之后的位置。
$
匹配输入字符串的结束位置。如果设置了RegExp 对象的 Multiline 属性，$ 也匹配 '\n' 或 '\r' 之前的位置
.
匹配除 "\n" 之外的任何单个字符。要匹配包括 '\n' 在内的任何字符，请使用象 '[.\n]' 的模式。
[...]
字符集合。匹配所包含的任意一个字符。例如， '[abc]' 可以匹配 "plain" 中的 'a'
[^...]
负值字符集合。匹配未包含的任意字符。例如， '[^abc]' 可以匹配 "plain" 中的'p'
p1|p2|p3
匹配 p1 或 p2 或 p3。例如，'z|food' 能匹配 "z" 或 "food"。'(z|f)ood' 则匹配 "zood" 或 "food"
*
匹配前面的子表达式零次或多次。例如，zo* 能匹配 "z" 以及 "zoo"。* 等价于{0,}。
+
匹配前面的子表达式一次或多次。例如，'zo+' 能匹配 "zo" 以及 "zoo"，但不能匹配 "z"。+ 等价于 {1,}。
{n}
n 是一个非负整数。匹配确定的 n 次。例如，'o{2}' 不能匹配 "Bob" 中的 'o'，但是能匹配 "food" 中的两个 o
{n,m}
m 和 n 均为非负整数，其中n <= m。最少匹配 n 次且最多匹配 m 次。

SELECT name
FROM person_tb1
WHERE name REGEXP '^st';--- 查找name字段中以'st'为开头的所有数据

SELECT name
FROM person_tb1
WHERE name REGEXP 'ok$';--- 查找name字段中以'ok'为结尾的所有数据

SELECT name
FROM person_tb1
WHERE name REGEXP 'mar';--- 查找name字段中包含'mar'字符串的所有数据

SELECT name
FROM person_tb1
WHERE name REGEXP '^[aeiou]|ok$';--- 查找name字段中以元音字符开头或以'ok'字符串结尾的所有数据



30、事务
--- 特性：原子性，稳定性，隔离性，可靠性

BEGIN 开启一个事务
ROLLBACK 事务回滚
COMMIT 提交事务


31、ALTER
--- 使用ALTER 命令修改数据表名或者数据表字段。

ALTER TABLE websites DROP name;--- 删除websites表的name字段，如果数据表只剩下一个字段则无法使用DROP来删除字段

ALTER TABLE websites ADD name varchar(255) AFTER id;--- 在websites表中添加 name字段，并规定name字段的位置在id字段的后面。

ALTER TABLE websites MODIFY name varchar(254);--- 使用 MODIFY 把字段 name的类型从 VARCHAR(255) 改为 VARCHAR(254)

ALTER TABLE websites CHANGE name newName varchar(255);--- 在 CHANGE 关键字之后，紧跟着的是你要修改的字段名，然后指定新字段名及类型

ALTER TABLE websites
MODIFY O_id BIGINT NOT NULL DEFAULT 100;--- 指定字段 O_id为 NOT NULL 且默认值为100

ALTER TABLE websites
ALTER O_id SET DEFAULT 1000;--- 使用 ALTER 来修改字段的默认值

ALTER TABLE websites 
ALTER O_id DROP DEFAULT;--- 使用 ALTER 命令及 DROP子句来删除字段的默认值,删除默认值后该字段默认值为NULL

ALTER TABLE websites RENAME TO newWebsites;--- 将数据表 websites重命名为 newWebsites;

ALTER TABLE websites engine=myisam;--- 修改存储引擎：修改为myisam

ALTER TABLE websites drop foreign key fk_name;--- 删除外键约束：fk_name 是外键别名

还可以用来创建及删除索引！记住：ALTER 是用来管理数据库表的！




