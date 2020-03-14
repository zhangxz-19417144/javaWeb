##  web-xml 简述

#####  通过/*来访问 /1 /2 这种简单的访问每一个id

```java
protected void service(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
		     //获取员工信息
		     //获取当前的url
		    String url=request.getRequestURL().toString();
		    System.out.println(url);
		    String id=url.substring((url.lastIndexOf("/"))+1);
		    response.setContentType("text/html;charset=utf-8");
		    PrintWriter out=response.getWriter();
		    out.println("<h1>"+id+"</h1>");
		    if(id.equals("1")) {
		    	out.println("<h1>"+"张三"+"</h1>");
		    }else if(id.equals("2")) {
		    	out.println("<h1>"+"李四"+"</h1>");
		    }else {
		    	out.println("<h1>"+"其他员工"+"</h1>");
		    }
	}
```



##### 通过web-xml来设置servlet-Context

```java
//default method
protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
		    ServletContext context=request.getServletContext();
		    
		    String copyRight=(String)context.getAttribute("copyRight");
		    String title=(String)context.getAttribute("title");
		    response.setContentType("text/html;charset=utf-8");
		    response.getWriter().println("<h1>"+copyRight+"</h1>"+"\n"+title);
	}
```

```java
//init method
protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
		  ServletContext context=request.getServletContext();
		  String copyright=context.getInitParameter("copyright");
		  context.setAttribute("copyRight", copyright);
		  String title=context.getInitParameter("title");
		  context.setAttribute("title", title);
		  
		  response.getWriter().println("init success");
	}
```

```xml
 <context-param>
       <param-name>copyright</param-name>
       <param-value>@ 2018 imooc.com</param-value>
   </context-param>
    <context-param>
       <param-name>title</param-name>
       <param-value>程序员的梦工厂</param-value>
   </context-param>
```

##### 错误页面的改写

```html
<!--404错误改写-->
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>Insert title here</title>
</head>
<body>
       <h1>404 ERROR</h1>
</body>
</html>
```

```xml
 <!-- error page的设置 -->
   <error-page>
        <error-code>404</error-code>
        <location>/error/404.html</location>
   </error-page>
```

