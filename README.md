# Table of Contents

* [Web发展历程](#Web发展历程)
* [HTML](#html)
* [XML](#xml)
* [JavaScript](#javascript)
* [JSON](#json)
* [AJAX](#ajax)
* [WebService](#webservice)
* [Python](#python)

# Web发展历程
* 1）静态HTML文档
* 2）静态多媒体信息
  静态概念---文件事先存放在服务器端，发送给浏览器后，浏览器展示给客户
* 3）浏览器端与用户的动态交互
  JavaScript、VBScript实现功能，浏览器解析、编译和运行服务器端发送过来的脚本语言编写的小程序
* 4）服务器端与用户的动态交互
  服务器端利用特定程序代码动态生成HTML文档：
  * a、编程语言编写的程序，如CGI程序和java编写的Servlet程序
  * b、嵌入了程序代码的HTML文档，如PHP、JSP（嵌入JAVA程序的HTML文档）、ASP文档
* 5）发布基于Web的应用程序，即Web应用
  Web应用的设计模式及框架  MVC、Struts等  
* 6）Web服务   Web服务架构采用SOAP（简单访问协议）作为通信协议--SOAP规定使用XML通信
  * Web服务：被客户端远程调用的各种方法
  * 客户端请求-服务端调用-Web服务-返回Web服务的响应结果给客户端 
  * 客户端协议连接器将SOAP请求包装成HTTP请求，即成为HTTP请求的正文部分
  * 服务器端协议解析器解析请求，调用web服务，将web服务返回的原始响应结果包装成SOAP响应结果
  * Web服务器把SOAP响应结果包装成一个HTTP响应结果，成为HTTP响应结果的正文部分
  * --web服务借助web服务器发布到网络上，而无需专门的SOAP服务器
* 7）推出Web2.0-全民共建的Web   Blog、RSS、WIKI、SNS、IM（如MSN\QQ等）

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

## HTML协议
网络上传输超级文本（HTML文档）的协议，规定了Web的基本运作过程，以及浏览器和Web服务器之间的通信细节。
位于分层网络体系结构中的应用层，建立在TCP/IP协议的基础上。使用可靠的TCP连接，默认端口80.
* client与server一次信息交换的过程：
  * 1）client与server建立ftp连接
  * 2）客户端发出http请求
  * 3）server发回相应的HTTP响应
  * 4）client与server的ftp连接断开
* HTTP请求格式：
  * 1）请求方法、URI和HTTP协议版本  ##  POST  /hello.jsp  HTPP/1.1
  * 2）请求头
  * 3）请求正文
  HTML请求参数："?"后的值，多个参数之间用& http://localhost:8080/servlet/Hello?username=Tom&password=1234abcd
  HTML表单<form>
  客户端上传文件到服务器
* HTTP响应格式：
  * 1）HTTP协议版本、状态代码和描述  #  HTTP/1.1  200  OK
  * 2）响应头
  * 3）响应正文
* HTTP请求和响应的正文部分数据格式：MIME类型（多用途网络邮件扩展协议）
* 异构系统HTTP协议通信方式：
  * 1）HTTPClient类 - HTTPServer程序
  * 2）IE浏览器  - HTTPServe程序
  * 3）HTTPClient类 - Tomcat服务器（简单的HTTP服务器）
  * 4）IE浏览器  - Tomcat服务器

* 网页中超级链接处理过程：
  * 1）点击超链接
  * 2）浏览器再次与HTTPServer建立TCP连接，发出访问超级链接文件的HTTP请求-服务器...
* 网页中图片的处理过程：
  * 1）点击图片超链接
  * 2）浏览器再次与HTTPServer建立TCP连接，发出访问超级链接文件的HTTP请求
  * 3）浏览器接收到该文件后，解析html文件；
  * 4）解析到<img>标记-根据img标记中的src属性再次向HTPPServer发出请求访问src指定的*.gif文件的HTTP请求
  * 5）HTTPServer将本地gif文件发给浏览器
  * 6）浏览器展示gif图片

### HTTP会话
web服务器跟踪客户的状态的一种方法，多个客户访问同一个网页，如果分辨不同的客户
会话是指单个用户与单个web应用的一连串交互过程，一次会话中客户可能多次请求一个web应用的同一个网页，也可能访问一个web应用的多个网页。
Servlet规范制定了基于Java的会话的具体运作机制：会话开始时，servlet容器将创建一个HttpSession对象，用于存放客户状态信息，每一个Session对象有一个唯一标示符SessionID。客户端会把SessionID作为Cookie保存，供下次客户端访问请求时服务器端的servlet容器核查比对，如查不到则重新创建一个。
##### HttpSession生命周期及会话范围
一次会话范围即浏览器端与一个web应用进行一次会话的过程。
* 创建HttpSession的场景：
  * 1）浏览器第一次访问web应用时；
  * 2）会话被销毁，浏览器再次访问web应用时。客户端SessionID还在，但是服务端servlet容器的HttpSession对象已不存在，无法找到该SessionID对应的HttpSession对象，进而重新创建对象，开始新的会话。
* HttpSession对象结束生命周期的场景：
  * 1）浏览器进程终止；
  * 2）服务端执行HttpSession对象的invalidate()方法；
  * 3）会话过期（一段时间不交互，servlet容器自动销毁会话--tomcat默认interval为1800s）  tomcat中不会被销毁，而是持久化到永久性存储设备中，下次web应用重启后，tomcat会重新加载这些会话。   运行时状态+持久化状态

* 会话的持久化好处：
  * 1）节约内存
  * 2）确保web应用重启后恢复重启前的会话 --故障恢复

# Tomcat
Servlet接口：Web服务器和Web应用（Servlet实现类）之间的接口
tomcat作为运行servlet的容器，基本功能：负责接收和解析来自客户的请求，同时把客户的请求传送给响应的Servlet，并把Servlet的响应结果返回给客户。
客户 ---  Web服务器    --- Web应用
客户 ---  Servlet容器  --- Servlet对象
### tomcat工作模式：
1）独立的Servlet容器  tomcat运行在JVM中
2）其他服务器进程内的Servlet容器    JNI（Java本地调用接口）通信机制
3）其他服务器进程外的Servlet容器    IPC（进程间通信）通信机制
安装tomcat：安装JDK+安装tomcat + 设置JAVA_HOME（jdk安装路径）\CATALINA_HOME（tomcat安装目录）
### tomcat组成：
/bin  启动和关闭tomcat的脚本
/conf tomcat服务器的各种配置文件
/lib  存放tomcat服务器和web应用都可访问的jar文件，web应用的lib子目录存放的jar文件只能被当前web应用访问
/webapps  web应用文件
/work  存放运行时工作文件
/logs  日志目录
### tomcat中发布JavaWeb应用：jar为JDK的命令
  拷贝war文件到webapps目录下即可，启动tomcat服务器时会把webapps目录下的war文件自动展开为开放式的目录结构。
  jar cvf C:\workspace\helloapp.war  web应用的打包文件war文件
  jar xvf C:\workspace\helloapp.war  展开war文件
web组件URL http://localhost:8080/helloapp/hello.asp
  $CATALINA_HOME/webapps/helloapp
### 配置tomcat虚拟主机：
server.xml中的Engine元素下的Host元素值，可以多个虚拟主机名对应同一个主机。
虚拟主机别名设置：
  http://www.mycompany.com:8080/helloapp/hello.asp
  http://mycompany.com:8080/helloapp/hello.asp
  http://mycompany:8080/helloapp/hello.asp
tomcat的Context元素，代表了运行在虚拟主机Host上的单个web应用。

### tomcat的控制平台和管理平台
本身属于tomcat的两个应用admin、manager，在webapps目录下。
控制平台功能：配置tomcat服务器的各种元素
管理平台功能：发布、启动、停止、卸载web应用

### 安全域
web服务器用来保护web应用的资源的一种机制，或者完全由web应用实现，或者采用web服务器通用的安全验证功能。
用户  -- 角色  -- web资源
类型：内存域（存放在xml配置中）、JDBC域（存放在数据库中）、数据源域（存放在数据库中）、JNDI域、JAAS域等
用户登录的三种验证方法：
1）基本验证（用户名密码+Base64编码--可读文本）
2）摘要验证（用户名密码+MD5加密）
3）基于表单的验证（自定义登录页面，非法用户名或口令验证）

### tomcat服务器与其他HTTP服务器集成
此时，tomcat工作方式通常是进程外的servlet容器。
* 集成原因：
  * 1）tomcat主要功能是提供Servlet/JSP容器，但是处理静态资源、管理功能等不如其他专业的HTTTP服务器，如IIS和Apache服务器。
  * 2）对于不支持Servlet/JSP的HTTP服务器，可以通过tomcat服务器来运行Servlet/JSP组件。
* 集成原理：
  * 客户端 --- \[Connector连接器  -- tomcat服务器\]  ---\[JK插件 --- 其他HTTP服务器\]
  * tomcat提供JK插件负责tomcat和其他HTTP服务器的通信，采用AJP协议。
tomcat集群：高可用性、高性能计算、负载均衡

### tomcat中配置SSI
SSI（Server Side Include）是指嵌入到HTML页面的一组指令集合，通用的、运行在服务器端，服务器端优先执行SSI命令获取结果后放到响应结果中（动态完成一些简单功能）
tomcat中配置支持SSI：/conf/web.xml中的servlet-mapping元素
SSI格式：<!--#指令名参数名="参数值" -->   .shtml文件
```
 <html>   
  <head> 
   <title>HTML测试</title>  
  </head>
  <body>     
   欢迎您于（<!--#echo var="DATE_LOCAL" -->）访问本网站。
  </body>
 </html>
 
eg.#echo、#include、#exec、#config等
```
### tomcat中配置SSL
SSL（Server Socket Layer）一种保证网络上的两个节点进行安全通信的协议。加密通信、安全证书等。
TLS（Transport Layer Security）IETF对SSL的标准化
SSL和TLS建立在TCP/IP协议基础上，应用层协议采用SSL来保证安全通信
HTTPS：建立在SSL协议上的HTTP（默认端口80），默认端口443
tomcat中使用SSL：
1）准备安全证书  权威机构获得或者使用keytool工具自我创建
2）配置SSL连接器  server.xml文件中的Connnector元素的注释取消即可
访问支持SSL的web站点：
https访问后，服务器会发送安全证书到客户端，如不是权威机构颁发的证书，客户端浏览器会显示安全警报窗口，点击是表示信任该证书。

### 创建嵌入式tomcat服务器
此时，tomcat工作模式为进程内的Servlet容器。
tomcat嵌入到java应用程序中，控制tomcat服务器的启动和关闭

### tomcat阀
tomcat阀：对于catalina容器接收到的HTTP请求进行预处理；  --只适用于tomcat
过滤器：Java Servlet规范提出的，适用于所有Servlet容器
类型：
1）客户访问日志阀
2）远程地址过滤器   ip过滤
3）远程主机过滤器   host名过滤
4）客户请求记录器

### 使用ANT工具管理web应用
类似于传统的make工具

### 使用Log4J进行日志操作




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

# Servlet技术
* Servlet是JavaWeb应用中最核心的组件，本身也是java编写的类，Servlet对象由Servlet容器创建。
* Servlet的功能：
  * 1）动态生成HTML文档、图像等；
  * 2）将请求转发给同一个web应用中的其他Servlet组件，同一个web应用内web组件之间的合作；
  * 3）访问Servlet容器内的其他web应用，将请求转发给其他web应用中的Servlet组件；
  * 4）读取客户端的Cookie，以及向客户端写入Cookie；
  * 5）访问其他服务器资源（如数据库、基于java的应用程序等）
  * 6）发送供客户端下载的文件；
  * 7）读取并保存客户端的上传文件；
  * 8）访问Servlet容器为web应用提供的工作目录；
  * 9）处理由多个客户端同时访问web应用中的相同资源导致的并发问题。
* Servlet对象模型：
  * 请求对象：ServletRequest、HttpServletRequest 从中获取客户端请求信息
  * 响应对象：ServletResponse、HttpServletResponse 生成响应结果
  * Servlet配置对象：ServletConfig 获取初始化参数信息
  * Servlet上下文对象：ServletContext 访问容器提供的各种资源
* Servlet API：
* Servlet接口：
  * init(ServletConfig config)方法：初始化Servlet对象
  * service(ServletRequest req,ServletResponse res)方法：负责响应客户端的请求
  * destroy()方法：释放Servlet对象占用的资源
  * getServletConfig()方法：返回ServletConfig对象
  * getServletInfo()方法：返回一个字符串，包含Servlet的信息。
* JavaWeb应用的生命周期： -由Servlet容器控制
  * 启动阶段
    * web.xml加载到内存、创建ServletContext对象、初始化Filter（过滤器）、初始化Servlet
  * 运行时阶段
    * 应用的所有Servlet处于待命状态，随时响应客户端请求，提供响应的服务
  * 终止阶段
    * 销毁web应用中所有处理运行时状态的Servlet、Filter；
    * 销毁所有与web应用相关的对象。 
 * tomcat管理平台manager（本身也是一个web应用）管理web应用生命周期 --平台界面操作web应用
   * Servlet容器启动时会启动web应用，web应用启动后就处于运行时状态；
   * Servlet容器被关闭时，会先终止所有web应用
* Servlet的生命周期：--也是由Servlet容器控制
  * 初始化阶段：特定Servlet被客户端首次请求访问；web应用被重启时，web应用中的所有Servlet都会被重新初始化
    * 1）加载Servlet类
    * 2）创建ServletConfig对象
    * 3）创建Servlet对象
    * 4）调用Servlet对象的init()方法
  * 运行时阶段
    * Servlet容器将响应结果发送给客户时就会销毁ServletRequest对象和ServletResponse对象。
  * 销毁阶段
    * web应用被终止时，Servlet容器会先调用web应用中所有Servlet对象的destroy()方法，然后再销毁Servlet对象。
  
服务端的HttpServlet可通过设置特定的HTTP响应头来禁止客户端缓存网页（因为动态更新、含有敏感数据等）。

# 比较HTML、Servlet、JSP（Java Server Page --Servlet的扩展）
1）静态HTML  --简洁直观
浏览器--请求访问hello.html--Web服务器--读取hello.html中的数据
2）Servlet动态生成html页面  --代码繁琐
浏览器--请求访问helloServlet--Web服务器--运行helloServlet--生成html文档
3）JSP动态生成html页面 --html文件中加入Java程序片段和JSP标记
JSP语法、指令、生命周期


# Python

经典的Python入门教材：`<<Think Python>>`
[PDF格式的电子版](http://www.greenteapress.com/thinkpython2/thinkpython2.pdf)  or  [HTML网页版本](http://www.greenteapress.com/thinkpython2/html/index.html)
