## 监听器

#### 作用

 1.对web应用对象进行监控

 2.通过Listener监听自动触发指定的功能代码

 3.与过滤器的区别：过滤器的职责是对url进行过滤拦截，是主动的执行

​                                   监听器的职责是对web应用对象进行监听，是被动触发

```java
package com.imooc.listener;

import javax.servlet.ServletContextEvent;
import javax.servlet.ServletContextListener;
//import javax.servlet.annotation.WebListener;
//@WebListener:注解一般不推荐使用
public class FirstListener implements ServletContextListener{

  //暂停时他就销毁了
	@Override
	public void contextDestroyed(ServletContextEvent arg0) {
		// TODO Auto-generated method stub
		System.out.println("ServletContext已销毁");
		
	}
 
	@Override
	public void contextInitialized(ServletContextEvent arg0) {
		// TODO Auto-generated method stub
		System.out.println("ServletContext已初始化");
	}

}

```

```xml
//监听器的配置一行就足够了
<listener>
     <listener-class>com.imooc.listener.FirstListener</listener-class>
 </listener>
```



#### 三种监听对象

ServletContext--对全局ServletContext及其属性进行监听

HttpSession--用户会话及其属性操作进行监听

ServletRequest--对请求及其属性操作进行监听

```java
package com.imooc.listener;

import javax.servlet.ServletContextEvent;

import javax.servlet.ServletContextListener;
import javax.servlet.ServletRequestEvent;
import javax.servlet.ServletRequestListener;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpSession;
import javax.servlet.http.HttpSessionEvent;
import javax.servlet.http.HttpSessionListener;

public class WebListener implements ServletContextListener,HttpSessionListener,ServletRequestListener{

	@Override
	public void contextDestroyed(ServletContextEvent arg0) {
		// TODO Auto-generated method stub
		System.out.println("ServletContext已被销毁");
	}

	@Override
	public void contextInitialized(ServletContextEvent arg0) {
		// TODO Auto-generated method stub
		System.out.println("ServletContext初始化完成");
		
	}

	@Override
	public void sessionCreated(HttpSessionEvent arg0) {
		// TODO Auto-generated method stub
		HttpSession session=arg0.getSession();
		System.out.println("创建Session的id为："+session.getId());
	}

	@Override
	public void sessionDestroyed(HttpSessionEvent arg0) {
		// TODO Auto-generated method stub
		System.out.println("Session已被销毁");
	}

	@Override
	public void requestDestroyed(ServletRequestEvent arg0) {
		// TODO Auto-generated method stub
		System.out.println("HttpServletRequest已被销毁");
	}

	@Override
	public void requestInitialized(ServletRequestEvent arg0) {
		// TODO Auto-generated method stub
		HttpServletRequest request=(HttpServletRequest)arg0.getServletRequest();
	    System.out.println("HttpServletRequest已被创建，uri："+request.getRequestURI());
	}

}

```

```java
package com.imooc.listener;

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
		// TODO Auto-generated method stub
		response.getWriter().println("hello world");
		request.getServletContext().setAttribute("sc-attribute", "sc");
		request.getSession().setAttribute("session", "se");
		request.setAttribute("request", "re");
	}

}

```

```xml
<?xml version="1.0" encoding="UTF-8"?>
<web-app xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns="http://xmlns.jcp.org/xml/ns/javaee" xsi:schemaLocation="http://xmlns.jcp.org/xml/ns/javaee http://xmlns.jcp.org/xml/ns/javaee/web-app_3_1.xsd" id="WebApp_ID" version="3.1">
  <display-name>Listener-interface</display-name>
  <welcome-file-list>
    <welcome-file>index.html</welcome-file>
    <welcome-file>index.htm</welcome-file>
    <welcome-file>index.jsp</welcome-file>
    <welcome-file>default.html</welcome-file>
    <welcome-file>default.htm</welcome-file>
    <welcome-file>default.jsp</welcome-file>
  </welcome-file-list>
  <listener>
     <listener-class>com.imooc.listener.WebListener</listener-class>
  </listener>
   <!--  <listener>
     <listener-class>com.imooc.listener.WebAttributeListener</listener-class>
  </listener> -->
</web-app>
```

**执行的顺序**

ServletContext初始化完成

HttpServletRequest已被创建，uri：/Listener-interface/hello

创建Session的id为：8F45411E394102CFBA3E932484B911E4

HttpServletRequest已被销毁

注意：ServletContext销毁会出现在整个项目暂停的时候，而Session要被销毁则出现在特意去清除它，一般情况下，他不会自动销毁

#### 在线用户统计与流量分析小demo

```java
package com.imooc.listener;

import java.text.SimpleDateFormat;
import java.util.ArrayList;
import java.util.Date;
import java.util.List;

import javax.servlet.ServletContextEvent;
import javax.servlet.ServletContextListener;
import javax.servlet.ServletRequestEvent;
import javax.servlet.ServletRequestListener;
import javax.servlet.http.HttpServletRequest;

public class RequestTotalListener implements ServletContextListener,ServletRequestListener{

	@Override
	public void contextDestroyed(ServletContextEvent sre) {
		// TODO Auto-generated method stub
		
	}

	@Override
	public void contextInitialized(ServletContextEvent sre) {
		// TODO Auto-generated method stub
		List timeList=new ArrayList();
		List valueList=new ArrayList();
		
		sre.getServletContext().setAttribute("timeList", timeList);
		sre.getServletContext().setAttribute("valueList", valueList);
		
	}

	@Override
	public void requestDestroyed(ServletRequestEvent sre) {
		// TODO Auto-generated method stub
		
	}

	@Override
	public void requestInitialized(ServletRequestEvent sre) {
		HttpServletRequest  request=(HttpServletRequest)sre.getServletRequest();
		String url=request.getRequestURL().toString();
		if(url.endsWith("/rt") == true) {
			return;
		}
		// TODO Auto-generated method stub
		//timeList:10:00,10:02,10:04
		//valueList:4      5     8
		List<String> timeList=(List)sre.getServletContext().getAttribute("timeList");
		List<Integer> valueList=(List)sre.getServletContext().getAttribute("valueList");
		//创建时间
		Date date=new Date();
		SimpleDateFormat sdf=new SimpleDateFormat("HH:mm");
		String time=sdf.format(date);
		//判断存在时间点和不存在时间点
		//indexOf的应用：如果要检索的字符串值没有出现，则该方法返回 -1
		if(timeList.indexOf(time)== -1) {
			timeList.add(time);
			valueList.add(1);
			//重新设置ServletContext
			sre.getServletContext().setAttribute("timeList", timeList);
			sre.getServletContext().setAttribute("valueList", valueList);
		}else {
			//index获取这个时间点的索引
			int index=timeList.indexOf(time);
			//value代表这个索引下的value值
			int value=valueList.get(index);
			//index依然代表索引，value值加1
			valueList.set(index, value+1);
			//重新设置ServletContext,没有设置timeList的原因是timeList没有变化
			sre.getServletContext().setAttribute("valueList", valueList);
		}
	}

}

```

```java
package com.imooc.listener;

import java.io.IOException;
import java.util.HashMap;
import java.util.List;
import java.util.Map;

import javax.servlet.ServletContext;
import javax.servlet.ServletException;
import javax.servlet.annotation.WebServlet;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;

import com.alibaba.fastjson.JSON;

/**
 * Servlet implementation class RequestTotalServlet
 */
@WebServlet("/rt")
public class RequestTotalServlet extends HttpServlet {
	private static final long serialVersionUID = 1L;
       
    /**
     * @see HttpServlet#HttpServlet()
     */
    public RequestTotalServlet() {
        super();
        // TODO Auto-generated constructor stub
    }

	/**
	 * @see HttpServlet#doGet(HttpServletRequest request, HttpServletResponse response)
	 */
	protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
		// TODO Auto-generated method stub
		ServletContext context=request.getServletContext();
		List<String> timeList=(List)context.getAttribute("timeList");
		List<Integer> valueList=(List)context.getAttribute("valueList");
		response.setContentType("text/html;charset=utf-8");
//		response.getWriter().println(timeList.toString());
//		response.getWriter().println("<br/>");
//		response.getWriter().println(valueList.toString());
		Map result=new HashMap();
		result.put("timeList",timeList);
		result.put("valueList",valueList);
		String json=JSON.toJSONString(result);
		response.getWriter().println(json);
	}

}

```

```html
//三个test page
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>Test Page</title>
</head>
<body>
 i'm test page1
</body>
</html>
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>Test Page</title>
</head>
<body>
 i'm test page2
</body>
</html>
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>Test Page</title>
</head>
<body>
 i'm test page3
</body>
</html>
```

```html
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>Insert title here</title>
<script type="text/javascript" src="js/echarts.min.js"></script>
<script type="text/javascript" src="js/jquery.3.3.1.min.js"></script>
</head>
<body>
<!-- 为ECharts准备一个具备大小（宽高）的Dom -->
    <div id="main" style="width: 600px;height:400px;"></div>
    <script type="text/javascript">
    function showChart(){
    	  $.ajax({
        	  url:"/request-total/rt",//也可以./rt
        	  type:"get",
        	  dataType:"json",
        	  success:function(json){
        		  console.log(json.timeList);
        		  console.log(json.valueList);
        		  // 基于准备好的dom，初始化echarts实例
        	        var myChart = echarts.init(document.getElementById('main'));

        	        // 指定图表的配置项和数据
        	        var option = {
        	            title: {
        	                text: '请求流量分析统计'
        	            },
        	            tooltip: {},
        	            legend: {
        	                data:['访问量']
        	            },
        	            xAxis: {
        	                data: json.timeList
        	            },
        	            yAxis: {},
        	            series: [{
        	                name: '访问量',
        	                type: 'line',
        	                data:json.valueList
        	            }]
        	        };

        	        // 使用刚指定的配置项和数据显示图表。
        	        myChart.setOption(option);
        	  }
          })
    }
    window.setInterval("showChart()",1000)
       
    </script>
</body>
</html>
```

```xml
 <listener>
      <listener-class>com.imooc.listener.RequestTotalListener</listener-class>
  </listener>
```

#### 静态数据预处理

```java
package com.imooc.listener;

import java.util.ArrayList;
import java.util.List;

import javax.servlet.ServletContextEvent;
import javax.servlet.ServletContextListener;

import com.imooc.listener.entity.Channel;

public class StaticDataListener implements ServletContextListener{

	@Override
	public void contextDestroyed(ServletContextEvent arg0) {
		// TODO Auto-generated method stub
		
	}

	@Override
	public void contextInitialized(ServletContextEvent arg0) {
		// TODO Auto-generated method stub
		List list=new ArrayList();
		list.add(new Channel("实战课程","http://www.imooc.com/1"));
		list.add(new Channel("好课程","http://www.imooc.com/2"));
		list.add(new Channel("就业班","http://www.imooc.com/3"));
		arg0.getServletContext().setAttribute("channelList", list);
	}

}

```

```java
package com.imooc.listener.entity;

public class Channel {
     private String name;
     private String url;
     
	public Channel(String name, String url) {
		super();
		this.name = name;
		this.url = url;
	}
	public String getName() {
		return name;
	}
	public void setName(String name) {
		this.name = name;
	}
	public String getUrl() {
		return url;
	}
	public void setUrl(String url) {
		this.url = url;
	}
     
}

```

```jsp
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
<%@ taglib prefix="c" uri="http://java.sun.com/jsp/jstl/core" %>
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>Insert title here</title>
</head>
<body>
<c:forEach items="${applicationScope.channelList}" var="c">
         <a href="${c.url}">${c.name}</a>|
</c:forEach>
<hr/>
</body>
</html>
```

```xml
<listener>
     <listener-class>com.imooc.listener.StaticDataListener</listener-class>
  </listener>
```

