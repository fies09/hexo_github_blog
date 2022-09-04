---
title: python关键字整理
comments: true
copyright: true
tags:
  - 关键字
categories:
  - python
abbrlink: 4321eec9
date: 2022-08-29 19:49:42
---

一,各个关键字代表的含义

abs():取元素绝对值
np.array():表示数组
np.argsort():返回元素排序的下标位置(针对索引)
np.sum():求和(针对数组)
np.sqrt():开平方
np.mean():求平均值
np.stu():求标准差
loadtxt:读取文件(属性如下)
delimiter = ",":分隔符
skiprows = 1:空除第一行
usecols=(17,13,2):分别使用第17,13,2列
c:collections
c.Counter(sortlabel[0:k]).most_common(1)[0][0]:出现次数最多的元素
.print:输出
ord():将字符转化为ASCII
char():将ASCII转换为字符串
bin():十 --- 二
oct():十 --- 八
hex():十 --- 十六
eval():十六 --- 十
int():其他 --- 十
list.sort():列表排序
str(字符串).split(','):以逗号分隔
%s:字符串 %d:整数
%f:小数
sorted(内容):所有可迭代的对象
list.sort(reverse=True):降序排列
content.decode("编码类型"):编码
content.encode("编码类型"):解码
+:同类型拼接
print("姓名：{0} 年龄：{1}".format('zs',age))
print("姓名:%s 年龄:%d" %('ct',35)) #%f[小数（浮点数）] %s [字符串]
%d [整数]

import os
os.system('cls'):清屏
os._exit(0):退出
choice.lower() == 'y':输入的字母为小写y,一般用于条件判断

import time
time = time.strftime("%Y%m%d%H%M%S",time.localtime()):当前时间
time.sleep(秒数):延迟
time.time():当前时间

continue:跳出循环,重复操作
break:跳出循环:进行下一步操作
end = "" :不换行,输出为一行

import getpass (暂时无法实现)
password = getpass.getpass('要输入的密码'):密码隐藏

arr.shape:查看数组几行几列
arr.dtype:数组的数据类型
ur.urlretrieve():将文件下载到本地进度条
.print:输出

map用法:
1,定义一个函数,
def square(x): #计算平方数
return x ** 2
map(函数名(square),一个或多个序列(列表))
#计算列表中的元素平方

2,使用lambda匿名函数
map(lambda x: x **2 ,序列(列表))
#计算列表中的元素平方

3,提供了两个列表.对相同位置的数据进行相加
map(lambda x,y:x+y,[1,3,1,4,5,2,1],[1,0,1,0,1,0,9])

enumerate:指定列表元素下标并输出,一般用于for循环
start指定下标开始位置,默认为0
eg:
seq = ['one','two','three']
for i , element in enumerate(seq,start = 2):
print(i,element)
#2 one
#3 two
#4 three

=:赋值
==:判断是否相等(在if中使用)

都和numpy有关
在灵活的包装器np.(add, sub, mul, div, mod, pow)算术运算符:+，-，*，/，%，**

1,数据类型
bool:布尔型(True,False)
int:整型(整数)
float:浮点型(小数)
complex:复数

2,进制转换
bin():将给的参数转化成二进制
oct():将给的参数转换成八进制
hex():将给的参数转换成十六进制

3,数学运算
abs():返回绝对值
divmode():返回商和余数
round():四舍五入
sum():求和
pow(a,b):求a的b次幂,如果有三个参数,则求完次幂后对第三个数取余
min():求最小值
max():求最大值

1,序列
1).列表和元组
list():将一个可迭代对象转换成列表
tuple():将一个可迭代对象转换成元组
2).相关内置函数
reversed():将一个序列翻转,返回翻转序列的迭代器
slice():列表的切片
3).字符串
str():将数据转化成字符串
bytes():把字符串转化成bytes类型
ord():输入字符找带字符编码的位置
chr():输入位置数字找出对应的字符
ascii():是ascii码中的返回该值,不是就返回u
repr():返回一个对象的string形式

2.数据集合
字典:dict创建一个字典
集合:set创建一个集合

3.相关内置函数
len():返回一个对象中的元素的个数
sorted():对可迭代对象进行排序操作(lamda)
lterable:可迭代对象
reverse:是否是倒叙,True:倒叙,False:正序
enumerate():获取集合的枚举对象

key:排序规则(排序函数)
all():可迭代对象中全部是True,结果才是True
any():可迭代对象中有一个是True,结果就是True
fiter():过滤(lamda)
map():会根据提供的函数对指定序列做出映射(lamda)
zip():用于将可迭代对象作为参数

和作用域有关:
locals():返回当前作用域中的名字
globals():返回全局作用域中的名字

和迭代器生成器相关:
range():生成数据
iter():获取迭代器,内部实际使用的是__iter__()方法来获取迭代器
next():迭代器向下执行一次,内部实际使用了__next__()方法返回迭代器的下一个项目

字符串类型代码的执行:
eval():执行字符串类型的代码,并返回最终结果
exec():执行字符串类型的代码
compile():将字符串类型的代码编码,代码对象能够通过exec语句来执行或者eval()进行求值

输入输出:
print():打印输出
input():获取用户输入的内容

内存相关:
hash():获取到对象的哈希值(int,str,bool,tuple)

文件操作相关:
open():用于打开一个文件,创建一个文件句柄

模块相关:
import():用于动态加载类和函数

帮助:
help():函数用于查看函数或模块用途的详细说明

调用相关:
callable():用于检查一个对象是否是可调用的,

查看内置属性:
dir():查看对象的内置属性,访问的是对象中的__dir__()方法

strip():去除指定字符/默认去除空格

1,分片[::]:起始,末尾(不包含),步长---->取末尾的前一个
[:]:取所有
[::-1]:从后往前取所有

2,表达式,运算符
表达式=操作数+运算符
运算符:7个
算术运算符:
+(加) -(减) *(乘) /(除) %(求余) //(整除)

比较运算符:= < >

逻辑运算符:and not or

成员运算符:in not in

身份运算符:is is not

#详细说明(代码实现位运算符)
位运算符: << 左移 >> 右移

异或:^

创建:virtualenv 虚拟环境名
开启: source activate

1，占位符

```
%d是整数的占位符，%f是小数的占位符（%.nf，n表示保留n位小数）,%s是字符串占位符
```

2，格式转换

- `int()`：将一个数值或字符串转换成整数，可以指定进制。
- `float()`：将一个字符串转换成浮点数。
- `str()`：将指定的对象转换成字符串形式，可以指定编码。
- `chr()`：将整数转换成该编码对应的字符串（一个字符）。
- `ord()`：将字符串（一个字符）转换成对应的编码（整数）

3，分支结构

if xx:

	pass

elif xx：

	pass

else:

 	pass

4, 循环结构

`for-in`循环，一种是`while`循环

while循环中使用：

`break`关键字来提前终止循环, 只能终止它所在的那个循环,在嵌套的循环结构中使用， 不执行

continue： 放弃本次循环后续的代码直接让循环进入下一轮，继续执行

5, 格式化输出:
print("zhangsan",2)
print("姓名:%s 年龄:%d" %(name,35)))
print("{0}".format("你好))
