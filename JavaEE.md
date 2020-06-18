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
 ```
 HTTP/1.1 302 Found
 Location: /b
 ```
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

### MVC开发
由以上得知：
+ Servlet适合编写Java代码，实现各种复杂的业务逻辑，但不适合输出复杂的HTML；
+ JSP适合编写HTML，并在其中插入动态内容，但不适合编写复杂的Java代码。
如何才能结合Servlet和JSP的优点，避免两者的缺点。
我们可以写一个Servlet用来执行与数据库、Java Bean相关的操作，然后把Request和Response给Forward到特定的JSP中。在这个JSP中，只负责展示JavaBean相关的信息。
注意：
+ 这里把JSP放入WEB-INF文件夹下是因为它是一个特殊目录，Web Server会阻止浏览器对WEB-INF目录下任何资源的访问，这样就防止用户通过/user.jsp路径直接访问到JSP页面。（这里CTP框架与此不同，CTP框架下JSP文件都放在WEB-INF目录外）
这样的话，我们把Servlet视为业务逻辑处理，把Java Bean视为模型，把JSP视为渲染。这种设计模式即MVC：Model-View-Controller，控制器，模型，视图。
使用MVC模式的好处是，Controller专注于业务处理，它的处理结果就是Model。Model可以是一个JavaBean，也可以是一个包含多个对象的Map，Controller只负责把Model传递给View，View只负责把Model给“渲染”出来，这样，三者职责明确，且开发更简单，因为开发Controller时无需关注页面，开发View时无需关心如何创建Model。

### MVC advanced devalopment
通过结合Servlet和JSP的MVC，我们可以发挥二者的优点：
+ Servlet实现业务逻辑；
+ JSP实现展示逻辑。
但是，直接把MVC搭在Servlet和JSP上并不好，这是因为：
+ Servlet提供的接口仍然偏底层，需要实现Servlet调用相关接口；
+ JSP对页面开发不友好，更好的替代品是模板引擎；
+ 业务逻辑最好由纯粹的Java类实现，而不是强迫继承自Servlet。
因此，我们可以用普通的Java类来实现Controller，类似下面这种：
```
    public class Controller {
        @GetMapping("/signin")
        public ModelAndView signin() {
            ...
        }
        @PostMapping("/signin")
        public ModelAndView doSignin(SigninBean bean) {
            ...
        }
        @GetMapping("/signout")
        public ModelAndView soignout(HttpSession session) {
            ...
        }
    }
```
如果是GET请求，我们希望MVC框架可以直接把URL参数按方法参数对应起来然后传入：
```
    @GetMapping("/hello")
    public ModelAndView hello(String url) {
        ...
    }
```
如果是POST请求，我们希望MVC框架可以直接把Post参数变成一个JavaBean后通过方法参数传入：
```
    @PostMapping("/signin")
    public ModelAndView doSignin(SigninBean bean) {
        ...
    }
```
为了增加灵活性，如果Controller的方法在处理请求时需要访问HttpServletRequest、HttpServletResponse、HttpSession这些实例时，只要方法参数有定义，就可以自动传入：
```
    @GetMapping("/signout")
    public ModelAndView signout(HttpSession session) {
        ...
    }
```
#### 设计MVC框架
那么如何设计这个MVC框架？首先定义ModeAndView：
```
    public class ModelAndView {
        Map<String, Object> model;
        String view;
    }
```
比较复杂的是我们需要在MVC框架中创建一个接收所有请求的Servlet，通常我们把它命名为DispatcherServlet，它总是映射到/，然后，根据不同的Controller的方法定义的@Get或@Post的Path决定调用哪个方法，最后，获得方法返回的ModelAndView后，渲染模板，写入HttpServletResponse，即完成了整个MVC的处理。
DispatcherServlet以及如何渲染均由MVC框架实现，在MVC框架之上只需要编写每一个Controller。
我们来看看如何编写最复杂的DispatcherServlet。首先，我们需要存储请求路径到某个具体方法的映射：
```
@WebServlet(urlPatterns = "/")
public class DispatcherServlet extends HttpServlet {
    private Map<String, GetDispatcher> getMappings = new HashMap<>();
    private Map<String, PostDispatcher> postMappings = new HashMap<>();
}
```
处理一个GET请求是通过GetDispatcher完成的，它需要如下信息：
```
class GetDispatcher {
    Object instance; // Controller实例
    Method method; // Controller方法
    String[] parameterNames; // 方法参数名称
    Class<?>[] parameterClasses; // 方法参数类型
}
```
有了以上信息，就可以定义一个invoke()方法来处理请求：
```
class GetDispatcher {
    ...
    public ModelAndView invoke(HttpServletRequest request, HttpServletResponse response) {
        Object[] arguments = new Object[parameterClasses.length];
        for(int i = 0; i < parameterClasses.length; i++) {
            String parameterName = parameterNames[i];
            Class<?> parameterClass = parameterClasses[i];
            if(parameterClass == HttpServletRequest.class) {
                arguments[i] = request;
            } else if (parameterClass == HttpServletResponse.class) {
                arguments[i] = response;
            } else if (parameterClass == HttpSession.class) {
                arguments[i] = request.getSession();
            } else if (parameterClass == int.class) {
                arguments[i] = Integer.valueOf(getOrDefault(request, parameterName, "0"));
            } else if (parameterClass == long.class) {
                arguments[i] = Long.valueOf(getOrDefault(request, parameterName, "0"));
            } else if (parameterClass == boolean.class) {
                arguments[i] = Boolean.valueOf(getOrDefault(request, parameterName, "false"));
            } else if (parameterClass == String.class) {
                arguments[i] = getOrDefault(request, parameterName, "");
            } else {
                throw new RuntimeException("Missing handler for type: " + parameterClass);
            }
            return (ModelAndView) this.method.invoke(this.instance, arguments);
        }
    }
    private String getOrDefault(HttpServletRequest request, String name, String defaultValue) {
        String s = request.getParameter(name);
        return s == null ? defaultValue : s;
    }
}
```
上述代码使用了反射。
类似的，PostDispatcher需要如下信息：
```
class PostDispatcher {
    Object instance; // Controller实例
    Method method; // Controller方法
    Class<?>[] parameterClasses; // 方法参数类型
    ObjectMapper objectMapper; // JSON映射
}
```
POST的dispatcher如下：
```
class PostDispatcher {
    ...
    public ModelAndView invoke(HttpServletRequest request, HttpServletResponse response) {
        Object[] arguments = new Object[parameterClasses.length];
        for (int i = 0; i < parameterClasses.length; i++) {
            Class<?> parameterClass = parameterClasses[i];
            if (parameterClass == HttpServletRequest.class) {
                arguments[i] = request;
            } else if (parameterClass == HttpServletResponse.class) {
                arguments[i] = response;
            } else if (parameterClass == HttpSession.class) {
                arguments[i] = request.getSession();
            } else {
                // 读取JSON并解析为JavaBean:
                BufferedReader reader = request.getReader();
                arguments[i] = this.objectMapper.readValue(reader, parameterClass);
            }
        }
        return (ModelAndView) this.method.invoke(instance, arguments);
    }
}
```
最后我们来实现整个DispatcherServlet的处理流程，以doGet()为例：
```
class DispatcherServlet extends HttpServer {
    ...
    public void doGet(HttpServletRequest request, HttpServletResponse respose) throws ServletException, IOException {
        resp.setContentType("text/html");
        resp.setCharacterEncoding("UTF-8");
        String path = req.getRequestURI().substring(req.getContextPath().length());
        // 根据路径查找GetDispatcher:
        GetDispatcher dispatcher = this.getMappings.get(path);
        if (dispatcher == null) {
            // 未找到返回404:
            resp.sendError(404);
            return;
        }
        // 调用Controller方法获得返回值:
        ModelAndView mv = dispatcher.invoke(req, resp);
        // 允许返回null:
        if (mv == null) {
            return;
        }
        // 允许返回`redirect:`开头的view表示重定向:
        if (mv.view.startsWith("redirect:")) {
            resp.sendRedirect(mv.view.substring(9));
            return;
        }
        // 将模板引擎渲染的内容写入响应:
        PrintWriter pw = resp.getWriter();
        this.viewEngine.render(mv, pw);
        pw.flush();
    }
}
```