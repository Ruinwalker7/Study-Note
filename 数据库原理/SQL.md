# SQL

## DQL语句

```mysql
show databases;
create database bjpower;
use bjpower;
```



### 基础

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

select distinct job from emp;

distinct只能出现在所有字段的最前方，表示联合起来去除重复记录



### 条件查询

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

- like(模糊查询)

%匹配任意多个字符

_下划线：任意一个字符

```mysql
//找出名字中有O的
select * from emp where name like '%O%';
//首字母为F
select * from emp where name like 'F%';
//找第二个字母为R
select * from emp where name like '_R%';
//查特殊字符，\转义字符
```



### 排序

```mysql
select
	ename,sal
from 
	emp
where
	sal between 2000 and 4000
order by
	sal;//默认升序，降序加desc
//顺序不能改变
```

### 模糊查询

``` sql
WHERE name LIKE '%k' #以k结尾的串
WHERE name LIKE '%oo%' #包含oo的元素
WHERE name LIKE '_oogle' #除了第一个字母，后面为oogle的
```





### *分组查询

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



### 多表查询

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

```mysql
select
	e.name,d.dname
from
	emp e (outer)right join dept d
on
	e.deptno = d.deptno;
```



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



### 选择最前面几行

```sql
SELECT TOP 10 #sql server用法
LIMIT 5 #mysql 用法
```



限制查询数量，分页查询使用

限制的优先级最后

### SQL函数

`having`函数：

出现的原因是where语句不能够和合计函数一起使用



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



#### 多行处理函数（分组函数 最后输出一行）

##### 	max

##### 	min

##### 	count

count(具体字段) 统计具体字段下不为null的行数

count(*) 统计所有个数

##### 	sum



## DML语句

### 删除数据

```sql
DELETE 表名 #删除所有数据
DELETE sales WHERE title_id IN 
(SELECT title_id  FROM titles WHERE type = 'business')
```

### 插入数据

```sql
INSERT [INTO] 表名[(字段名[,字段名]...)]  VALUES(常量[,常量]...) 
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

float

date

datetime



### 删除表：

drop table if exist t_student //表不存在的时候也不会报错

### 插入

INSERT into 表名(字段1，字段2，字段3...) values(值1，值2，值3)

**一一对应**

INSERT语句只插入，执行后一定添加一条记录





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

