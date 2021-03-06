---
title: HTTP协议详解
date: 2018-03-09 10:36:15
tags:
---

## 引言 ##

HTTP是一个属于应用层的面向对象的协议，它的主要特点可概括如下：<!-- more -->

 1. 支持B/S架构，也支持C/S架构；
 2. 简单快速：向服务器请求数据时，只需要传送**请求方法**和**请求路径**；
 3. 灵活：允许传输任意类型的数据对象，由Content-Type加以标记；
 4. 无连接：限制每次连接只处理一个请求；
 5. 无状态：协议对于事务处理没有记忆功能。
  
**请求方法（所有方法全为大写）**
 
 - GET     请求获取Request-URI所标识的资源
 - POST    在Request-URI所标识的资源后附加新的数据
 - HEAD    请求获取由Request-URI所标识的资源的响应消息报头
 - PUT     请求服务器存储一个资源，并用Request-URI作为其标识
 - DELETE  请求服务器删除Request-URI所标识的资源
 - TRACE   请求服务器回送收到的请求信息，主要用于测试或诊断
 - CONNECT 保留将来使用
 - OPTIONS 请求查询服务器的性能，或者查询与资源相关的选项和需求

**请求路径**

HTTP使用统一资源标识符（Uniform Resource Identifiers, URI）来传输数据和建立连接。一个完整的URL包括以下几部分：

- 协议部分：该URL的协议部分为“http：”，这代表网页使用的是HTTP协议。在Internet中可以使用多种协议，如HTTP，FTP等等本例中使用的是HTTP协议。在"HTTP"后面的“//”为分隔符

- 域名部分：该URL的域名部分为“www.aspxfans.com”。一个URL中，也可以使用IP地址作为域名使用

- 端口部分：跟在域名后面的是端口，域名和端口之间使用“:”作为分隔符。端口不是一个URL必须的部分，如果省略端口部分，将采用默认端口

- 虚拟目录部分：从域名后的第一个“/”开始到最后一个“/”为止，是虚拟目录部分。虚拟目录也不是一个URL必须的部分。本例中的虚拟目录是“/news/”

- 文件名部分：从域名后的最后一个“/”开始到“？”为止，是文件名部分，如果没有“?”,则是从域名后的最后一个“/”开始到“#”为止，是文件部分，如果没有“？”和“#”，那么从域名后的最后一个“/”开始到结束，都是文件名部分。本例中的文件名是“index.asp”。文件名部分也不是一个URL必须的部分，如果省略该部分，则使用默认的文件名

- 锚部分：从“#”开始到最后，都是锚部分。本例中的锚部分是“name”。锚部分也不是一个URL必须的部分

- 参数部分：从“？”开始到“#”为止之间的部分为参数部分，又称搜索部分、查询部分。本例中的参数部分为“boardID=5&ID=24618&page=1”。参数可以允许有多个参数，参数与参数之间用“&”作为分隔符。

## HTTP协议----请求篇详解 ##

http请求由四部分组成，分别是：请求行（request line）、请求报头（header）、空行、请求正文

 1. 请求行格式：“Method Request-URL HTTP-Version CRLF”

 > Method表示请求方法；
 Request-URI是一个统一资源标识符；
HTTP-Version表示请求的HTTP协议版本；
CRLF表示回车和换行

 2. 请求报头，HOST将指出请求的目的地.User-Agent,服务器端和客户端脚本都能访问它,它是浏览器类型检测逻辑的重要基础

 3. 请求正文也叫主体，可以添加任意的其他数据。

## HTTP协议----响应篇详解 ##

http响应也由四部分组成，分别是：状态行、响应报头、空行、响应正文

 1. 状态行格式：“HTTP-Version Status-Code Reason-Phrase CRLF”

 > HTTP-Version表示服务器HTTP协议的版本；  Status-Code表示服务器发回的响应状态代码；
  > Reason-Phrase表示状态代码的文本描述。
  
  **状态代码（Status-Code）**

- 1xx：指示信息--表示请求已接收，继续处理
- 2xx：成功--表示请求已被成功接收、理解、接受
- 3xx：重定向--要完成请求必须进行更进一步的操作
- 4xx：客户端错误--请求有语法错误或请求无法实现
- 5xx：服务器端错误--服务器未能实现合法的请求

 2. 响应报头，Date:生成响应的日期和时间；Content-Type:指定了MIME类型的HTML(text/html),编码类型是UTF-8。

 3. 响应正文就是服务器返回的资源的内容 。

## GET和POST请求的区别 ##

详见[99%的人理解错了HTTP中GET与POST的区别][1]

- GET和POST的底层都是TCP/IP,都属于TCP链接；
- GET把参数包含在URL中，POST通过request body传递参数；
- GET后退按钮/刷新无害，POST数据会被重新提交；
- GET能被缓存、书签可收藏，POST不能缓存、书签不能收藏 ；
- GET对数据长度有限制（2kb），POST无限制；
- GET明文传输数据，不安全；POST参数不会被保存在浏览器历史或 web 服务器日志中，比较安全。
 




 


  [1]: http://mp.weixin.qq.com/s?__biz=MzI3NzIzMzg3Mw==&mid=100000054&idx=1&sn=71f6c214f3833d9ca20b9f7dcd9d33e4#rd