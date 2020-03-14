## session

### SessionIndexServlet()
   
```java
     protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {  
		HttpSession session=request.getSession();
		String name=(String)session.getAttribute("name");
        //匹配中文字符集
		response.setContentType("text/html;charset=utf-8");
		response.getWriter().println("这是首页，当前用户名为："+name);
```

### SessionLoginServlet()
```java
     protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
		System.out.println("登录成功");
		//获取用户回话session对象
		HttpSession session=request.getSession();
		
		session.setAttribute("name", "张三");
		
		request.getRequestDispatcher("/session/index").forward(request, response);
	}
```