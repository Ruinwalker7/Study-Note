# MySQL

```mysql
show databases;
create database bjpower;
use bjpower;
```



语句

```sql
SELECT * FROM name;//查询所有字段，不要用，可读性差，效率低
INSERT INTO emp
VALUES(1002.'TOM');
//查询多个字段
select name,sexe from TABLE_NAME;
//起别名，不进行修改
select name ,sexe as sexes from table_name;
//字符串以单引号作为标准，Oracle里面z
```

字段可以参加加减乘除运算



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

​	

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

关键词顺序：

from

where

group by

select

order by



### distinct关键字

select distinct job from emp;

distinct只能出现在所有字段的最前方，表示联合起来去除重复记录



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



### limit

限制查询数量，分页查询使用

限制的优先级最后



### 表的操作

#### 创建

create table 表名(字段名1 数据类型，字段名2 数据类型，字段名3 数据类型)

default语句可以设置默认值

#### 数据类型

varchar

char

int

bigint

float

date

datetime



#### 删除表：

drop table if exist t_student //表不存在的时候也不会报错

#### 插入

insert into 表名(字段1，字段2，字段3...) values(值1，值2，值3)

**一一对应**

insert语句只插入，执行后一定添加一条记录
