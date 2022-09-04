---
title: mysql基础
comments: true
copyright: true
tags:
  - sql
  - mysql
categories:
  - mysql
abbrlink: 8e40d0ad
date: 2022-08-29 20:18:48
---

总结
	有关数据库的DDL操作
	- SHOW DATABASES;
	-CREATE DATABASES 数据库名;
	-DROP DATABASES [IF EXISTS]数据库名;
	-USE 数据库名;
	
	有关数据表的DDL操作
	-SHOW TABLES;
	-DESC 表名;
	-CREATE TABLES 表名(......);
	-DROP TABLE 表名;
	-ALTER TABLE  表名;
	
	数据表的列类型
	-数值类型:INT,FLOAT
	-日期类型:DATE,TIME
	-字符串类型:CHAR,VARCHAR,TEXT

一,
库操作
①show databases;  查询当前用户下所有数据库

②create database 数据库名;  创建数据库

③use 数据库名;    打开库(包括切换其他数据库)

④show tables;     显示当前数据库下所有的数据表

⑤drop database 数据库名;  删除数据库

⑥desc 表名;(dos下)      显示表结构

float(7,2)-------整数部分99999     99999.99

char(4):  不可变字符串长度3位
varchar(4),可变数据长度

二,
表操作
①create table 表名(id int);   建表
 create table demo3(id int,username varchar(20),userpass varchar(20),ctime datetime);


float(7,2)---- 整数值99999  最大值99999.99

char(4)
abc
verchar(4)
ab

②drop table 表名;      删除数据表

③alter 
添加字段
alter table 表名 add 字段名 数据类型;	[单字段]     
alter table demo1 add name varchar(20);	

alter table 表名 add(字段1 数据类型,字段2 数据类型...);	[多字段]   
alter table demo1 add(dates date,flag int);

insert into 表名 values(字段1值,,字段2值)

⑤drop
alter table 表名 drop 字段名;    删除字段

④modify
修改字段 (原表中已存在的,不修改列名)
alter table 表名 modify 字段名 字段类型;		[单字段] 
alter table demo1 modify flag char(1);

alter table 表名 modify 字段名1 字段类型1,modify 字段名2 字段类型2...;		[多字段] 
alter table demo1 modify flag char(1);

⑥
重命名字段
alter table 表名 change 旧字段名 新字段名 字段类型;

重命名表名
alter table 表名 rename 新表名;
rename table 表名 to 新表名;

删除表数据(表截断),保留结构,清除表中所有数据,不清选择性删除,不能回退
truncate 表名;       

三,约束
数据库约束概念:在数据表上强制执行的效验规则,保证了数据的完整性
(唯一约束,主键约束,外键约束会自动创建索引,非空约束不会,)

非空约束 :
建立数据表字段中使用或者在修改数据表字段中使用 ,      格式: 字段名 字段类名 约束名 
不等于null :空字符串,0,'null',null不区分大小写,主键或非空不为null
创建,修改
create table 表名(字段名 字段类型 not null);     不填可默认为null
create table 表名(id int not null);

alter table 表名 modify 字段名 字段类型 约束名;
alter table demo1 modify url text not null;

alter table 表名 drop 字段名;    删除字段
alter table demo drop id;


唯一约束:
建立在not null 前
.修改,
不出现重复值,允许出现多个NULL,同一张表可有多个唯一约束,可由多列组合而成,会为之建立对应的索引,若不给唯一约束起名.该唯一约束默认与列名相同,
alter table 表名 modify 字段名 字段类型 unique;
alter table demo modify id int unique;

alter table 表名 drop 字段名;      删除唯一约束 (必须有多个字段)
alter table demo drop id;

主键约束
相当于 非空约束 + 唯一约束   唯一确定一行记录的字段或字段组合
注意.一张数据表中只能有一个主键
创建
create table 表名(字段名1 字段类型1 primary key,字段名2 字段类型2);
create table demo5(id int primary key,name varchar(20));

alter table 表名 drop primary key; 删除主键


alter table 表名 modify 字段名 字段类型 primary key;
alter table demo5 modify id int primary key; 修改主键 [将非主键字段修改为 主键字段]


create table 表名(字段名1 字段类型1,字段名2 字段类型2,primary key(字段名1,字段名2 ));
create table demo6(id int,name varchar(20),primary key(id,name)); 两个字段共用一个主键.复合主键


create table 表名(主键名 主键类型 primary key auto_increment,字段名2 字段类型2);
create table demo6(id int primary key auto_increment,name varchar(20));主键自增,在primary key 后加上auto_increment


insert into 表名(字段名) values(字段值); [主键自增时增加字段内容]
insert into demo6(name) values('zs');

外键 约束([一对一,可选择任意一方来增加外键列,只要为外键列增加唯一约束就可],[一对多,在多的一端增加外键列])
有外键的为从表,没有的为主表,保证两个数据表间的参照关系
创建
先创建主表
create table 主表名(主键名 主键类型 primary key,字段名2 字段类型2);
create table dept(deptno int primary key,dname varchar(20));

再建从表
create table 从表名(从表主键名 从表主键类型 primary key,字段名2 字段类型2,主表主键名 主表主键类型,constraint 从表名_fk foreign key(主表主键名) references 主表名(主表主键名));    从表名_fk:自定义的约束名  
create table emp(empno int primary key,ename varchar(20),sal int,deptno int,constraint emp_fk  foreign key(deptno) references dept (deptno));

删除外键约束
①删除外键
alter table 从表名 drop foreign key 约束名;
alter table emp drop foreign key emp_fk;emp_fk,约束名

②删除外键索引 
alter table 从表名 drop index 约束名;
alter table emp drop index emp_fk;

增加外键
alter table 从表名 add foreign key(主表主键) references 主表名(主表主键名);references 参照表及其主键(主表的主键)
alter table emp add foreign key(deptno) references dept(deptno);


检查约束:保证数据的输入校验规则(oracle支持,mysql 不支持)
create table 表名(id int check(id in (1,2)));
插入数据时只能输入1或2
create table 表名(age int check(age>8));
插入数据时只能是大于8的数据

show create table 表名;查询建表结构

DML语句,数据操作语言
插入,修改,删除语句

插入数据:向新行中写入insert
insert into 表名(字段名...) values(值...)  指定插入字段,如果字段名没写,默认值为NULL
insert into demo1(id,name,url) values(1,'zs','test');
insert into demo1(name,url) values('zs','test');

insert into 表名 values(值...)  默认插入所有字段
insert into demo1 values(1,'zs','test');

插入多条记录(行)
insert into 表名 values(值...),(值...);
insert into demo1 values(4,'zs','test'),(5,'aa','test');

修改/更新数据update
update 表名 set 需要修改的字段 = 新值;

[单个修改]
[多条记录]update 表名 set 需要修改的字段 = 新值;
update demo1 set url = 'a';

[多个字段值修改]
update 表名 set 需修改的字段1 = 新值1,需修改的字段2 = 新值2;

[根据条件修改]
update 表名 set 需要修改的字段 = 新值 where 条件;
update demo1 set name = 'test' where id = 4;

删除delete from 
[单条删除]
delete from 表名 where 条件;
delete from demo1 where name = 'zs';
[多条删除]
delete from 表名;
delete from demo1;


DQL数据库查询语言
查询
select 字段列表 from 表名;[查询指定行数据]
select name from demo1;
select * from 表名;[查询所有行数据]
select * from demo1;

算术运算符:+-*/
为字段名起别名 格式:字段名 as 别名(使用双引号)
select name,sal + 500 as "sals" from demo1;
注意:as可以省略,双引号可以省略(在别名中出现空格时不能省略)
select name,sal + 500 sals from demo1;

去重:distinct
a,单字段去重
select distinct 字段名 from 表名;
b,组合字段去重
select distinct 字段名1,字段名2... from 表名;


not > and > or

limit 取数据记录
limit n -----记录数
limit n,m n---起始索引  (从0开始) m----记录数

大多数 用于分页
select * from 表名 limit n;

数据排序
格式: order by 字段名;(查询 语句 结尾)
---升序:asc[默认]
select * from 表名 order by 字段名;
eg:select * from demo1 order by sal asc; 按工资升序排列
---降序 :desc
select * from 表名 order by 字段名 desc;
eg:select * from demo1 order by sal desc; 按工资 降序排列 
---组合排序
select * from demo1 order by sal,name desc;按照第一个字段排序,如果有重复数据则按照第二个字段排序
select * from 表名 order by 字段名1 排序类型1,字段名2 排序类型2;

---别名排序
select 字段名1,对字段名2修改 字段名2修改的别名 from 表名 order by 字段名2修改的别名;一般情况是为生成的新字段数据,执行排序操作
eg: select name,sal+200 sals from demo1 order by sals;

索引
作用:用于提高数据表查询效率
创建索引:
自动:由数据库自动完成,通过创建主键约束 ,唯一约束,外键约束会自动建立索引
手动:
 create index i_name on demo1(name);
格式:create index 索引名 on 表名(字段...);
create index i_us on demo1(url,sal);组合索引

删除索引
drop index 索引名  on 表名;
drop index i_us on demo1;

alter table 表名 drop index 索引名
alter table demo1 drop index i_name;

函数
字符串
concat('a','b');连接ab.一般是select查询
数值
mod(1,3);1/3的余数
select ceil(0.8);1 ----ceil向上取整   ceil(-0.8);0
select floor(0.8);0 ---floor向下取整  floor(-0.8);-1
截断
select truncate(3.21,2);---3.21
select truncate(3.21,1);---3.2
当前日期
select curdate();
当前时间
select curtime();
当前时间日期
select now();
一般计算年龄:
select dates,year(now())-year(dates)  age from demo1;


select ifnull(1,2);1
select ifnull(数据1,数据 2);如果数据1为null,返回数据2,否则返回数据1

select nullif(3,2);3
select nullif(数据1,数据2);如果数据1和数据2相等,返回null,否则返回数据1

select name,sal,if(sal,'有值','空值') result from demo1;
if(数据1,数据2,数据3);如果数据1 为true,返回数据2,否则返回数据3

select isnull(sal) from demo1;0表示false.1表示true(是不是空值,如果是空值true,不是空值false)
isnull(数据1);判断数据1是否为null,如果为null返回true,否则返回false

select version();显示数据库当前版本
select database();显示当前数据库名
select password('123');123字符password加密的方式
select md5('123');字符123md5加密的方式

多行函数(聚合函数)   除 count外 都会 忽略null的值 
avg():平均值
count():统计行数
max():求最大值
min():求最小值
sum():求和
select avg(sal),max(sal),min(sal),sum(sal) from demo1;

where + group by + order by

数据分组 group by
select deptno,avg(sql) from emp group by deptno;从员工 表中依据部门编号分组查询部门编号 ----分组查询

分组排序
select deptno,(ename,)avg(sql) avgs from emp group by deptno order by avgs desc;

where 和having 的区别
where 只能用在表名后
having用在group by 后

分组排序 加条件
查询平均工资大于1500的部门
select deptno,avg(sql) avgs from emp group by deptno having sal > 1500  order by avgs desc;

select deptno,sum(sal) sums from emp where sal > 1500 group by deptno; 

select deptno,sum(sal) sums from emp where sal > 1500 group by deptno having sums >3000;

select deptno,sum(sal) sums from emp where sal > 1500 group by deptno having sums >3000 order by sums;
{	
	DQL:数据库查询语言
	主要由SELECT . WHERE,ORDER BY,GROUP BY,和HAVING关键字构成

	DML:数据操作语言
	主要由insert,update,和delete三个关键字完成,对数据库记录修改
	
	DDL:数据定义语言
	主要由create,alter和drop和truncate四个关键字完成
	
	DCL:数据控制语言
	主要由grant和revoke关键字完成
	
	TPL:事务处理语言
	主要由commit,rollback,transaction关键字完成
}
