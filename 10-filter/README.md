

## 过滤器

#### 功能及其作用

 1.对url进行统一的拦截处理

 2.通常应用于应用程序层面进行全局处理

#### 开发过滤器的三要素

1.任何过滤器都要实现filter接口

2.在filter接口的doFilter()方法编写过滤器的代码功能

3.web.xml中对过滤器进行配置，说明拦截url的范围 

**sample1**

```java
package com.imooc.filter;

import java.io.IOException;

import javax.servlet.Filter;
import javax.servlet.FilterChain;
import javax.servlet.FilterConfig;
import javax.servlet.ServletException;
import javax.servlet.ServletRequest;
import javax.servlet.ServletResponse;

public class MyFirstFilter implements Filter{

	@Override
	public void destroy() {
		// TODO Auto-generated method stub
		
	}

	@Override
	public void doFilter(ServletRequest arg0, ServletResponse arg1, FilterChain chain)
			throws IOException, ServletException {
		   
		   System.out.print("过滤器已经生效");
		  chain.doFilter(arg0, arg1);
		
	}

	@Override
	public void init(FilterConfig arg0) throws ServletException {
		// TODO Auto-generated method stub
		
	}

}

```

```java
//有注解的过滤器
package com.imooc.filter;

import java.io.IOException;

import javax.servlet.Filter;
import javax.servlet.FilterChain;
import javax.servlet.FilterConfig;
import javax.servlet.ServletException;
import javax.servlet.ServletRequest;
import javax.servlet.ServletResponse;
import javax.servlet.annotation.WebFilter;

@WebFilter(filterName="AnnoationFilter",urlPatterns="/*")
public class AnnoationFilter implements Filter{

	@Override
	public void destroy() {
		// TODO Auto-generated method stub
		
	}

	@Override
	public void doFilter(ServletRequest request, ServletResponse response, FilterChain chain)
			throws IOException, ServletException {
		System.out.println("注解过滤器生效了");
		chain.doFilter(request, response);
		
	}

	@Override
	public void init(FilterConfig filterConfig) throws ServletException {
		// TODO Auto-generated method stub
		
	}

}

```

```xml
<?xml version="1.0" encoding="UTF-8"?>
<web-app xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns="http://xmlns.jcp.org/xml/ns/javaee" xsi:schemaLocation="http://xmlns.jcp.org/xml/ns/javaee http://xmlns.jcp.org/xml/ns/javaee/web-app_3_1.xsd" id="WebApp_ID" version="3.1">
  <display-name>MyFirstServlet</display-name>
  <welcome-file-list>
    <welcome-file>index.html</welcome-file>
    <welcome-file>index.htm</welcome-file>
    <welcome-file>index.jsp</welcome-file>
    <welcome-file>default.html</welcome-file>
    <welcome-file>default.htm</welcome-file>
    <welcome-file>default.jsp</welcome-file>
  </welcome-file-list>
<!--     <filter>
        <filter-name>MyFirstFilter</filter-name>
        <filter-class>com.imooc.filter.MyFirstFilter</filter-class>
    </filter>
    这里url-pattern的意思是过滤的范围，这里的范围是全部
    <filter-mapping>
         <filter-name>MyFirstFilter</filter-name>
        <url-pattern>/*</url-pattern>
    </filter-mapping>
    
    <filter> -->
</web-app>
```

```html
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>Insert title here</title>
</head>
<body>
     我是默认页面
</body>
</html>
```

**sample2(中文乱码过滤器,init-param可以设置过滤器参数,常常用于可能需要改变的参数,记得还有ServletRequest接口的说明)**

```java
package com.imooc.filter;

import java.io.IOException;


import javax.servlet.Filter;
import javax.servlet.FilterChain;
import javax.servlet.FilterConfig;
import javax.servlet.ServletException;
import javax.servlet.ServletRequest;
import javax.servlet.ServletResponse;
import javax.servlet.annotation.WebFilter;
import javax.servlet.annotation.WebInitParam;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
@WebFilter(filterName="CharacterEncodingFilter",urlPatterns="/*",
initParams= {
		@WebInitParam(name="encoding",value="UTF-8")
})
public class CharacterEncodingFilter implements Filter {
    private String encoding;
    @Override
	public void init(FilterConfig filterConfig) throws ServletException {
		encoding=filterConfig.getInitParameter("encoding");
		System.out.println(encoding);

	}

	@Override
	public void doFilter(ServletRequest request, ServletResponse response, FilterChain chain)
			throws IOException, ServletException {
		// TODO Auto-generated method stub
    //这里ServletRequest接口是HttpServletRequest的上一级接口，相当于ServletRequest是动物，         HttpServletRequest是哺乳类动物的概念，而他们是接口，没有具体实现功能，他们都在servlet-api.jar里面，是J2EE的规范中的一种
    //他们的实现类叫RequestFacade,由tomcat这种第三方厂商来具体实现的
		HttpServletRequest req = (HttpServletRequest)request;
		req.setCharacterEncoding(encoding);
		HttpServletResponse res=(HttpServletResponse)response;
		res.setContentType("text/html;charset="+encoding);
		chain.doFilter(request, response);
	}

	
	@Override
	public void destroy() {
		// TODO Auto-generated method stub

	}

}

```

```java
package com.imooc.filter;

import java.io.IOException;
import javax.servlet.ServletException;
import javax.servlet.annotation.WebServlet;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;

/**
 * Servlet implementation class HelloServlet
 */
@WebServlet("/hello")
public class HelloServlet extends HttpServlet {
	private static final long serialVersionUID = 1L;
       
    /**
     * @see HttpServlet#HttpServlet()
     */
    public HelloServlet() {
        super();
        // TODO Auto-generated constructor stub
    }

	/**
	 * @see HttpServlet#doGet(HttpServletRequest request, HttpServletResponse response)
	 */
	protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
		response.getWriter().println("中文字幕");
	}

}

```

```xml

   <!--  <filter>
        <filter-name>CharacterEncodingFilter</filter-name>
        <filter-class>com.imooc.filter.CharacterEncodingFilter</filter-class>
        <init-param>
             <param-name>encoding</param-name>
             <param-value>UTF-8</param-value>
        </init-param>
    </filter>
    这里url-pattern的意思是过滤的范围，这里的范围是全部
    <filter-mapping>
         <filter-name>CharacterEncodingFilter</filter-name>
        <url-pattern>/*</url-pattern>
    </filter-mapping> -->
```

**sample3(多个url的控制)**

```java
package com.imooc.filter;

import java.io.IOException;

import javax.servlet.Filter;
import javax.servlet.FilterChain;
import javax.servlet.FilterConfig;
import javax.servlet.ServletException;
import javax.servlet.ServletRequest;
import javax.servlet.ServletResponse;
import javax.servlet.annotation.WebFilter;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;

//@WebFilter(filterName="testFilter",urlPatterns= {"/","/servlet/*","*.jsp"})
public class testFilter implements Filter {

	@Override
	public void destroy() {
		// TODO Auto-generated method stub

	}

	@Override
	public void doFilter(ServletRequest request, ServletResponse response, FilterChain chain)
			throws IOException, ServletException {
//		response.getWriter().println("i am "+this.getClass().getSimpleName());
		HttpServletRequest req = (HttpServletRequest)request;
		HttpServletResponse res = (HttpServletResponse)response;
		System.out.println("i am "+req.getRequestURL());
		chain.doFilter(request, response);
	}

	@Override
	public void init(FilterConfig filterConfig) throws ServletException {
		// TODO Auto-generated method stub

	}

}

```

```java
package com.imooc.filter;

import java.io.IOException;
import javax.servlet.ServletException;
import javax.servlet.annotation.WebServlet;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;

/**
 * Servlet implementation class testServlet
 */
@WebServlet("/servlet/sample1")
public class testServlet extends HttpServlet {
	private static final long serialVersionUID = 1L;
       
    /**
     * @see HttpServlet#HttpServlet()
     */
    public testServlet() {
        super();
        // TODO Auto-generated constructor stub
    }

	/**
	 * @see HttpServlet#doGet(HttpServletRequest request, HttpServletResponse response)
	 */
	protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
		// TODO Auto-generated method stub
		response.getWriter().println("hhhh");
	}

	/**
	 * @see HttpServlet#doPost(HttpServletRequest request, HttpServletResponse response)
	 */
	protected void doPost(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
		// TODO Auto-generated method stub
		doGet(request, response);
	}

}

```

```html
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>Insert title here</title>
</head>
<body>
    i am test jsp!!!
</body>
</html>
```

```xml
<?xml version="1.0" encoding="UTF-8"?>
<web-app xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns="http://xmlns.jcp.org/xml/ns/javaee" xsi:schemaLocation="http://xmlns.jcp.org/xml/ns/javaee http://xmlns.jcp.org/xml/ns/javaee/web-app_3_1.xsd" id="WebApp_ID" version="3.1">
  <display-name>url-pattern</display-name>
  <welcome-file-list>
    <welcome-file>index.html</welcome-file>
    <welcome-file>index.htm</welcome-file>
    <welcome-file>index.jsp</welcome-file>
    <welcome-file>default.html</welcome-file>
    <welcome-file>default.htm</welcome-file>
    <welcome-file>default.jsp</welcome-file>
  </welcome-file-list>
  <filter>
       <filter-name>testFilter</filter-name>
       <filter-class>com.imooc.filter.testFilter</filter-class>
  </filter>
  <filter-mapping>
         <filter-name>testFilter</filter-name>
         <url-pattern>/</url-pattern>
  </filter-mapping>
  <!-- 多个url的时候 -->
  <!--  <filter-mapping>
         <filter-name>testFilter</filter-name>
         <url-pattern>*.jsp</url-pattern>
  </filter-mapping>
   <filter-mapping>
         <filter-name>testFilter</filter-name>
         <url-pattern>/servlet/*</url-pattern>
  </filter-mapping> -->
</web-app>
```

**特别的**

如果有映射是/，有index.jsp，index.html......他会自动忽略用到/的servlet，直接匹配index.jsp,index.html...

```java
package com.imooc.filter;

import java.io.IOException;
import javax.servlet.ServletException;
import javax.servlet.annotation.WebServlet;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;

/**
 * Servlet implementation class sampleServlet
 */
@WebServlet("/")
public class sampleServlet extends HttpServlet {
	private static final long serialVersionUID = 1L;
       
    /**
     * @see HttpServlet#HttpServlet()
     */
    public sampleServlet() {
        super();
        // TODO Auto-generated constructor stub
    }

	/**
	 * @see HttpServlet#doGet(HttpServletRequest request, HttpServletResponse response)
	 */
	protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
		// TODO Auto-generated method stub
		response.getWriter().println("xx");
	}

	/**
	 * @see HttpServlet#doPost(HttpServletRequest request, HttpServletResponse response)
	 */
	protected void doPost(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
		// TODO Auto-generated method stub
		doGet(request, response);
	}

}

```

```jsp
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>Insert title here</title>
</head>
<body>
    i am index jsp!!!
</body>
</html>
```

```xml
<filter>
       <filter-name>testFilter</filter-name>
       <filter-class>com.imooc.filter.testFilter</filter-class>
  </filter>
  <filter-mapping>
         <filter-name>testFilter</filter-name>
         <url-pattern>/</url-pattern>
  </filter-mapping>
```

**过滤链**

```java
package com.imooc.filter;

import java.io.IOException;

import javax.servlet.Filter;
import javax.servlet.FilterChain;
import javax.servlet.FilterConfig;
import javax.servlet.ServletException;
import javax.servlet.ServletRequest;
import javax.servlet.ServletResponse;

public class FilterA implements Filter{

	@Override
	public void destroy() {
		// TODO Auto-generated method stub
		
	}

	@Override
	public void doFilter(ServletRequest request, ServletResponse response, FilterChain chain)
			throws IOException, ServletException {
		 System.out.println("i am Filter A");
		 chain.doFilter(request, response);
//       按照这个顺序，会是servlet-C-B-A，因为一直先向后传递，然后响应的时候逆向回来
//		 chain.doFilter(request, response);
//		 System.out.println("i am Filter A");
		 
		
	}

	@Override
	public void init(FilterConfig arg0) throws ServletException {
		// TODO Auto-generated method stub
		
	}

}

```

```java
package com.imooc.filter;

import java.io.IOException;

import javax.servlet.Filter;
import javax.servlet.FilterChain;
import javax.servlet.FilterConfig;
import javax.servlet.ServletException;
import javax.servlet.ServletRequest;
import javax.servlet.ServletResponse;

public class FilterB implements Filter{

	@Override
	public void destroy() {
		// TODO Auto-generated method stub
		
	}

	@Override
	public void doFilter(ServletRequest request, ServletResponse response, FilterChain chain)
			throws IOException, ServletException {
		System.out.println("i am FilterB");
		chain.doFilter(request, response);
//		 chain.doFilter(request, response);
//		 System.out.println("i am Filter B");

	}

	@Override
	public void init(FilterConfig filterConfig) throws ServletException {
		// TODO Auto-generated method stub
		
	}

}

```

```java
package com.imooc.filter;

import java.io.IOException;

import javax.servlet.Filter;
import javax.servlet.FilterChain;
import javax.servlet.FilterConfig;
import javax.servlet.ServletException;
import javax.servlet.ServletRequest;
import javax.servlet.ServletResponse;

public class FilterC implements Filter{

	@Override
	public void destroy() {
		// TODO Auto-generated method stub
		
	}

	@Override
	public void doFilter(ServletRequest request, ServletResponse response, FilterChain chain)
			throws IOException, ServletException {
		System.out.println("i am FilterC");
		chain.doFilter(request, response);
//		 chain.doFilter(request, response);
//		 System.out.println("i am Filter C");

		
	}

	@Override
	public void init(FilterConfig filterConfig) throws ServletException {
		// TODO Auto-generated method stub
		
	}

}

```

```java
package com.imooc.filter;

import java.io.IOException;
import javax.servlet.ServletException;
import javax.servlet.annotation.WebServlet;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;

/**
 * Servlet implementation class SampleServlet
 */
@WebServlet("/sa")
public class SampleServlet extends HttpServlet {
	private static final long serialVersionUID = 1L;
       
    /**
     * @see HttpServlet#HttpServlet()
     */
    public SampleServlet() {
        super();
        // TODO Auto-generated constructor stub
    }

	/**
	 * @see HttpServlet#doGet(HttpServletRequest request, HttpServletResponse response)
	 */
	protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
		// TODO Auto-generated method stub
		response.getWriter().println("hhhhhh");
		System.out.println("i am servlet");
	}

	/**
	 * @see HttpServlet#doPost(HttpServletRequest request, HttpServletResponse response)
	 */
	protected void doPost(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
		// TODO Auto-generated method stub
		doGet(request, response);
	}

}

```

```xml
<?xml version="1.0" encoding="UTF-8"?>
<web-app xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns="http://xmlns.jcp.org/xml/ns/javaee" xsi:schemaLocation="http://xmlns.jcp.org/xml/ns/javaee http://xmlns.jcp.org/xml/ns/javaee/web-app_3_1.xsd" id="WebApp_ID" version="3.1">
  <display-name>filterChain</display-name>
  <welcome-file-list>
    <welcome-file>index.html</welcome-file>
    <welcome-file>index.htm</welcome-file>
    <welcome-file>index.jsp</welcome-file>
    <welcome-file>default.html</welcome-file>
    <welcome-file>default.htm</welcome-file>
    <welcome-file>default.jsp</welcome-file>
  </welcome-file-list>
  <filter>
       <filter-name>FilterA</filter-name>
       <filter-class>com.imooc.filter.FilterA</filter-class>
  </filter>
  <filter-mapping>
       <filter-name>FilterA</filter-name>
       <url-pattern>/*</url-pattern>
  </filter-mapping>
   <filter>
       <filter-name>FilterB</filter-name>
       <filter-class>com.imooc.filter.FilterB</filter-class>
  </filter>
  <filter-mapping>
       <filter-name>FilterB</filter-name>
       <url-pattern>/*</url-pattern>
  </filter-mapping>
   <filter>
       <filter-name>FilterC</filter-name>
       <filter-class>com.imooc.filter.FilterC</filter-class>
  </filter>
  <filter-mapping>
       <filter-name>FilterC</filter-name>
       <url-pattern>/*</url-pattern>
  </filter-mapping>
</web-app>
```

正常输出结果是按照过滤链的顺序来执行过滤器，这个sample就是ABC之后就执行servlet，具体可以参照ppt那个图。

**注意：一般一个过滤器只承担一个功能，当过滤链写在注解里面时，过滤链是按照过滤器的类名的字母升序的顺序依次执行的而不是filterName名称，一般不推荐写在注解里面**

**sample4(多端设备自动匹配)**

```java
package com.imooc.filter;

import java.io.IOException;

import javax.servlet.Filter;
import javax.servlet.FilterChain;
import javax.servlet.FilterConfig;
import javax.servlet.ServletException;
import javax.servlet.ServletRequest;
import javax.servlet.ServletResponse;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;

public class DeviceAdapterFilter implements Filter{

	@Override
	public void destroy() {
		// TODO Auto-generated method stub
		
	}

	@Override
	public void doFilter(ServletRequest request, ServletResponse response, FilterChain chain)
			throws IOException, ServletException {
		HttpServletRequest req=(HttpServletRequest)request;
		HttpServletResponse res=(HttpServletResponse)response;
		/*
		  /index.html
		  pc:desktop/index.html
		  mobile:mobile/index.html
		 */
		String uri=req.getRequestURI();
		if(uri.startsWith("/desktop")||uri.startsWith("/mobile")) {
			chain.doFilter(request, response);
		}else {
			String userAgent=req.getHeader("user-agent").toLowerCase();
			String targetURI="";
			if(userAgent.indexOf("android")!=-1||userAgent.indexOf("iphone")!=-1) {
				targetURI="/mobile"+uri;
				System.out.println("移动端设备正在访问，正在跳转到新的uri："+targetURI);
				res.sendRedirect(targetURI);
			}else {
				targetURI="/desktop"+uri;
				System.out.println("PC端设备正在访问，正在跳转到新的uri："+targetURI);
				res.sendRedirect(targetURI);
			}
		}
		
	}

	@Override
	public void init(FilterConfig arg0) throws ServletException {
		// TODO Auto-generated method stub
		
	}

}

```

```html
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>Insert title here</title>
</head>
<body>
    <img alt="" src="/images/mobile.jpg"/>
</body>
</html>

<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>Insert title here</title>
</head>
<body>
   <img alt="" src="/images/desktop.jpg"/>
</body>
</html>
```





