---
title: mysql面试题
comments: true
copyright: true
tags:
  - mysql面试题
  - 面试题
categories:
  - 面试题
abbrlink: c45230cb
date: 2022-08-29 20:17:10
---

1, mysql索引
[mysql索引的详细说明](https://www.cnblogs.com/jiuya/p/10023483.html)
普通索引:
是最基本的索引,没有任何限制
唯一索引:
索引列的值必须唯一,但允许有空值,如果是组合索引,则列值的组合必须唯一
主键索引:
一个表只能有一个主键,不允许有空值,一般在建表的同时创建主键索引
组合索引:
指多个字段上创建的索引,只有在查询条件中使用了创建索引时的第一个字段,索引才会被使用.使用组合索引时遵循最左前缀集合
全文索引:
主要是用来查找文本中的关键字,而不是直接与索引中的值相比较.



2, 数据库优化
①,如有条件,数据可以存放于redis,读取速度快,
②,建立索引,外键等
③使用联合来代替手动创建的临时表
④使用事物
⑤使用锁定表
⑥选取合适的字段属性
⑦使用连接代替子查询



3,创建数据库的四大原则?
答:
事物: 是数据库操作的最小工作单元,是一组不可再分割的操作集合
原子性: 整个程序中的所有操作,要么全部完成,要么全部不完成,不可能停滞在中间某个环节
一致性: 倘若事物操作失败,则回滚事物时,与原状态一致.
隔离性: 当在进行事物操作时, 其他事物的操作不可能影响当前事物操作,事物与事物之间是隔离的,互不干扰,干完再整合.
持久性: 事物的操作具有持久性, 事物结果一旦写入数据库,在不改动的情况下,数据库一直都是这个数据.



4,orm数据库?
orm数据库:称为对象–关系映射,主要实现模型对象到关系数据库数据的映射,把数据表中的每一条记录映射为一个模型对象,



5,数据表student有id,name,score,city字段,其中name中的名字可有重复,需消除重复行,请写sql语句
答:select distinct name from student;



6, 表合并
①
#sql union 不允许重复的值
select 字段名 from 表名 union select 字段名2 from 表名2

#sql union all 允许重复的值
elect 字段名 from 表名 union all select 字段名2 from 表名2

②
insert into 表1 select * from 表2



7,数据库优化查询方法
答:外键,索引,联合查询,选择特定字段等等



8,写5条常用sql语句
答:show databases;
show tables;
desc tb_name;
select * from tb_name
delete from tb_name where id = 5;
update tb_name set name=’xue’,age=23 where id = 5;



9, [mysql](https://fies09.github.io/article/8e40d0ad.html?highlight=mysql)
排序: order by
分组: group by
条件: where
where 和having 的区别
where 只能用在表名后
having用在group by 后



10,列出常见mysql数据存储引擎
InnoDB:支持事物处理,支持外键,支持崩溃修复能力和并发控制.
MyISAM:插入数据快,空间和内存使用比较低,
MEMORY:所有数据都在内存中,数据的处理速度快.但是安全性不高



11,Mysql数据库的隔离级别
①Read Uncommitted(读取末提交内容)
在该隔离级别,所有事物都可以看到其他末提交事物的执行结果,本隔离级别很少用于实际应用，因为它的性能也不比其他级别好多少。读取未提交的数据，也被称之为脏读（Dirty Read）。
②Read Committed（读取提交内容）
这是大多数数据库系统的默认隔离级别（但不是MySQL默认的）。它满足了隔离的简单定义：一个事务只能看见已经提交事务所做的改变。这种隔离级别 也支持所谓的不可重复读（Nonrepeatable Read），因为同一事务的其他实例在该实例处理其间可能会有新的commit，所以同一select可能返回不同结果。
③Repeatable Read（可重读）
这是MySQL的默认事务隔离级别，它确保同一事务的多个实例在并发读取数据时，会看到同样的数据行。不过理论上，这会导致另一个棘手的问题：幻读（PhantomRead）。简单的说，幻读指当用户读取某一范围的数据行时，另一个事务又在该范围内插入了新行，当用户再读取该范围的数据行时，会发现有新的“幻影” 行。InnoDB和Falcon存储引擎通过多版本并发控制（MVCC，Multiversion Concurrency Control）机制解决了该问题。
④Serializable（可串行化）
这是最高的隔离级别，它通过强制事务排序，使之不可能相互冲突，从而解决幻读问题。简言之，它是在每个读的数据行上加上共享锁。在这个级别，可能导致大量的超时现象和锁竞争。



12, mysq与redis,mongodb的区别
mysql: 关系型数据库,数据保存在磁盘中,速度慢
redis: 内存型非关系数据库,数据保存在内存中, 速度快
mongodb: 非关系型数据库,虚拟内存加持久化存储,有独特的mongodb查询方式



13, join
inner jon 内连接
内连接又叫等值连接，此时的inner可以省略。
获取两个表中有匹配关系的记录，即两表取交集
select * from a join b on a.'name' = b.'name'
left join 左连接
以左表为基础，获取匹配关系的记录，如果右表中没有匹配项，NULL表示
select * from a left join b on a.'name'=b.'name'
right join 右连接
以右表为基础，获取匹配关系的记录，如果左表中没有匹配项，NULL表示
select * from a right join b on a.'name'=b.'name'
