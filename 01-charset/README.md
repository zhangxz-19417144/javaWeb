##   中文字符集的转换

### get
   
   ```java
     protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
		//在get方法中，tomcat8以上的版本自动帮中文变成utf-8进行输出，不需要进行设置
		String ename=request.getParameter("ename");
		String address=request.getParameter("address");
		System.out.println(ename+":"+address);
		//输出的时候，不管是get还是post一定要用这句话来转化中文
		response.setContentType("text/html;charset=utf-8");
		response.getWriter().println(ename+":"+address);
	}
   ```
### post

  ```java
      protected void doPost(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
		//设置全程的字符集为utf-8，而且只能在post方法中用
		request.setCharacterEncoding("utf-8");
		String ename=request.getParameter("ename");
		String address=request.getParameter("address");
     //		String utf8Ename=new String(ename.getBytes("iso-8859-1"),"utf-8");
     //		String utf8Address=new String(address.getBytes("iso-8859-1"),"utf-8");
		System.out.println(ename+":"+address);
	}
  ```