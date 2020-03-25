##  jquery和ajax

#### 选择器

**选择器包括 $(#id)-id选择器    $("标签")-标签选择器   $(".class")-类选择器**

```html
<!DOCTYPE html >

<html>
<head>
<meta charset="UTF-8">
<meta http-equiv="Content-Type" content="text/html; charset=utf-8" />
<title>jQuery实验室</title>

<style>
.myclass {
	font-style: italic;
	color: darkblue;
}
/* 高亮css类 */
.highlight {
	color: red;
	font-size: 30px;
	background: lightblue;
}
</style>

</head>

<body>
	<div class="section">
		<h2>jQuery选择器实验室</h2>
		<input style="height: 24px" id="txtSelector" />
		<button id="btnSelect" style="height: 30px">选择</button>
		<hr />
		<div>
			<p id="welcome">欢迎来到选择器实验室</p>
			<ul>
				<li>搜索引擎：<a href="http://www.baidu.com">百度</a> <span> <a
						style="color: darkgreen" href="http://www.so.com">360</a>
				</span>
				</li>
				<li>电子邮箱：<a href="http://mail.163.com">网易邮箱</a> <span> <a
						style="color: darkgreen" href="http://mail.qq.com">QQ邮箱</a>
				</span>
				</li>
				<li>中国名校：<a href="http://www.tsinghua.edu.cn">清华大学</a> <span>
						<a style="color: darkgreen" href="https://www.pku.edu.cn/">北京大学</a>
				</span>
				</li>
			</ul>

			<span class="myclass ">我是拥有myclass类的span标签</span>

			<p class="myclass">我是拥有myclass的p标签</p>
			<form id="info" action="#" method="get">
				<div>
					用户名：<input type="text" name="uname" value="admin" /> 密码：<input
						type="password" name="upsd" value="123456" />
				</div>
				<div>
					婚姻状况： <select id="marital_status">
						<option value="1">未婚</option>
						<option value="2">已婚</option>
						<option value="3">离异</option>
						<option value="4">丧偶</option>
					</select>
				</div>
				<div class="left clear-left">
					<input type="submit" value="提交" /> <input type="reset" value="重置" />
				</div>
			</form>
		</div>
	</div>
	<script type="text/javascript" src="js/jquery-3.3.1.js"></script>
	<script type="text/javascript">
	      /*
	        id选择器使用#id值
	        类选择器使用.类名
	        $("#marital_status").addClass("highlight");
	        $(".myclass").addClass("highlight");
	      */
	     
	      document.getElementById("btnSelect").onclick=function(){
	    	     var selector=document.getElementById("txtSelector").value;
	    	     $("*").removeClass("highlight");
	    	     $(selector).addClass("highlight"); 
	      }
	</script>
</body>
</html>

```

**属性的操作**

```html
<script type="text/javascript" src="js/jquery-3.3.1.js"></script>
     <script type="text/javascript">
         var href_attr=$("a[href*='163']").attr("href");
         alert(href_attr);
         $("a[href*='163']").attr("href","http://www.baidu.com");
         //输出只会输出第一个网址，不会把所有网址输出
         var att=$("a").attr("href");
         alert(att);
         $("a").removeAttr("href");
     </script>
```

**操作元素的css样式**

```html
<script type="text/javascript" src="js/jquery-3.3.1.js"></script>
	<script type="text/javascript">
	       $("a").css("color","red");
          $("a").css({"color":"red","font-weight":"bold","font-style":"italic"});
          $("li").addClass("highlight");
          $("p").removeClass("myclass");
          //只会显示第一个color
	       var color= $("a").css("color");
	       alert(color);
	</script>
```

 **设置元素内容**

```html
<script type="text/javascript" src="js/jquery-3.3.1.js"></script>
	<script type="text/javascript">
	      //修改输入项的内容为administrator
	      $("input[name='uname']").val("administrator");
	      //获取元素input[name='uname']的输入项的值
	      var name= $("input[name='uname']").val();
	      alert(name);
	      //text不会解释标签，html会解释标签
	     /*  $("span.myclass").text("<br>锄禾日当午，汗滴禾下土</br>"); */
	      $("span.myclass").html("<br>锄禾日当午，汗滴禾下土</br>"); 
	     //在输出框(弹窗)，用html标签也不会被解释，用text只会输入文字，标签会被自动屏蔽
	      var a= $("span.myclass").text(); 
	     /* var a= $("span.myclass").html();  */
	     alert(a);
	</script>
```

**jquery常用事件有：click,keypress,submit,load**

```html
<script type="text/javascript" src="js/jquery-3.3.1.js"></script>
	<script type="text/javascript">
	      //写事件的两种方式，方式一
	      $("p.myclass").on("click",function(){
	    	  $(this).css("color","red");
	      })
	      //写事件的两种方式，方式二
	       $("span.myclass").click(function(){
	    	  $(this).css("background-color","yellow");
	      })
	       $("input[name='uname']").keypress(function(event){
	    	   //32表示空格的意思
	    	   if(event.keyCode==32)
	    	   { $(this).css("color","red");}
	      })
	      
	</script>
```

#### ajax的介绍

##### 1.ajax全称叫 asynchronous javascript and xml

##### 2.可以在不刷新页面的前提下，进行页面的局部更新



##### ajax的使用流程

```html
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>Insert title here</title>
</head>
<body>
     <input id="btnLoad" type="button" value="加载">
     <div id="divContent"></div> 
     <script type="text/javascript">
     document.getElementById("btnLoad").onclick = function(){
         //创建XmlHttpRequest对象
         var xmlhttp;
         //最近的一些浏览器
         if(window.XMLHttpRequest){
        	 xmlhttp=new XMLHttpRequest();
         }else{
         //比较古老的浏览器
        	 xmlhttp=new ActiveXObject("Microsoft.XMLHTTP");
         }
         console.log(xmlhttp);
         //发送ajax请求
            //true代表异步，false代表同步
         xmlhttp.open("GET","/ajax/content",true);
         xmlhttp.send();
         //处理服务器响应
         xmlhttp.onreadystatechange=function(){
        	 //4表示响应文本（/ajax/content里面的内容）已经被接收，200表示响应正常（它是响应状态码）
        	 if(xmlhttp.readyState==4 && xmlhttp.status==200){
        		 //获取响应体的文本
        		 var t=xmlhttp.responseText;
        		 alert(t);
        		 //把响应结果进行显示在浏览器中
        		 document.getElementById("divContent").innerHTML=t;
        	 }
         }
     }
     </script>
</body>
</html>
```

```java
@WebServlet("/content")
protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
		response.getWriter().println("<p style='color:blue'>xixi</p>");
	}
```

**sample1**

```java
@WebServlet("/exe")
protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
		List<Employee> list=new ArrayList<>();
		list.add(new Employee("小红","职员","人事部"));
		list.add(new Employee("小明","经理","技术部"));
		list.add(new Employee("小白"," 。。。","无线事业部"));
		String json=JSON.toJSONString(list);
		System.out.println(json);
		response.setContentType("text/html;charset=utf-8");
		response.getWriter().print(json);
	}
```

```java
package com.imooc.ajax;

public class Employee {
     private String name;
     private String job;
     private String department;
     
	public Employee(String name, String job, String department) {
		super();
		this.name = name;
		this.job = job;
		this.department = department;
	}
	public String getName() {
		return name;
	}
	public void setName(String name) {
		this.name = name;
	}
	public String getJob() {
		return job;
	}
	public void setJob(String job) {
		this.job = job;
	}
	public String getDepartment() {
		return department;
	}
	public void setDepartment(String department) {
		this.department = department;
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
    <input id="btnEmp" type="button" value="员工列表" >
     <input id="b" type="button" value="职员列表" >
      <input type="button" value="部门列表" id="c">
      <div id="divContent"></div> 
    
      <script type="text/javascript">
      
      //创建XmlHttpRequest对象
      var xmlhttp;
      var html1="";   
      var html2="";
      var html3="";  
      if(window.XMLHttpRequest){
      	xmlhttp=new XMLHttpRequest();
      }else{
      	xmlhttp=new ActiveXObject("Microsoft.XMLHTTP");
      }  
      //发送ajax请求
        xmlhttp.open("GET","/ajax/exe",true);
        xmlhttp.send(); 
      //处理服务器响应
       xmlhttp.onreadystatechange = function(){
 	  if(xmlhttp.readyState == 4 && xmlhttp.status == 200){
 		 //获取响应体的文本
 		 var t=xmlhttp.responseText;
 		 //把响应结果进行显示在浏览器中
 		 var json=JSON.parse(t);
 		
 		 for(var i=0;i<json.length;i++){
 			 var emp = json[i];
 			 html1=html1+"<h1>"+emp.name+"</h1>";
 			 html2=html2+"<h1>"+emp.job+"</h1>";
 			  html3=html3+"<h1>"+emp.department+"</h1>";  
 		 } 
 		
 	  }
 	  }
 	 document.getElementById("btnEmp").onclick=function(){
   	    document.getElementById("divContent").innerHTML=html1;
        }  
 	 document.getElementById("b").onclick=function(){
    	    document.getElementById("divContent").innerHTML=html2;
         }  
   	  document.getElementById("c").onclick=function(){
    	    document.getElementById("divContent").innerHTML=html3;
         }    
      </script>
</body>
</html>
```

**ajax和jqury配合使用**

```java
package com.imooc.ajax;

public class news {
    private String title;
    private String date;
    private String source;
    private String content;
    public news() {
    	
    }
	public news(String title, String date, String source, String content) {
		super();
		this.title = title;
		this.date = date;
		this.source = source;
		this.content = content;
	}
	public String getTitle() {
		return title;
	}
	public void setTitle(String title) {
		this.title = title;
	}
	public String getDate() {
		return date;
	}
	public void setDate(String date) {
		this.date = date;
	}
	public String getSource() {
		return source;
	}
	public void setSource(String source) {
		this.source = source;
	}
	public String getContent() {
		return content;
	}
	public void setContent(String content) {
		this.content = content;
	}
}

```

```java
package com.imooc.ajax;

import java.io.IOException;
import java.util.ArrayList;
import java.util.List;

import javax.servlet.ServletException;
import javax.servlet.annotation.WebServlet;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;

import com.alibaba.fastjson.JSON;

/**
 * Servlet implementation class newServlet
 */
@WebServlet("/news")
public class newServlet extends HttpServlet {
	private static final long serialVersionUID = 1L;
       
    /**
     * @see HttpServlet#HttpServlet()
     */
    public newServlet() {
        super();
        // TODO Auto-generated constructor stub
    }

	/**
	 * @see HttpServlet#doGet(HttpServletRequest request, HttpServletResponse response)
	 */
	protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
		String type=request.getParameter("t");
		List<news> list=new ArrayList<>();
		
		if(type!=null && type.equals("nba")) {
			list.add(new news("nba","5月份","不知道从哪里来",".........."));
			list.add(new news("nba","6月份","不知道从哪里来",".........."));
			list.add(new news("nba","7月份","不知道从哪里来",".........."));
			list.add(new news("nba","8月份","不知道从哪里来",".........."));
		}else if(type == null || type.equals("cba")) {
			list.add(new news("cba","5月份","不知道从哪里来",".........."));
			list.add(new news("cba","6月份","不知道从哪里来",".........."));
			list.add(new news("cba","7月份","不知道从哪里来",".........."));
			list.add(new news("cba","8月份","不知道从哪里来",".........."));
		}
		
		//这个JSON是fastjson的，它是把字符串转换成json对象
		String json=JSON.toJSONString(list);
		System.out.println(json);
		response.setContentType("text/html;charset=utf-8");
		response.getWriter().print(json);
	}

}

```

```html
//jquery和ajax的配合使用
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>Insert title here</title>
</head>
<body>
<div id="contain"></div>
<script type="text/javascript">
     //创建xmlhttprequest对象
     var xmlhttp;
     if(window.XMLHttpRequest){
    	 xmlhttp=new XMLHttpRequest();
     }else{
    	 xmlhttp=new ActiveXObject("Microsoft.XMLHTTP");
     }
     //ajax请求
     xmlhttp.open("GET","/ajax/news",true);
     xmlhttp.send();
     //处理服务器响应
     xmlhttp.onreadystatechange=function(){
    	 if(xmlhttp.readyState==4 && xmlhttp.status==200){
    		 //获取响应体的文本
    		 var t=xmlhttp.responseText;
    		 console.log(t);
    		 //这个JSON是javascript自带的，把字符串格式转换成javascript可以识别的json格式
    		 var json=JSON.parse(t);
    		 console.log(json);
    		 var html="";
    		 for(var i=0;i<json.length;i++){
    			 var news = json[i];
    			 html=html+"<h1>"+news.title+"</h1>";
    			 html=html+"<h2>"+news.date+"&nbsp;"+news.source+"</h2>";
    			 html=html+"</hr>"
    		 }
    		 document.getElementById("contain").innerHTML=html; 
    	 }
     }
</script>
</body>
</html>
```

```html
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>Insert title here</title>
</head>
<script type="text/javascript" src="js/jquery-3.3.1.js"></script>
<script type="text/javascript">
    $(function(){
     	$.ajax({
    		"url":"/ajax/news",
    	    "type":"get",
    	    "data":"t=cba",//一般采用{"t":"cba","uu":"lll"}
    	    "dataType":"json",
    	    "success":function(json){
    	    	for(var i=0;i<json.length;i++){
    	    		$("#container").append("<h1>"+json[i].title+"</h1>");
    	    	}
    	    },
    	    "error":function(xmlhttp,errorText){
    	    	console.log(xmlhttp);
    	    	console.log(errorText);
    	    	if(xmlhttp.status == "405"){
    	    		alert("无效的请求方式");
    	    	}else if(xmlhttp.status == "404"){
    	    		alert("未找到url资源");
    	    	}else if(xmlhttp.status == "500"){
    	    		alert("服务器内部错误");
    	    	}else{
    	    		alert("产生异常");
    	    	}
    	    }
    	}) 
    })
</script>
<body>
<div id="container"></div>
</body>
</html>
```

**sample1(json可以根据第几批输入，用i来控制顺序依次输出)**

```java
@WebServlet("/song")
protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
		
		 List list=new ArrayList();
		 list.add("<h1>稻香<br>晴天<br>告白气球</h1>");
	     list.add("<h1>千千阙歌<br>傻女<br>七友</h1>");
	     list.add("<h1>一块红布<br>假行僧<br>新长征路上的摇滚</h1>");
	     String json=JSON.toJSONString(list);
	     response.setContentType("text/html;charset=utf-8");
	     response.getWriter().print(json);
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
     <input id="lx" type="button" value="流行歌曲">
     <input id="jd" type="button" value="经典歌曲">
     <input id="yg" type="button" value="摇滚歌曲">
     <div id="container"></div>
     <script type="text/javascript" src="js/jquery-3.3.1.js"></script>
     <script type="text/javascript">
     //json可以根据装入的顺序一批一批的输出，太棒了。。。。
      $("#lx").click(function(){
    	  f(0);
      });
       $("#jd").click(function(){
    	  f(1);
      });
      $("#yg").click(function(){
    	  f(2);
      }); 
      function f(i){
    	  $.ajax({
    		 "url":"/ajax/song",
    		 "type":"get",
    		 "dataType":"json",
    		 "success":function(json){
    			 document.getElementById("container").innerHTML=json[i];
    			
    		 }
    	  })
      }
</script>
</body>
</html>
```

**二级联动**

```java
package com.imooc.ajax;

import java.io.IOException;
import java.util.ArrayList;
import java.util.List;

import javax.servlet.ServletException;
import javax.servlet.annotation.WebServlet;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;

import com.alibaba.fastjson.JSON;

/**
 * Servlet implementation class ChannelServlet
 */
@WebServlet("/ch")
public class ChannelServlet extends HttpServlet {
	private static final long serialVersionUID = 1L;
       
    /**
     * @see HttpServlet#HttpServlet()
     */
    public ChannelServlet() {
        super();
        // TODO Auto-generated constructor stub
    }

	/**
	 * @see HttpServlet#doGet(HttpServletRequest request, HttpServletResponse response)
	 */
	protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
		String level=request.getParameter("level");
		String parent=request.getParameter("parent");
		List<Channel> list=new ArrayList<>();
	    if(level.equals("1")) {
	    	list.add(new Channel("ai","人工智能"));
	    	list.add(new Channel("web","前端小程序"));
	    }else if(level.equals("2")) {
	    	if(parent.equals("ai")) {
	    		list.add(new Channel("micro","微服务"));
		    	list.add(new Channel("blockchain","区块链"));
		    	list.add(new Channel("other","其他"));
	    	}else if(parent.equals("web")) {
	    		list.add(new Channel("html","HTML"));
		    	list.add(new Channel("css","CSS"));
		    	list.add(new Channel("other","其他"));
	    	}
	    }
	      String json = JSON.toJSONString(list);
	      response.setContentType("text/html;charset=utf-8");
	      response.getWriter().println(json);
		
	}

}

```

```java
package com.imooc.ajax;

public class Channel {
    private String code;
    private String name;
    
    public Channel() {
    	
    }
    
	public Channel(String code, String name) {
		super();
		this.code = code;
		this.name = name;
	}
	public String getCode() {
		return code;
	}
	public void setCode(String code) {
		this.code = code;
	}
	public String getName() {
		return name;
	}
	public void setName(String name) {
		this.name = name;
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
<script type="text/javascript" src="js/jquery-3.3.1.js"></script>
<script type="text/javascript">
      $(function(){
    	  $.ajax({
    		  "url":"/ajax/ch",
    	      "type":"get",
    	      "data":{"level":"1"},
    	      "dataType":"json",
    	      "success":function(json){
    	    	  console.log(json);
    	    	  for(var i=0;i<json.length;i++){
    	    		  var ch=json[i];
    	    		  $("#lv1").append("<option  value='"+ch.code+"'>"+ch.name+"</option>");
    	    	  }
    	      }
    	  })
      })
      $(function(){
    	  $("#lv1").on("change",function(){
    		  var parent=$(this).val();
    		  console.log(parent);
    		  $.ajax({
    			  "url":"/ajax/ch",
        	      "type":"get",
        	      "data":{"level":"2","parent":parent},
        	      "dataType":"json",
        	      "success":function(json){
        	    	  $("#lv2>option").remove();
        	    	  for(var i=0;i<json.length;i++){
        	    		  var ch=json[i];
        	    		  $("#lv2").append("<option  value='"+ch.code+"'>"+ch.name+"</option>");
        	    	  }
        	      }
    		  })
    	  })
      })
</script>
<body>
    <Select id="lv1" style="width:200px;height:30px">
     <option select="selected">请选择</option>
    </Select>
    <Select id="lv2" style="width:200px;height:30px"></Select>
</body>
</html>
```



