### JavaEE
JavaEE is Java Platform Enterprise Edition
JavaSE is Java Platform Standard Edition
JavaEE是基于JavaSE的
最早JavaEE的名称为J2EE: Java 2 Platform Enterprise Edition, 后改名为JavaEE
JavaEE最核心的组件是基于Servlet标准的Web服务器

### Servlet
JavaEE提供的Servlet API使我们可以自己编写自己的Servlet来处理HTTP Request，而Web Server实现Servlert API接口，实现底层功能。
编写Servlet需要引入Servlt API，可以通过Maven引入。在pom.xml添加一个servlet dependency即可。
还需要一个web.xml文件放在WEB-INF文件夹中。
然后，可以使用Maven或者Java命令生成war包。
我们无法直接运行war包，而是必须先启动web Server，再由web Server加载war包中的Servlet。因此，web Server也被称为Servlet容器。

支持Servlet API的常用服务器：
+ Tomcat:由Apache开发的开源免费服务器；
+ Jetty：由Eclipse开发的开源免费服务器；
+ GlassFish：一个开源的全功能JavaEE服务器。
还有些商用服务器，如Oracle的WebLogic，IBM的WebSphere。

在Servlet Container内运行Servlet有如下特点：
+ 无法在代码中直接通过new创建Servlet实例，必须由Servlet容器自动创建Servlet实例；
+ Servlet容器只会给每个Servlet类创建唯一实例；
+ Servlet容器会使用多线程执行doGet()或doPost()方法。

#### Redirect
重定向是指当Browser请求一个URL时，Server返回一个Redirect指令，告诉Browser地址已经改变了，需要使用新的URL再发一遍请求。
比如Browser发送GET /a，Server返回Redirect指令302：
 '''
 HTTP/1.1 302 Found
 Location: /b
 '''
Browser接收到这个Redicect指令后会再次发送一个新的请求：GET /b，Server会返回这个URL页面。这就是重定向。
这个过程中有两次HTTP Request。重定向有两种，一种是301响应，称为永久重定向；一种是302响应，称为临时重定向。

