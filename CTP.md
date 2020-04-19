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
    