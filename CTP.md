## session建立之后JSP，主要包含三个部分
### 数据表现处理Bean的初始化
对于客户会话建立完毕以后的jsp进行交易结果数据显示，需要通过数据显示Bean的接口方法来完成，Bean的初始化方法如下：
```
<jsp:useBean id="utb" scope="page" class="com.ecc.mci.cs.html.MCIJspContextService">
<%
    try {
        utb.initialize(request);
    } catch(Exception e) {
        isTimeout = true;
        return;
    }
%>
</jsp:useBean>
```
### Web URL的确定
```
    String sessionId = utb.getSessionId();
    String fullPath = utb.getAppFullWebPath(sessionId);
```
在JSP页面中，想使用路径的话前面加上<%=fullPath%>，例如：
```
    <script type="text/javascript" src="<%=fullPath%>scripts.js" />
```
### form中action的写法
#### POST Form
```
    <form method="post" action="<%=fullPath%><%=utb.getRequestUrl()%>">
    <%= utb.getRequiredHtmlFields ("", "ActionFlowName") %>
    </form>
```
#### GET Form
```
    <form method="get" action="<%=fullPath%><%=utb.getGetActionUrl()%>?<%=utb.getRequiredHtmlParams(null, "ActionFlowName")%>/>
    <%=utb. getRequiredHtmlParams(null, "ActionFlowName")%>
    </form>
```
### 数据表现处理
#### 取会话ID
```
    String sessionId = utb.getSessionId();
```
#### 取会话Context
```
    Context sessionCtx = utb.getSessionContext();
```
#### 取交易Context
```
    Context context = utb.getContext();
```
## 三种输出格式
对于HTML渠道而言有三种输出格式：
1. HTML格式
2. JPEG格式
3. XML数据报文格式
相对的，在config.ini中配置如下：
```
    <field id="jspPage" value="com.ecc.cte.cs.html.HtmlChannelResponsePageBean" />
    <field id="jpegChart" value="com.ecc.cte.cs.html.HtmlChannelResponseChartBean" />
    <field id="asciiPkg" value="com.ecc.cte.cs.html.HtmlChannelResponseAsciiPkgBean" />
```
相应的，在客户交易请求的参数中，需要提供参数RESType，其值分别为jspPage、jpegChart、asciiPkg。默认情况下为jspPage。
### HTML格式
即JSP格式
### JPEG格式
图形格式
### XML数据报文格式
XML输出处理主要是针对在javascript中通过控件ActiveXObject("Microsoft.XMLHTTP")发起交易请求。发起这种交易请求，需要满足以下条件：
1. 在请求的参数中必须包含参数对 RESType＝asciiPkg
2. 在交易流程中的返回页面业务功能单元中，pageName的值换成数据域名称（该数据域的值是需要返回客户端的 XML 数据包）。
在没有指定返回的情况下，CTP自动判断错误处理数据域 CTEException，然后返回如下数据包：
```
<error>具体的错误信息</error>
```
在交易中要得到xml数据包，需要自定义打包的xml数据包的格式，利用交易组件FormatAction 将交易中的数据组织成xml格式的文本，输出到客户端。
# 业务逻辑处理
CTP中一个交易的组成：
+ 定义交易数据（包括类型数据）
+ 定义服务功能
+ 定义 context 链
+ 定义数据格式
+ 定义交易流程
CTP交易两大原则：
+ 业务功能单元(Action)无类成员变量原则
+ 交易流程(ActionFlow)无类成员变量原则

## 定义交易数据（包括类型数据）
### 类型数据的定义
类型数据在XML的TAG中以data为关键字，并指明数据类型。如：
```
    <data id="name" refType="String" />
```
### 非类型数据的定义
```
    <field id="name" />
```
### 交易数据集合定义
交易数据集合都是kColl类型的集合，在交易集合定义时，参数dynamic的定义决定该交易集合是否可以任由交易请求中的数据进行扩展，而不需要预先进行交易数据声明。该参的定义如下：
+ true -- 可以扩展
+ false或不定义该参数 -- 不可以扩展
## 定义数据格式
数据格式Format的定义，一般如下：
```
    <fmtDef id="transferRecvFmt">
    ...
    </fmtDef>
```
## 定义交易流程
