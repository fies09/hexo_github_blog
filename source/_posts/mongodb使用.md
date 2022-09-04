---
title: mongodb使用
comments: true
copyright: true
tags:
  - sql
  - mongodb
categories:
  - sql
abbrlink: a7b87908
date: 2022-08-29 20:15:13
---

window下mongodb的使用:

MongoDB数据库:(两种方式一般使用第二种方便快捷)
***管理员***
一 ,使用流程:
1,启动服务器(连接):在bin目录下:mongod --dbpath “data目录”
2,访问(打开): http://localhost:27017/
3,在bin目录下输入mongo,进入数据库
3,进入:use admin (use进入,创建,切换)
3,db.shutdownServer() :关闭数据库(关闭)
3,访问(打开 ): http://localhost:27017/
{手动}


{自动}
也可直接打开:
二 ,升级为系统服务:
1, 解压zip到MongoDB64文件夹下 --> 创建data和log文件夹 -->在log文件夹下创建MongoDB.log文件--> 进入bin目录 --> 输入 mongod --dbpath “data目录” --logpath “MongoDB.log目录及名称” --install -serviceName “MongoDB”
将服务手动启动:net start MongoDB/net stop MongoDB
2,在bin目录下输入mongo,进入数据库
3,use 数据库名 :切换/创建数据库
4,db:查看当前所在的数据库
5,show dbs:查看当前MongoDB数据库中的所有数据库
6,在数据库中创建一个集合(表)movie并插入一条记录
db.movie(表名).insert({name:”mymovie01”})(记录)
7,当数据库中没有数据库对象(数据)时,show dbs 不会显示
8,删除数据库:db.dropDatabase():删除当前所在的数据库
9,集合的创建(数据表的创建)(与上述区别,没插入数据)
db.createCollection(name)
10,show collections:显示所有集合
11,索引的元信息存储在每个数据库的system.indexes集合中,不能插入删除
12,集合的删除操作db.集合名.drop()
13,数据类型:
String:字符串(utf8)
Integer:整数
Boolean:布尔型:(true/false)
Double:浮点型(小数)
Min/Max keys:最低和最高值比较
Arrays:多个值放到一个key中
Timestamp:时间戳
Object:此数据类型用于嵌入式文件(1 v 1,1 v多,多 v 1, 多 v 多)
Null:空值
Symbol:通常保留给特定符号类型的语言,此数据类型用于字符串相同
Date:日期
Object ID:文档(表,集合)ID
Binary data :二进制数据
Code:用于存储到文档中的js代码
Regular expression:正则表达式

文档对象的增查更删:
增加:
①db.文档名.insert([{k1:v1},{k2:v2}])
②var docs = [{k1:v1},{k2:v2}]
db.文档名.insert(docs)
查看文件结构:db.文档名.find()

查:
db.集合名.find() :查询所有
db.集合名.findOne() :返回集合中的第一条文档数据
db.集合名.find().pretty() :结构化显示数据
条件查询:
db.集合名.find(k:”v”).pretty()  
小于:$lt  小于等于:$lte
大于: $gt     大于等于:$gte  
不等于:$ne   
db.集合名.find({k:{$ne:v}}).pretty()

AND/OR
AND
db.集合名.find({k1:"v1",k2:"v2"}).pretty()
OR
db.集合名.find({$or:[{k1:"v1"},{k2:"v2"}]}).pretty()
大于并且..或.. 
db.集合名.find({k1:{$gt:v1},$or:[{k2:"v2"},{k3:"v3"}]}).pretty() 

更新(修改):
Criteria:更新操作条件,类似sql语句中的where子句
ObjNEW:更新的操作符::(如$,$inc...),也可以理解为sql update查询内set后面的
Upsert:如果不存在update的记录,是否插入objNEW,true为插入,默认是false,不插入
Multi:默认是false,只更新找到的第一条记录,如果这个参数为true,就把按条件查出来
多条记录全部更新
db.集合名.update({k1:"v1"},{$set:{k2:”要修改的值”}},false,true)

删除:
remove:db.infos.remove({k1:"v1"})
deletion criteria(可选):
justOne(可选):
投影(不显示0,1显示):db.infos.find({},{k1:0,k2:0,k3:0}).pretty()

限制记录limit()
只显示n个记录:db.infos.find().limit(n)
先跳过m条记录,然后显示n条记录:db.infos.find().limit(n).skip(m)

组合使用(限制记录+分页处理):n --> 显示文档个数  m --> 文档个数(页数 - 1)
Skip()+Limit():分页显示
db.infos.find().limit(n).skip(m)

排序:1:升序(小到大)  -1:降序(大到小)
db.infos.find().sort({key:-1}).pretty()
