---
categories:
  - 编程语言
  - 前端
---
# HTTP

## 概述

HyperText Transfer Tansfer Protocol 超文本传输协议，是一种基于**TCP**的应用层协议，也是目前为止最为流行的**应用层协议**之一，可以说HTTP协议是万维网的基石。历经了0.9、HTTP/1.0、HTTP/1.1、HTTP/2几个版本，目前流行的还是HTTP1.1这个版本，HTTP2还在推广中。

首先需要明白HTTP是一种**客户端请求、服务器应答式**的应用层传输协议，也就是说**服务器端是不可能主动向客户端发送数据**的，如果服务器能主动向客户端发送数据那这个世界就乱套了，同时还说明了在网络正常的情况下**请求和响应都是一一对应**的。而这个请求和响应也就是后端开发人员经常看到的**Request和Response**。

![HTTP问答响应模型](https://img-blog.csdn.net/20170330190707249?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvSG9sbW9meQ==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

## Request|Response HTTP请求报文格式与响应报文格式

<img src="https://img-blog.csdn.net/20170330192653242?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvSG9sbW9meQ==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast" alt="HTTP Request 请求报文格式" style="zoom: 67%;" />

<img src="https://img-blog.csdn.net/20170330192754102?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvSG9sbW9meQ==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast" alt="Response HTTP响应报文格式" style="zoom: 67%;" />

请求

```http
POST /index.html HTTP/1.1
HOST: www.XXX.com
User-Agent: Mozilla/5.0(Windows NT 6.1;rv:15.0) Firefox/15.0

Username=admin&password=admin
```

响应

```http
HTTP/1.1 200 OK
Content-Encoding: gzip
Content-Type: text/html;charset=utf-8

<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8" />
    <title>Document</title>
</head>
<body>
    <p>this is http response</p>
</body>
</html>
```

### 请求方式Method

1. **GET**方法意思是获取URL指定的资源，这个请求方式是最简单的也是最常用的。使用GET 方法时，可以将请求参数和对应的值附加在 URI 后面，**用一个问号`"?"`将资源的URI和请求参数隔开**，**参数之间使用与符号`"&"`隔开**，因此传递参数长度也受到了限制，而且与隐私相关的信息也直接暴露在URI中。

   比如`/index.jsp?username=holmofy&password=123123`

2. **HEAD** 方法与GET用法相同，但没有响应体，使用场合没有GET多。比如下载前使用HEAD发送请求，通过`ContentLength`响应字段，来了解网络资源的大小；或者通过`LastModified`响应字段来判断本地缓存资源是否要更新。
3. **POST** 方法一般用**提交信息或数据**，请求服务器进行处理（例如提交表单或者上传文件）。表单使用POST相对GET来说还是比较隐秘的，而且GET的URL有长度限制，而上传大文件就必须要使用POST了。

4. **OPTIONS**方法比较少见，该方法用于请求服务器告知其支持哪些其他的功能和方法。通过OPTIONS 方法，可以询问服务器具体支持哪些方法，或者服务器会使用什么样的方法来处理一些特殊资源。可以说这是一个探测性的方法，客户端通过该方法可以在不访问服务器上实际资源的情况下就知道处理该资源的最优方式。这个选项在跨域HTTP请求的情况出现的比较多，[这里](https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Access_control_CORS)有一片关于跨域请求的文章，其中有一张图很好的解释了什么是跨域HTTP请求。

![跨域HTTP请求](https://mdn.mozillademos.org/files/14295/CORS_principle.png)

5. PUT：用于向指定资源位置上传其最新内容(原来没有就上传，有就上传并覆盖原来的内容)
6. DELETE：请求服务器删除Request-URI所标识的资源。
7. TRACE：回显服务器收到的请求，主要用于测试或诊断。比较少见。
8. CONNECT：HTTP/1.1协议中预留给能够将连接改为管道方式的代理服务器。通常用于SSL加密服务器的链接（经由非加密的HTTP代理服务器）。

**其中GET和POST是最常用的，其次是HEAD和OPTIONS；像PUT和DELETE属于危险方法，服务器都会关闭；TRACE和CONNECT非常少见**

### Request-URI

URI 的最常见形式是统一资源定位符 (URL)，它也被称为 Web地址。这种网址是我们经常见到的。

```http
http://example.org/absolute/path/resource.txt
```

在请求的URI中还有一种相对路径的，对于这种请求来说在请求头信息中一般有Host值来指定服务器主机。比如：

```http
GET /Test.jsp HTTP/1.1
...
Host:www.example.com
...
```

### Http响应码

客户端发出HTTP请求，服务端接收后，会向客户端发送响应信息，其中HTTP响应中的第一行中，最重要的就是HTTP的状态码。比如：

```http
HTTP/1.0 200 OK
```

这个状态码是200，表示请求成功。HTTP协议中状态码有三位数字组成，第一位数字定义了响应的类别，有以下五种：

- 1XX：信息提示。表示请求已被服务器接受，但需要继续处理，范围为100~101。
- 2XX：请求成功。服务器成功处理了请求。范围为200~206。
- 3XX：客户端重定向。重定向状态码用于告诉客户端浏览器，它们访问的资源已被移动，并告诉客户端新的资源位置。客户端收到重定向会重新对新资源发起请求。范围为300~305。
- 4XX：客户端信息错误。客户端可能发送了服务器无法处理的东西，比如请求的格式错误，或者请求了一个不存在的资源。范围为400~415。
- 5XX：服务器出错。客户端发送了有效的请求，但是服务器自身出现错误，比如Web程序运行出错。范围是500~505。

常见的状态码务必要熟悉：

200：客户端请求成功。

302：重定向。

404：请求资源不存在。

400：请求语法错误，服务器无法理解。

403：服务器收到请求，但拒绝提供服务。

500：服务器内部错误。

503：服务器当前不能处理客户端请求，可能需要一段时间后才能恢复正常。

> 更多更详细的响应码可以参考最后面的附录。