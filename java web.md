[TOC]

## servlet
[servlet document](http://tomcat.apache.org/tomcat-9.0-doc/servletapi/index.html "document")

[java api documentation](https://docs.oracle.com/en/java/javase/13/docs/api/index.html "java api documentation")



#### Servlet interface
servlet实例是单例多线程的, 线程安全问题，不要在servlet容器中修改变量
一つインスタンスでmultiple thread、二人上同時尋ねれば、thread安全の問題があるので、直接に変数を変わることがしない方が良い

servlet liftCycle生命周期
``` java
# init 初始化, get a servlet config
void init(ServletConfig config) throws ServletException

# service, http请求回复，repeat request and response with the uri
void service(ServletRequest req, ServletResponse res) throws ServletException, IOException

destroy 销毁
void destroy()

String getServletInfo()
ServletConfig getServletConfig()
```

##### servlet map映射
WEB-INF->web.xml

```
<servlet>
        <servlet-name>logging-servlet</servlet-name>
        <servlet-class>com.LoggingServlet</servlet-class>
        <load-on-startup>1</load-on-startup>
</servlet>
<servlet-mapping>
    <servlet-name>logging-servlet</servlet-name>
    <url-pattern>/logging</url-pattern>
</servlet-mapping>

<!--servlet init param-->
<servlet>
...
<init-param>
<param-name></param-name>
<param-value></param-value>
</init-param>
</servlet>
```
load-on-startup: servlet初めて起動するときの順序, 0,1,...

- ServletConfig
servletConfigはweb.xmlの対応の一つservletタグを含む
一つservlet容器は必ず一つSだけのervletConfig含む

getServletName()->string
getInitParameterNames()->Enumeration\<String>
getInitParameter(string name)->string value

getServletContext()->ServletContext
- ServletContext
- welcomePage
1, WEB-INF->web.xml
2, $CATALINA_HOME/conf/web.xml

```
<welcome-file-list>
<welcome-file></welcome-file>
...
</welcome-file-list>
```

abstract抽象方法只能出现在抽象类中

- GenericServlet
javax.servlet.GenericServlet






##### encoding
tomcat服务器默认是iso-8859-1
tomcat9解决了GET乱码问题，典型的tomcat7的GET是乱码

Post:
```
request.setCharacterEncoding("utf-8");

```
get:
1, $CATALINA/conf/server.xml->`<Connector URIEncoding="utf-8">`





##### HttpServleetResponse

`setContentType("text/html;charset=UTF-8")`
setCharacterEncoding: change ContentType's character

请求转发和重定向:
请求转发: 服务器内跳转
重定向: 服务器外跳转，防止表单重复提交

装饰器模式：对象增强
适配器模式：有些方法让子类实现，让子类重写一个类，父类调用子类的方式


#### HttpServlet
toGet/toPost
override `service`


RequestFacadeはHttpServletRequestの実現、請求するときにクリエイトして、レスポンスした次第に消える
ライフサイクル:
request->response


#### jsp
jsp文件会被翻译到servlet方法中，jsp就是一个servlet
```
<%--comment--%>
<%Java 语句%> 这里定义的语句，变量不能定义权限，不能定义方法，不能定义静态语句
<%!声明语句块%>这里被定义到servlet类里，可以定义权限，可以定义方法，可以定义静态语句
<%=表达式%> 直接能被放到页面上
<%@ page %>: pageEncoding,contentType => setContentType
<%@ page import="com.xxx.xxx" %>
errorPage: 
isErrorPage: current page is a error page
<%@ include file=""  %>
```

- jsp builtin

request, response
session: javax.Servlet.http.HttpSession
application: javax.servlet.ServletContext
config: javax.servlet.ServletConfig

out: javax.servlet.jsp.JspPrintWriter

pageContext: javax.servlet.jsp.PageContext

- JSP动作

```
<jsp:forward page="example.jsp">
	<jsp:param value="xxx" name="fdsfdsf" />
</jsp:forward>
request.getParameter(name)->value
<jsp:include page="example.jsp"></jsp:include>
```

EL表达式 expression language

#### 四大域
pageContext->request->session->application
pageScope->requestScope->sessionScope->applicationScope

```
pageContext.setAttribute(name, value)
${name}
${pageScope.name}

${pageContext.request.contextPath}/xxxServlet
${param.xxx} # 请求参数中的值
${paramValues.xxx} # checkbox, select
${initParam.xxx} # web.xml init_param
```

#### el函数

.tld文件: tag library definition

jsp
```
<% taglib uri="xxxx" prefix="xxxx" %>
uri # tld内的uri标签的内容
prefix # short-name

${short-name:function(params) }
```

#### JSTL: jsp tag library


#### jsp simpletag















