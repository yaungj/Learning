# Learning
NoteBook
1、HTML
  HTML 超文本标记语言（HyperText Markup Language）是一种用于创建网页的标准标记语言，不是一种编程语言。
  标记语言是一套标记标签 (markup tag)
  HTML 使用标记标签来描述网页
  HTML 运行在浏览器上，由浏览器来解析
  HTML文档后缀名.html或.htm
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
HTML 标签通常是成对出现的:开始标签+结束标签；标签速查：http://www.runoob.com/html/html-quicklist.html
  HTML 标题（Heading）是通过<h1> - <h6> 标签来定义的.
  HTML 段落是通过标签 <p> 来定义的.
  HTML 链接是通过标签 <a> 来定义的.<a href="http://www.runoob.com/html/html-basic.html">HTML基础</a>
  HTML 图像是通过标签 <img> 来定义的.<img src="http://www.runoob.com/images/logo.png" width="258" height="39" />
  HTML 元素可以设置属性，属性和属性值对大小写不敏感
  <script> 标签用于定义客户端脚本，比如 JavaScript。
  XHTML 以 XML 格式编写的 HTML，是强制性的   声明 ：<!DOCTYPE ....>



Web Service
通过使用 Web services，您的应用程序可向全世界发布功能或消息。通过 Web services，您的会计部门的 Win 2k 服务器可与 IT 供应商的 UNIX 服务器进行连接。
Web services 使用 XML 来编解码数据，并使用 SOAP 借由开放的协议来传输数据。
Web Services 拥有三种基本的元素:SOAP、WSDL 以及 UDDI。
基本的 Web services 平台是XML + HTTP。
WSDL 网络服务描述语言，是基于XML的用来描述Web services以及如何访问它们的一种语言。
WSDL 可描述web service，连同用于web service的消息格式和协议的细节。
SOAP 简易对象访问协议，是一种使应用程序有能力通过HTTP交换信息的基于 XML 的简易协议。用于应用程序之间的通信。
SOAP 是一种用于访问web service的协议，基于XML，独立于语言、平台。
UDDI 指通用的描述、发现以及整合（Universal Description, Discovery and Integration）。
UDDI 是一种目录服务，通过它，企业可注册并搜索 Web services。由 WSDL 描述的网络服务接口目录。经由 SOAP 进行通迅。
