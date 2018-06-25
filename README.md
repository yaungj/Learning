# Table of Contents

* [HTML](#html)
* [XML](#xml)
* [JavaScript](#javascript)
* [JSON](#json)
* [AJAX](#ajax)
* [WebService](#webservice)
* [Python](#python)


# HTML

* HTML 超文本标记语言（HyperText Markup Language）是一种用于创建网页的标准标记语言，不是一种编程语言。
  * 标记语言是一套标记标签 (markup tag)
  * HTML 使用标记标签来描述网页
  * HTML 运行在浏览器上，由浏览器来解析
  * HTML文档后缀名.html或.htm
```
<!DOCTYPE html>  声明为 HTML5 文档,doctype 声明是不区分大小写的
 <html>   HTML页面根元素
  <head>  元素包含了文档的元（meta）数据
   <meta charset="utf-8">  中文网页需要声明utf-8编码，否则会出现乱码
   <title>HTML测试</title>  元素描述了文档的标题
  </head>
  <body>      元素包含了可见的页面内容
   <h1>我的第一个标题</h1>    元素定义一个大标题
   <p>我的第一个段落</p>   元素定义一个段落
  </body>
 </html>
```
* HTML 标签通常是成对出现的:开始标签+结束标签；[标签速查](http://www.runoob.com/html/html-quicklist.html)
  * HTML 标题（Heading）是通过`<h1> - <h6>` 标签来定义的.
  * HTML 段落是通过标签 `<p>` 来定义的.
  * HTML 链接是通过标签 `<a>` 来定义的.`<a href="http://www.runoob.com/html/html-basic.html">HTML基础</a>`
  * HTML 图像是通过标签 `<img>` 来定义的.`<img src="http://www.runoob.com/images/logo.png" width="258" height="39" />`
  * HTML 元素可以设置属性，属性和属性值对大小写不敏感
  * `<script>` 标签用于定义客户端脚本，比如 JavaScript。
  * XHTML 以 XML 格式编写的 HTML，是强制性的   声明 ：`<!DOCTYPE ....>`

HTML协议
网络上传输超级文本（HTML文档）的协议，规定了Web的基本运作过程，以及浏览器和Web服务器之间的通信细节。
位于分层网络体系结构中的应用层，建立在TCP/IP协议的基础上。使用可靠的TCP连接，默认端口80.
client与server一次信息交换的过程：
1）client与server建立ftp连接
2）客户端发出http请求
3）server发回相应的HTTP响应
4）client与server的ftp连接断开
HTTP请求格式：
  1）请求方法、URI和HTTP协议版本  ##  POST  /hello.jsp  HTPP/1.1
  2）请求头
  3）请求正文
HTTP响应格式：
  1）HTTP协议版本、状态代码和描述  #  HTTP/1.1  200  OK
  2）响应头
  3）响应正文
HTTP请求和响应的正文部分数据格式：MIME类型（多用途网络邮件扩展协议）
异构系统HTTP协议通信方式：
1）HTTPClient类 - HTTPServer程序
2）IE浏览器  - HTTPServe程序
3）HTTPClient类 - Tomcat服务器（简单的HTTP服务器）
4）IE浏览器  - Tomcat服务器

网页中超级链接处理过程：
  1）点击超链接
  2）浏览器再次与HTTPServer建立TCP连接，发出访问超级链接文件的HTTP请求-服务器...
网页中图片的处理过程：
  1）点击图片超链接
  2）浏览器再次与HTTPServer建立TCP连接，发出访问超级链接文件的HTTP请求
  3）浏览器接收到该文件后，解析html文件；
  4）解析到<img>标记-根据img标记中的src属性再次向HTPPServer发出请求访问src指定的*.gif文件的HTTP请求
  5）HTTPServer将本地gif文件发给浏览器
  6）浏览器展示gif图片



# XML

* XML 指可扩展标记语言（eXtensible Markup Language）。
* XML 和 HTML 为不同的目的而设计：
   *   XML 被设计用来传输和存储数据，其焦点是数据的内容。
   *   HTML 被设计用来显示数据，其焦点是数据的外观。

>文档对象模型（DOM，全称 Document Object Model）是一个使程序和脚本有能力动态地访问和更新文档的内容、结构以及样式的平台和语言中立的接口。"
>DOM 被分为 3 个不同的部分/级别：
 >>     核心 DOM - 用于任何结构化文档的标准模型
 >>     XML DOM - 用于 XML 文档的标准模型
 >>     HTML DOM - 用于 HTML 文档的标准模型
>XML DOM 定义访问和操作XML文档的标准方法。
>DOM 将 XML 文档作为一个树形结构，而树叶被定义为节点

* XQuery 之于 XML 作用就类似于 SQL 之于数据库的作用。
* XSL 指扩展样式表语言（EXtensible Stylesheet Language）, 它是一个 XML 文档的样式表语言。
** XSLT 指 XSL 转换。在此教程中，你将学习如何使用 XSLT 将 XML 文档转换为其他文档，比如 XHTML

# JavaScript 

  HTML 定义了网页的内容
  CSS 描述了网页的布局
  JavaScript 网页的行为  --Web的编程语言
JavaScript 是一种轻量级的编程语言。
JavaScript 是可插入 HTML 页面的编程代码。
JavaScript 插入 HTML 页面后，可由所有的现代浏览器执行。
HTML 中的脚本必须位于 <script> 与 </script> 标签之间。
脚本可被放置在 HTML 页面的 <body> 和 <head> 部分中，也可以把脚本保存到外部文件中。外部文件通常包含被多个网页使用的代码

JavaScript 可以通过不同的方式来输出数据：
使用 window.alert() 弹出警告框。
使用 document.write() 方法将内容写到 HTML 文档中。
使用 innerHTML 写入到 HTML 元素。
使用 console.log() 写入到浏览器的控制台。

* jQuery 是一个 JavaScript函数库。jQuery 极大地简化了 JavaScript 编程。
** jQuery 的功能概括
        1、html 的元素选取
        2、html的元素操作
        3、html dom遍历和修改
        4、js特效和动画效果
        5、css操作
        6、html事件操作
        7、ajax异步请求方式
        
* React 是一个用于构建用户界面的 JAVASCRIPT 库，主要用于构建UI。

* Vue.js（读音 /vjuː/, 类似于 view） 是一套构建用户界面的渐进式框架。
** Vue 只关注视图层， 采用自底向上增量开发的设计

* Node.js 就是运行在服务端的 JavaScript。一个基于Chrome JavaScript 运行时建立的一个平台。
** Node.js是一个事件驱动I/O服务端JavaScript环境


# JSON

JSON: JavaScript Object Notation(JavaScript 对象表示法)
JSON 是存储和交换文本信息的语法。类似 XML。
JSON 比 XML 更小、更快，更易解析。
JSON 使用 Javascript语法来描述数据对象，但是 JSON 仍然独立于语言和平台。JSON 解析器和 JSON 库支持许多不同的编程语言。
与 XML 不同之处：
    没有结束标签、更短
    读写的速度更快
    能够使用内建的 JavaScript eval() 方法进行解析
    使用数组
    不使用保留字
    
# AJAX

Asynchronous JavaScript and XML（异步的 JavaScript 和 XML）。
AJAX 不是新的编程语言，而是一种使用现有标准的新方法。
AJAX 最大的优点是在不重新加载整个页面的情况下，可以与服务器交换数据并更新部分网页内容。
AJAX 不需要任何浏览器插件，但需要用户允许JavaScript在浏览器上执行。
AJAX 是一种用于创建快速动态网页的技术
XMLHttpRequest 是 AJAX 的基础，用于在后台与服务器交换数据。

与 POST 相比，GET 更简单也更快，并且在大部分情况下都能用。
然而，在以下情况中，请使用 POST 请求：
    无法使用缓存文件（更新服务器上的文件或数据库）
    向服务器发送大量数据（POST 没有数据量限制）
    发送包含未知字符的用户输入时，POST 比 GET 更稳定也更可靠

# WebService

通过使用 Web services，您的应用程序可向全世界发布功能或消息。通过 Web services，您的会计部门的 Win 2k 服务器可与 IT 供应商的 UNIX 服务器进行连接。
Web services 使用 XML 来编解码数据，并使用 SOAP 借由开放的协议来传输数据。
Web Services 拥有三种基本的元素:SOAP、WSDL 以及 UDDI。
基本的 Web services 平台是XML + HTTP。
* WSDL 网络服务描述语言，是基于XML的用来描述Web services以及如何访问它们的一种语言。
WSDL 可描述web service，连同用于web service的消息格式和协议的细节。
* SOAP 简易对象访问协议，是一种使应用程序有能力通过HTTP交换信息的基于 XML 的简易协议。用于应用程序之间的通信。
SOAP 是一种用于访问web service的协议，基于XML，独立于语言、平台。
* UDDI 指通用的描述、发现以及整合（Universal Description, Discovery and Integration）。
UDDI 是一种目录服务，通过它，企业可注册并搜索 Web services。由 WSDL 描述的网络服务接口目录。经由 SOAP 进行通迅。

# Java


# Python

经典的Python入门教材：`<<Think Python>>`
[PDF格式的电子版](http://www.greenteapress.com/thinkpython2/thinkpython2.pdf)  or  [HTML网页版本](http://www.greenteapress.com/thinkpython2/html/index.html)
