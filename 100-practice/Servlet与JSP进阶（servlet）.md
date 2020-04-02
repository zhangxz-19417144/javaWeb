##### 题目描述

<p>
1.已知如下查询页面，点击查询按钮提交给一个Servlet进行处理，在Servlet中定义一个HashMap，存储中英文单词的信息，如apple为key值，苹果为value值，可以存储三个。然后获取JSP页面提交的参数，也就是key值，判断key值在HashMap中是否存在，可以使用containsKey（）方法，返回值是一个boolean类型。
2.如果返回值为true，将查询到的结果存储到request中，并转发到success.jsp显示。
3.如果返回值为false，将结果” 没有找到对应的单词解释”存储到session中，重定向到fail.jsp进行显示。
</p>



```java
//servlet
protected void service(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
	            String key=request.getParameter("input");
	            HashMap<String,String> word=new HashMap<String,String>();
	            word.put("apple","苹果");
	            word.put("rocket","火箭");
	            word.put("buck","雄鹿");
	            response.setContentType("text/html;chaset=utf-8");
	            if(word.containsKey(key)) {
	            	request.setAttribute("key",word.get(key));
	            	request.getRequestDispatcher("/success.jsp").forward(request, response);
	            }else {
	            	request.getRequestDispatcher("/fail.jsp").forward(request, response);
	            }
	}
```

```html
//首页
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>Insert title here</title>
</head>
<body>
     <form action="/Servlet-Struc/servlet/hash">
       <input type="text" name="input" value="请输入要查询的单词">
       <input type="submit" name="submit" value="查询">
     </form>
</body>
</html>
```

```html
//success.jsp
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>Insert title here</title>
</head>
<body>
       <%
          response.setContentType("text/html;chaset=utf-8");
          String key=(String)request.getAttribute("key");
          out.println(key);
       %>
</body>
</html>
```

```html
//fail.jsp
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>Insert title here</title>
</head>
<body>
     <%  
         response.setContentType("text/html;chaset=utf-8");
         out.println("没有找到");
     %>
</body>
</html>
```

