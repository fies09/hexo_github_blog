---
title: web面试题
comments: true
copyright: true
tags:
  - web面试题
  - 面试题
categories:
  - 面试题
abbrlink: 45b9fa15
date: 2022-11-25 15:43:13
---

1, Django的MVT模式(MVC:M相当于model,V相当于template,C相当于view)
M： Model, 模型 与MVC中的M相同，负责对数据的处理
V： View, 视图 与MVC中的C类似，负责处理用户请求，调用M和T，响应请求
T： Template, 模板 与MVC中的V类似，负责如何显示数据（产生html界面）



2, get/post
GET在浏览器回退时是无害的，而POST会再次提交请求。

GET请求参数会被完整保留在浏览器历史记录里，而POST中的参数不会被保留。

GET请求在URL中传送的参数是有长度限制的，而POST没有。

GET参数通过URL传递，POST放在Request body中。



3,csrf

跨站请求伪造: 主要用于在html中设计token访问令牌



4,FBV/CBV

FBV:基于函数视图

CBV:基于类视图



5, django请求周期

（1）用户输入网址，浏览器发起请求
（2）WSGI（服务器网关接口）创建socket服务端，接受请求
（3）中间件处理请求
（4）url路由，根据当前请求的url找到相应的视图函数
（5）进入view，进行业务处理，执行类或者函数，返回字符串
（6）再次通过中间件处理相应
（7）WSGI返回响应
（8）浏览器渲染



6,uwsgi

web服务器(应用服务器),用于连接Web服务器和Web应用框架



7,中间件

是一个轻量低级别的插件系统,在全局变量改变django的输入与输出,在settings中注册,,例如csrf跨站请求伪造



8,内置组件

Admin: 对model中对应的数据表进行增删改查提供的组件
model：负责操作数据库
form：1.生成HTML代码 2.数据有效性校验 3校验信息返回并展示
ModelForm: 即用于数据库操作,也可用于用户请求的验证



9,悲观锁,乐观锁

乐观锁: 总是认为不会产生并发问题，每次去取数据的时候总认为不会有其他线程对数据进行修改，因此不会上锁，但是在更新时会判断其他线程在这之前有没有对数据进行修改

悲观锁: 总是认为不会产生并发问题，每次去取数据的时候总认为不会有其他线程对数据进行修改，因此不会上锁，但是在更新时会判断其他线程在这之前有没有对数据进行修改



10, JWT

第一次认证通过用户名密码，服务端签发一个json格式的token。后续客户端的请求都携带这个token，服务端仅需要解析这个token，来判别客户端的身份和合法性



11,cookie和session

1.cookie:
cookie是保存在浏览器端的键值对,可以用来做用户认证
2.session：
将用户的会话信息保存在服务端,key值是随机产生的自符串,value值时session的内容
依赖于cookie将每个用户的随机字符串保存到用户浏览器上
Django中session默认保存在数据库中：django_session表flask,session默认将加密的数据写在用户的cookie中



12,  如何实现用户的登陆认证

1.cookie session
2.token 登陆成功后生成加密字符串
3.JWT：json wed token缩写 它将用户信息加密到token中,服务器不保存任何用户信息
服务器通过使用保存的密钥来验证token的正确性



13, restful风格

restful其实就是一套编写接口的协议，协议规定如何编写以及如何设置返回值、状态码等信息。
