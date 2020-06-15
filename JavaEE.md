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

#### Forward
转发指的是当一个Servlet处理请求时，它可以决定自己不继续处理，而是转给另一个Servlet处理。Forward与Redirect的区别在于，Forward是在Server内部完成的，因此Browser只发出一个Request，而且Browser并不知道发生了这次Forward。

#### Session和Cookie
在Web应用程序中，我们经常要跟踪用户身份。当一个用户登录成功后，如果他继续访问其他页面，Web程序如何才能识别出该用户身份？

因为HTTP协议是一个无状态协议，即Web应用程序无法区分收到的两个HTTP请求是否是同一个浏览器发出的。为了跟踪用户状态，服务器可以向浏览器分配一个唯一ID，并以Cookie的形式发送到浏览器，浏览器在后续访问时总是附带此Cookie，这样，服务器就可以识别用户身份。

这种基于唯一ID识别用户身份的机制称为Session。每个用户第一次访问服务器后，会自动获得一个Session ID。如果用户在一段时间内没有访问服务器，那么Session会自动失效，下次即使带着上次分配的Session ID访问，服务器也认为这是一个新用户，会分配新的Session ID。

对于Web应用程序来说，我们总是通过HttpSession这个高级接口访问当前Session。如果要深入理解Session原理，可以认为Web服务器在内存中自动维护了一个ID到HttpSession的映射表。
而服务器识别Session的关键就是依靠一个名为JSESSIONID的Cookie。在Servlet中第一次调用req.getSession()时，Servlet容器自动创建一个Session ID，然后通过一个名为JSESSIONID的Cookie发送给浏览器。
这里需要注意的是：
+ JSESSIONID是由Servlet容器自动创建的，目的是维护一个浏览器会话，它和我们的登录逻辑没有关系；
+ 登录和登出的业务逻辑是我们自己根据HttpSession是否存在一个"user"的Key判断的，登出后，Session ID并不会改变；
+ 即使没有登录功能，仍然可以使用HttpSession追踪用户，例如，放入一些用户配置信息等。

Servlet提供的HttpSession本质上就是通过一个名为JSESSIONID的Cookie来跟踪用户会话的。除了这个名称外，其他名称的Cookie我们可以任意使用。因此我们可以随意设置自己的Cookie。我们可以设置Cookie的生效路径范围，还可以设置Cookie的有效期。

### JSP开发
JSP和Servlet其实没有任何区别，因为一个JSP在执行前首先会被编译成一个Servlet。可见JSP本质上就是一个Servlet，只不过无需配置映射路径，Web Server会根据路径查找对应的.jsp文件，如果找到了，就自动编译成Servlet再执行。在服务器运行过程中，如果修改了JSP的内容，那么服务器会自动重新编译。
