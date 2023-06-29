# SQL

## DQL语句

```mysql
show databases;
create database bjpower;
use bjpower;
```



### SELECT

```sql
SELECT * FROM name;//查询所有字段，不要用，可读性差，效率低
INSERT INTO emp
VALUES(1002.'TOM');
//查询多个字段
select name,sexe from TABLE_NAME;
//起别名，不进行修改
select name ,sexe as sexes from table_name;
//字符串以单引号作为标准，Oracle里面
```

字段可以参加加减乘除运算



#### distinct关键字

```sql
select distinct job from emp;
```

distinct只能出现在所有字段的最前方，表示联合起来去除重复记录

作用于多列时组合评估重复项



#### TOP

选择最前面几行

```sql
SELECT top 10 name,continent FROM country;
#返回列name的前10行记录,只显示name和continent两列
SELECT top 50 percent name FROM country;
#返回列name的前百分之50的记录,只显示name一列
```





### INTO

使用结果集来创建新表。

- 注意:可以设置把选择的行存入临时表或者永久表.
- 局域临时表只有当前用户才能看到,使用INTO #TableName
- 不添加#号就是进入永久表,临时表在退出程序时会自动删除





### WHERE 条件查询

```mysql
select 
	字段
from
	表
where
	条件
```



条件有哪些：

- <小于

- <>或!= 不等于

- between ... and ... 在什么之间

- is null(is not null)   为null（不为null）

- and 与

- or 或

**and和or同时出现，and的优先级比or高，加括号改变优先级**

- in 包含，相当于范围 	

```mysql
select * from emp where job = 'Manager' or job = 'Salesman';
select * from emp where job in {'Manager','Salesman'};
```

- not 

否定 常用于is或in



#### 日期查询

- 日期列用法例子: Birthday > ‘2000-1-1’



#### 模糊查询

``` sql
WHERE name LIKE '%k' #以k结尾的串
WHERE name LIKE '%oo%' #包含oo的元素
WHERE name LIKE '_oogle' #除了第一个字母，后面为oogle的
```

- like(模糊查询)

%匹配任意多个字符

_下划线：任意一个字符

[]：[abcd] [a-d] 匹配其中任意一个字符

`[^]`：[\^abcd]  [^]  不匹配其中字符

```mysql
//找出名字中有O的
select * from emp where name like '%O%';
//首字母为F
select * from emp where name like 'F%';
//找第二个字母为R
select * from emp where name like '_R%';
//查特殊字符，\转义字符
```



### 聚合函数

聚合:聚合是预先计算好的数据汇总
聚合函数:对一组值计算并返回单一的计算结果



可以直接SELECT之后使用的 

- AVG
- SUM
- COUNT
- MAX
- MIN



可以使用`DISTINCT`先消除重复值

#### COUNT

- COUNT(ALL expression) 对组中的每一行都计算 expression 并返回非空值的数量
- COUNT(DISTINCT expression) 对组中的每一行都计算 expression 并返回唯一非空值的数量



### *分组查询 GROUP BY

先分组：

```mysql
	select
		...//分组字段可以写，然后是分组函数
	from
		...
	group by
		...//先分组在使用分组函数
	having//必须连在group by后面
		...//判断语句
		//能使用where别用having，除非如判断平均值的大小，才选择用where
```

**`group by`出现的情况下，`select`里面的字段一定要出现在`group by`中**

**空值单独一组**



- 如果使用 ALL 关键字，那么查询结果将包括由 GROUP BY 子句产生的所有组，即使某些组没有符合搜索条件的行。
- 没有 ALL 关键字，包含 GROUP BY 子句的 SELECT 语句将不显示没有符合条件的行的组。



### HAVING

- HAVING 子句是应用于结果集的附加筛选。再次的分组和聚类。逻辑上讲，HAVING 子句从中间结果集对行进行筛选，这些中间结果集是用 SELECT 语句中的 FROM、WHERE 或 GROUP BY 子句创建的。
- HAVING 子句通常与 GROUP BY 子句一起使用，尽管 HAVING 子句前面不一定必须有 GROUP BY 子句。
- 聚合函数判断只能在HAVING中使用



### 排序 ORDER BY

```mysql
select
	ename,sal
from 
	emp
where
	sal between 2000 and 4000
order by ASC#DESC
	sal;//默认升序，降序加desc
```



- 指定要排序的列。可以将排序列指定为列名或列的别名和表达式
- 可指定多个排序列,按先后嵌套排序
- ORDER BY 子句可包括未出现在此选择列表中的列
- **如果指定SELECT DISTINCT，或者SELECT语句包含UNION运算符，则排序列必定出现在选择列表中**



### 子查询

嵌套select语句

from子句中的子查询，可以将子查询到的结果当成临时的一张表

ex:查询最大值

```sql
SELECT 
    *
FROM
    table_a
WHERE
    p_postions = (SELECT MAX(p_postions) FROM table_a)
LIMIT 1;
```



子查询受以下条件的限制： 

- 通过比较运算符引入的子查询的选择列表只能包括一个表达式或列名称
- 如果外部查询的 WHERE 子句包括某个列名，则该子句必须与子查询选择列表中的该列在联接上兼容。
- 子查询的选择列表中不允许出现 ntext、text 和 image 类型
- 由于必须返回单个值，所以由无修改的比较运算符（指其后未接关键字 ANY 或 ALL）引入的子查询不能包括 GROUP BY 和 HAVING 子句。
- 包括 GROUP BY 的子查询不能使用 DISTINCT 关键字。
- 只有同时指定了 TOP，才可以指定 ORDER BY。
- 由子查询创建的视图不能更新。
- 按约定，通过 EXISTS 引入的子查询的选择列表由星号 (*) 组成，而不使用单个列名。由于通过 EXISTS 引入的子查询进行了存在测试，并返回 TRUE 或 FALSE 而非数据，所以这些子查询的规则与标准选择列表的规则完全相同。 



### 联接查询

#### 内连接 

##### 等值连接

```sql
//sql99
select
	e.ename , d.dname
from
	emp e
(inner) join
	dept d
on
	e.deptno = d.deptno
where
	...//sql99分离了连接和条件，更直观
```



##### 非等值连接

```mysql
select
	e.ename,e.sal,s.grade
from
	emp e
join
	salgrade s
on
	e.sal between s.losal and s.hisal;
```



##### 自连接

将一张表看成两张表

```
select
	e.empno 员工号,e.ename 员工名,b.ename 领导名 
from
	emp e
join
	emp b
on
	a.mgr = b.empno
```



#### 外连接

有一张表是主表

右外连接 右边为主表，主表一定要全写出来

（1）左连接：只要左边表中有记录，数据就能检索出来，而右边有的记录必要在左边表中有的记录才能被检索出来

（2）右连接：右连接是只要右边表中有记录，数据就能检索出来

（3）全连接：FULL OUTER JOIN 左右都显示出来

```mysql
select
	e.name,d.dname
from
	emp e (outer)right join dept d
on
	e.deptno = d.deptno;
```





### 处理函数

#### 单行处理函数

​	lower()换小写

​	upper()换大写

​	substr()取子串 substr(被截取的字符串，开始位置，长度)

​	concat()字符串拼接

​	trim()去空格

​	str_to_date 把字符串转换为日期

​	round四舍五入(round(被操作数，保留位数)保留位数如果为负，就是保留整数部分)

​	rand() 生成随机数默认小数，配合乘法和round可以构造

​	ifnull 将null转换为具体值

​		ifnull(数据，当成的值)

​	case ... when ... then ... when ... then ... else ... end 条件处理，类似switch 



## DML语句

### 删除数据

```sql
DELETE 表名 #删除所有数据
DELETE sales WHERE title_id IN 
(SELECT title_id FROM titles WHERE type = 'business')
```

假设为SELECT选择就好，选出来的都删掉了

### 插入数据

```sql
INSERT [INTO] 表名(字段名,字段名...)  VALUES(常量[,常量]...) 
```

- 如果没指定列的列表，值的顺序必须与表或视图中的列顺序一致
- 列表中**没有指定的所有列**都必须**允许 NULL值或者指定的默认值**等。也就是如果定义了字段列表，表中有些字段在插入语句中没有出现,则在这些字段上的值取空值NULL或默认值

如果插入值为空的四种情况

- 具有 IDENTITY 属性的标识列。使用下一个增量标识值。
- 有默认值。使用列的默认值。
- 具有 timestamp 数据类型。使用当前的时间戳值。
- 是可空的。使用空值。 



使用SELECT子查询可以将其他表的的值添加进入当前表，注意字段匹配



### 更新数据

更改表中的现有数据
语法：``UPDATE table_or_view SET column_name = expression [FROM table_source] WHERE search_condition``

如果没有WHERE语句则可能将所有列更新



## DDL操作

### 创建

create table 表名(字段名1 数据类型，字段名2 数据类型，字段名3 数据类型)

default语句可以设置默认值

### 数据类型

varchar

char

int

bigint

double

date

datetime

### 删除表：

drop table if exist t_student //表不存在的时候也不会报错

### 插入

INSERT into 表名(字段1，字段2，字段3...) values(值1，值2，值3)

**一一对应**

INSERT语句只插入，执行后一定添加一条记录





## 视图

```sql
CREATE VIEW [title_view] (name1,name2,...) AS 
SELECT [字段1] [字段2] ..
FROM [表名]
WHERE ...
```



## 查询过程（函数）

```sql
CREATE PROCEDURE [dbo].[p_second]
	@authorName varchar(100)
AS
BEGIN
	SELECT t.title,t.price,p.pub_name
	FROM titleauthor ta 
	INNER JOIN titles t ON t.title_id=ta.title_id 
	INNER JOIN authors a ON a.au_id = ta.au_id
	INNER JOIN publishers p ON t.pub_id = p.pub_id
	WHERE a.au_lname = @authorName or a.au_fname = @authorName
END
```

`CREATE PROCEDURE`和`ALTER PROCEDURE`的区别是一个是创造函数一个是修改函数



## 触发器

功能：

强化约束

跟踪变化



## 游标

概念：一种处理数据的方法，具有对结果集进行逐行处理的能力



游标的实现功能

- 允许对 SELECT 返回的表中的每一行进行相同或不同的操作，而不是一次对整个结果集进行同一种操作；
- 从表中的当前位置检索一行或多行数据；
- 游标允许应用程序对当前位置的数据进行修改、删除的能力；
- 对于不同用户对结果集包含的数据所做的修改，支持不同的可见性级别；
- 提供脚本、存储过程、触发器中用于访问结果集中的数据的语句。
  

声明：

```sql
DECLARE 游标名称 CURSOR       
[ LOCAL | GLOBAL ]                          --游标的作用域
[ FORWORD_ONLY | SCROLL ]                     --游标的移动方向
[ STATIC | KEYSET | DYNAMIC | FAST_FORWARD ]        --游标的类型
[ READ_ONLY | SCROLL_LOCKS | OPTIMISTIC ]          --游标的访问类型
[ TYPE_WARNING]                            --类型转换警告语句
FOR SELECT 语句                            --SELECT查询语句
[ FOR { READ ONLY | UPDATE [OF 列名称]}][,...n]      --可修改的列
```





# 多表查询

#### 连接方式

​	内连接：

​		等值连接

​		非等值连接

​		自连接

​	外连接：

​	全连接： 



#### 笛卡尔积现象 

解决方法，加条件

```
select
	emp.ename,dept.dname
from
	emp,dept
where
	emp.deptno = dept.deptno
```

