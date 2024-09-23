---
title: python面试题1.md
comments: true
copyright: true
tags:
  - python
  - 面试题
categories:
  - 面试题
abbrlink: 56c4f7dd
date: 2024-09-23 17:33:05
---

1, 线程，进程，协程

线程：程序执行中一个单一的顺序控制流程，是程序执行的最小单元，一个进程可以有一个或多个线程。

进程：是程序执行的一次过程，是临时的，有生命周期的。任何进程都可以同其他进程一起并发执行，是系统进行资源分配，调度的一个独立单位，由程序，数据和进程控制块三部分组成。

协程：在执行函数 A 时可以随时中断去执行函数B，然后中断函数 B继续执行函数 A,协程是一个线程在执行。



2，GIL 锁

全局解释器锁，同一进程加入有多个线程运行，一个线程在运行python程序时会霸占python 解释器，也就增加了一把锁。使该进程中的其他线程无法运行，等该线程运行完成之后其他线程才能运行。



3，多进程，多线程，进程池，线程池

多进程：python 提供了multiprocessing模块来支持多进程编程。

可以通过Process类来创建进程，并使用Queue、Pipe等数据结构来实现进程间通信。同时，Python中还有一些其他的并行编程库，如concurrent.futures和joblib，它们也提供了更方便的接口来实现并行计算。

进程池：创建适当的进程放入进程池,等待待处理事件,当处理完事件后进程不会销毁,仍然在进程池中等待处理其他事件,直到事件全部处理完毕.进程退出。

多线程：python 提供 threading 模块来支持多线程编程。

线程池: python的concurrent.futures模块提供了线程池和进程池的支持,可以用于并发处理I/O操作.



4，线程安全

线程安全是指在多线程环境中，多个线程同时访问共享资源时，保证程序的行为和结果是预期的。线程安全的代码能够避免竞态条件，死锁等并发问题，确保数据的一致性和正确性。

实现方式：

1,锁机制：使用锁（互斥锁，读写锁）来保护共享资源，确保同一时间只有一个线程可以访问该资源。

2.线程本地存储：每个线程都有自己独立的存储空间，避免共享资源的竞争。



5，线程锁

多个线程共享公共资源时,由于使用了线程锁,每个线程在修改共享资源时都会先获取锁,确保只有一个线程对公共资源进行处理,避免多个线程同时修改共享资源而导致的竞态条件问题.



6，死锁

即某个线程获取锁后无法释放锁,导致其他线程无法继续执行.死锁的解决方法:①添加超时时间②银行家算法(让锁按预期上锁和解锁)



7, 异步

asyncio， 通过使用async/await语法和事件循环来实现非阻塞的并发任务

使用asyncio库的协程特性来实现高效的并发编程，通过协程的方式可以避免线程切换的开销。



8，pandas

pandas 主要用于

​	1，数据读取，数据保存，

```
#pd.read_csv(), pd.read_excel(), pd.read_json()
#df.to_csv(), df.to_excel(), df.to_json()
# 读取 CSV 文件
df = pd.read_csv('data.csv')

# 保存为 Excel 文件
df.to_excel('output.xlsx', index=False)

```

​	2，数据过滤，选择

​	•	**按列选择**：df['ColumnName'] 或 df.ColumnName

​	•	**按行选择**：

​			•	**标签索引**：df.loc[]，基于行标签。

​			•	**位置索引**：df.iloc[]，基于行位置。

```
# 选择单列
ages = df['Age']

# 选择多列
subset = df[['Name', 'Age']]

# 按条件过滤
adults = df[df['Age'] >= 18]

# 使用 loc 按标签选择
row = df.loc[0]

# 使用 iloc 按位置选择
row = df.iloc[0]
```

​	3，数据清洗与处理

​		•	**处理缺失值**：

​			•	检测缺失值：df.isnull()

​			•	删除缺失值：df.dropna()

​			•	填充缺失值：df.fillna(value)

​		•	**数据类型转换**：df['ColumnName'] = df['ColumnName'].astype(new_type)

​		•	**重命名列**：df.rename(columns={'OldName': 'NewName'})

   4， 数据排序

​	•	**按值排序**：df.sort_values(by='ColumnName', ascending=True)

​	•	**按索引排序**：df.sort_index()

  5，数据添加与删除 

​	•	**添加列**：df['NewColumn'] = values

​	•	**删除列**：df.drop('ColumnName', axis=1)

​	•	**添加行**：df.append(new_row, ignore_index=True)

​	•	**删除行**：df.drop(index)

  6， 数据分组与聚合

​	•	**分组操作**：df.groupby('ColumnName')

​	•	**聚合函数**：sum(), mean(), count(), max(), min()

```
# 按性别分组，计算平均年龄
df.groupby('Gender')['Age'].mean()
```

​	7，合并与连接

​			•	**合并数据集**：pd.merge(df1, df2, on='Key')

​			•	**连接数据集**：pd.concat([df1, df2], axis=0)（行合并），axis=1（列合并）

​			•	**加入操作**：df1.join(df2, on='Key')

   8，数据透视表

​			•	**创建透视表**：pd.pivot_table(df, values='ValueColumn', index='IndexColumn', columns='ColumnsColumn', 				            aggfunc='mean')

```
# 按性别和部门计算平均薪资
pd.pivot_table(df, values='Salary', index='Department', columns='Gender', aggfunc='mean')
```

   9，应用函数

​	•	**逐元素操作**：df['ColumnName'].apply(func)

​	•	**整列或整行操作**：df.apply(func, axis=0)（列），axis=1（行）

​	•	**映射操作**：df['ColumnName'].map(mapping_dict)

```
# 计算年龄的平方
df['Age_Squared'] = df['Age'].apply(lambda x: x ** 2)

# 将性别映射为数字
gender_map = {'Male': 1, 'Female': 0}
df['Gender_Num'] = df['Gender'].map(gender_map)
```

​	10，时间与日期处理

​	•	**解析日期**：pd.to_datetime(df['DateColumn'])

​	•	**提取日期属性**：df['DateColumn'].dt.year, df['DateColumn'].dt.month

​	•	**日期差计算**：df['EndDate'] - df['StartDate']

​	 11，数据去重

​		•	**检测重复**：df.duplicated()

​		•	**删除重复**：df.drop_duplicates()

​	 12，窗口函数

​		•	**滚动计算**：df['ColumnName'].rolling(window_size).mean()

​	    •	**累积计算**：df['ColumnName'].expanding().sum()

​      13， 性能优化

​		•	**设置合适的数据类型**：使用 astype() 将数据转换为更节省内存的类型。

​		•	**使用矢量化操作**：避免使用循环，多利用 pandas 内置函数。

​		•	**使用** categorical **类型**：对于重复值多的字符串列，转换为分类类型可节省内存。



9，nosql, hadoop, spark

处理海量数据,关键在于海量存储和快速查询,技术选型包含分布式数据库,NoSQL数据库,内存数据库和大数据处理框架(eg:Hadoop, Spark). 关键技术和策略包括数据分片(Sharding),索引优化,数据压缩,缓存机制和并行处理,使用合适的数据结构和算法来减少资源消耗.



10，数仓，宽表

宽表： 指业务主题相关的指标，维度，属性关联在一起的一张数据库表。将多个数据源的数据整合到一张表当中。eg:用户信息，订单信息，行为日志表等数据可以整合到一张宽表。

数仓：数据仓库，是一个面向主题的，集成的，非易失性的，随时间变化的数据集合。数仓存储来着多源系统的数据，并经过清洗，转换和集成，支持复杂查询和分析。

数仓通常采用星型或雪花模型来设计数据模型，包括事实表和维度表。

事实表：存储业务中的度量数据（eg：销售额，订单数）

维度表：存储描述业务实体的属性数据（eg:时间，地点，产品）

宽表: 适用于快速数据查询和分析、报表生成和机器学习模型的输入。通过整合多个相关数据源，创建一个包含丰富信息的单表。

数据仓库: 适用于支持企业的决策系统，进行复杂查询和分析。通过主题化、集成化的数据存储，提供统一的视图，支持长时间跨度的历史数据分析。



11，nosql

数据类型：面向文档，健值，图表，列导向



数据库水平和垂直缩放的区别

水平扩展意味着您通过将更多机器添加到资源池中来进行扩展，(mongodb)

垂直缩放意味着您通过向现有机器添加更多功率（CPU,RAM）来进行缩放,(mysql-Amzon RDS)



12，hadoop 和 Spark 区别。

hadoop:主要是存储和批处理大规模数据。

spark: 主要用于高效的实时处理，迭代计算或交互式查询。



13, hadoop组件

1）HDFS集群：负责海量数据存储，集群中的主要角色NameNode/DataNode

2）YARN集群：负责海量数据运算时的资源调度,集群中的ResourceMamager/NodeManager

3）MapReduce: 它其实是一个应用程序开发包
