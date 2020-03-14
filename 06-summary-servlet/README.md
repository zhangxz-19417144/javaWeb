##  SUMMARY

**创建第一个servlet**

```java
public class FirstServlet extends HttpServlet{
    public void service(HttpServletRequest request, HttpServletResponse response) throws IOException {
    	 String name=request.getParameter("name");
    	 String html="<h1>"+name+"</h1>";
    	 PrintWriter out=response.getWriter();
    	 out.println(html);
    }
}

```

```xml
//这是第一种servlet的映射方式
<servlet>
       <servlet-name>first</servlet-name>
       <servlet-class>com.imooc.servlet.FirstServlet</servlet-class>
</servlet>
<servlet-mapping>
        <servlet-name>first</servlet-name>
        <url-pattern>/first</url-pattern>
</servlet-mapping>
```

***

**servlet与html的连接**

```java
public class SampleServlet extends HttpServlet{
     public void service(HttpServletRequest request,HttpServletResponse response) throws IOException {
    	 String name=request.getParameter("name");
    	 String mobile=request.getParameter("mobile");
    	 String sex=request.getParameter("sex");
    	 //getParameterValues 是获取数组
    	 String[] specs=request.getParameterValues("spec");
    	 PrintWriter out=response.getWriter();
    	 for(int i=0;i<specs.length;i++) {
    		 out.println("<h1>name:"+specs[i]+"</h1>");
    	 }
    	 out.println("<h1>"+name+"</h1>");
    	 out.println("<h1>"+mobile+"</h1>");
    	 out.println("<h1>"+sex+"</h1>");
    	 out.print("<a href='http://www.baidu.com'>baidu</a>");
     }
}

```

```xml
 <servlet>
       <servlet-name>sample</servlet-name>
       <servlet-class>com.imooc.servlet.SampleServlet</servlet-class>
 </servlet>
 <servlet-mapping>
        <servlet-name>sample</servlet-name>
        <url-pattern>/sample</url-pattern>
 </servlet-mapping>
```

***

**两个servlet之间的跳转**

```java
package com.imooc.servlet.direct;

import java.io.IOException;
import javax.servlet.ServletException;
import javax.servlet.annotation.WebServlet;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;

/**
 * Servlet implementation class CheckLoginServlet
 */
@WebServlet("/direct/check")
public class CheckLoginServlet extends HttpServlet {
	private static final long serialVersionUID = 1L;
       
    /**
     * @see HttpServlet#HttpServlet()
     */
    public CheckLoginServlet() {
        super();
        // TODO Auto-generated constructor stub
    }

	/**
	 * @see HttpServlet#doGet(HttpServletRequest request, HttpServletResponse response)
	 */
	protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
		System.out.println("登录成功");
		//请求转发
//		request.getRequestDispatcher("/direct/index").forward(request, response);
		//重定向,要加上"上下文路径"
		response.sendRedirect("/FirstServlet/direct/index");
	}

}

```

```java
package com.imooc.servlet.direct;

import java.io.IOException;
import javax.servlet.ServletException;
import javax.servlet.annotation.WebServlet;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;

/**
 * Servlet implementation class IndexServlet
 */
@WebServlet("/direct/index")
public class IndexServlet extends HttpServlet {
	private static final long serialVersionUID = 1L;
       
    /**
     * @see HttpServlet#HttpServlet()
     */
    public IndexServlet() {
        super();
        // TODO Auto-generated constructor stub
    }

	/**
	 * @see HttpServlet#doGet(HttpServletRequest request, HttpServletResponse response)
	 */
	protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
		response.getWriter().println("This is index page");
	}

}

```

***

HttpServlet--要使用servlet一定要继承的父类

HttpServletRequest--请求对象

HttpSession--用户会话对象

ServletContext--web应用全局对象

***

**Apache Tomcat**

Tomcat本质上是web服务器，它是运行servlet（服务器小程序）的容器，它只能实现J2EE中servlet和jsp两个功能。

**servlet**

主要是用来生成web动态内容，jsp本质上是servlet，他和tomcat的关系：相当于tomcat提供了硬件基础，servlet提供了软件实现。

具体的关系图见ppt

***

**servlet的开发步骤**

1.创建servlet类，继承HttpServlet

2.重写service方法，编写程序代码(service是doGet和doPost的父类)

3.配置web.xml,绑定url（也可以用注解@webServlet）

***

**请求参数**

1.请求参数是指浏览器通过请求向tomcat提交的数据（请求参数传给tomcat，然后tomcat找到对应的servlet）

2.请求参数通常是用户输入的数据，待servlet进行处理

3.两个类：request.getParameter()--接收单个参数

​                   request.getParameterValues()--接收多个参数

***

**Get和Post的区别**

1.Get常常用于不包含敏感信息的查询功能，因为他会把参数放在url中

2.Post常常用户安全性较高的功能或者服务器的写操作，如：

​    1⃣️用户登录

​    2⃣️用户注册

​    3⃣️更新公司账目

***

**Servlet的生命周期**

装载-web.xml（一启动tomcat就解析web.xml，知道了servlet的存在）

创建-构造函数（当访问的时候才创建，写那个url在地址栏的时候）

初始化-init（）

提供服务-service（）

销毁--destroy（）

***

**启动时加载servlet**

两种方式：1⃣️在注解中加载：onload还有url-pattern

​                   2⃣️在xml加载：<load-on-startup></load-on-startup>,两步：onload（）和init（）

***

包含JSTL EL的函数create

```java     
package com.imooc.employee;
//Employee
public class Employee {
	     private Integer  empno;
	     private  String  ename;
	     private  String  department;
	     private  String  job;
	     private  Float   salary;
		public Employee(Integer empno, String ename, String department, String job, Float salary) {
			super();
			this.empno = empno;
			this.ename = ename;
			this.department = department;
			this.job = job;
			this.salary = salary;
		}
		public Integer getEmpno() {
			return empno;
		}
		public void setEmpno(Integer empno) {
			this.empno = empno;
		}
		public String getEname() {
			return ename;
		}
		public void setEname(String ename) {
			this.ename = ename;
		}
		public String getDepartment() {
			return department;
		}
		public void setDepartment(String department) {
			this.department = department;
		}
		public String getJob() {
			return job;
		}
		public void setJob(String job) {
			this.job = job;
		}
		public Float getSalary() {
			return salary;
		}
		public void setSalary(Float salary) {
			this.salary = salary;
		}
               
}

```

```java
package com.imooc.employee;
//employeeServlet
import java.io.IOException;
import java.util.ArrayList;
import java.util.List;

import javax.servlet.ServletContext;
import javax.servlet.ServletException;
import javax.servlet.annotation.WebServlet;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;

import com.sun.net.httpserver.HttpContext;

/**
 * Servlet implementation class EmployeeServlet
 */
@WebServlet("/emp")
public class EmployeeServlet extends HttpServlet {
	private static final long serialVersionUID = 1L;
       
    /**
     * @see HttpServlet#HttpServlet()
     */
    public EmployeeServlet() {
        super();
        // TODO Auto-generated constructor stub
    }

	/**
	 * @see HttpServlet#doGet(HttpServletRequest request, HttpServletResponse response)
	 */
	protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
		ServletContext context=request.getServletContext();
	if(context.getAttribute("employee") == null) {
		List list=new ArrayList();
		Employee emp=new Employee(1111,"张三"," 游戏部","打游戏",10000f);
		list.add(emp);
		list.add(new Employee(1112,"李四"," 哲学部","研究哲学",10f));
		context.setAttribute("employee", list);
	}
	request.getRequestDispatcher("/employee.jsp").forward(request, response);
		
	}

}

```

```java
package com.imooc.employee;
//CreateServlet
import java.io.IOException;
import java.util.List;

import javax.servlet.ServletContext;
import javax.servlet.ServletException;
import javax.servlet.annotation.WebServlet;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;

/**
 * Servlet implementation class CreateServlet
 */
@WebServlet("/create")
public class CreateServlet extends HttpServlet {
	private static final long serialVersionUID = 1L;
       
    /**
     * @see HttpServlet#HttpServlet()
     */
    public CreateServlet() {
        super();
        // TODO Auto-generated constructor stub
    }

	/**
	 * @see HttpServlet#doPost(HttpServletRequest request, HttpServletResponse response)
	 */
	protected void doPost(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
     request.setCharacterEncoding("utf-8");
	 String empno= request.getParameter("empno");
     String ename= request.getParameter("ename");
     String department= request.getParameter("department");
     String job= request.getParameter("job");
     String salary=  request.getParameter("salary");
     System.out.println(empno);
     Employee emp=new Employee(Integer.parseInt(empno),ename,department,job,Float.parseFloat(salary));
	 ServletContext context=request.getServletContext();
	 List list=(List)context.getAttribute("employee");
	 list.add(emp);
	 context.setAttribute("employee", list);
	 request.getRequestDispatcher("/employee.jsp").forward(request, response);
	}

}

```

```jsp
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
<%@ taglib uri="http://java.sun.com/jsp/jstl/core" prefix="c"%>
<%@ taglib uri="http://java.sun.com/jsp/jstl/fmt" prefix="fmt"%>
<!DOCTYPE html>
<html>
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width,initial-scale=1">
    <title>员工列表</title>
    <link href="css/bootstrap.css" type="text/css" rel="stylesheet"></link>
    
    <script type="text/javascript" src="js/jquery-1.11.1.min.js"></script>
    <script type="text/javascript" src="js/bootstrap.js"></script>

    <style type="text/css">
        .pagination {
            margin: 0px
        }

        .pagination > li > a, .pagination > li > span {
            margin: 0 5px;
            border: 1px solid #dddddd;
        }

        .glyphicon {
            margin-right: 3px;
        }

        .form-control[readonly] {
            cursor: pointer;
            background-color: white;
        }
        #dlgPhoto .modal-body{
            text-align: center;
        }
        .preview{

            max-width: 500px;
        }
    </style>
    <script>
        $(function () {
            
            $("#btnAdd").click(function () {
                $('#dlgForm').modal()
            });
        })


    </script>
</head>
<body>

<div class="container">
    <div class="row">
        <h1 style="text-align: center">IMOOC员工信息表</h1>
        <div class="panel panel-default">
            <div class="clearfix panel-heading ">
                <div class="input-group" style="width: 500px;">
                    <button class="btn btn-primary" id="btnAdd"><span class="glyphicon glyphicon-zoom-in"></span>新增
                    </button>
                </div>
            </div>

            <table class="table table-bordered table-hover">
                <thead>
                <tr>
                    <th>序号</th>
                    <th>员工编号</th>
                    <th>姓名</th>
                    <th>部门</th>
                    <th>职务</th>
                    <th>工资</th>
                    <th>&nbsp;</th>
                </tr>
                </thead>
                <tbody>
                <c:forEach items="${applicationScope.employee}" var="emp" varStatus="idx">
                <tr>
                    <td>${idx.index+1}</td>
                    <td>${emp.empno}</td>
                    <td>${emp.ename}</td>
                    <td>${emp.department}</td>
                    <td>${emp.job}</td>
                    <td style="color: red;font-weight: bold">￥<fmt:formatNumber value="${emp.salary}" pattern="0,000.00"></fmt:formatNumber></td>
                </tr>               
                </c:forEach>
                </tbody>
            </table>
        </div>
    </div>
</div>

<!-- 表单 -->
<div class="modal fade" tabindex="-1" role="dialog" id="dlgForm">
    <div class="modal-dialog" role="document">
        <div class="modal-content">
            <div class="modal-header">
                <button type="button" class="close" data-dismiss="modal" aria-label="Close"><span aria-hidden="true">&times;</span>
                </button>
                <h4 class="modal-title">新增员工</h4>
            </div>
            <div class="modal-body">
                <form action="/Employee/create" method="post" >
                    <div class="form-group">
                        <label for="empno">员工编号</label>
                        <input type="text" name="empno" class="form-control" id="empno" placeholder="请输入员工编号">
                    </div>
                    <div class="form-group">
                        <label for="ename">员工姓名</label>
                        <input type="text" name="ename" class="form-control" id="ename" placeholder="请输入员工姓名">
                    </div>
                    <div class="form-group">
                        <label>部门</label>
                        <select id="dname" name="department" class="form-control">
                            <option selected="selected">请选择部门</option>
                            <option value="市场部">市场部</option>
                            <option value="研发部">研发部</option>
                        	<option value="后勤部">后勤部</option>
                        </select>
                    </div>

                    <div class="form-group">
                        <label>职务</label>
                        <input type="text" name="job" class="form-control" id="sal" placeholder="请输入职务">
                    </div>

                    <div class="form-group">
                        <label for="sal">工资</label>
                        <input type="text" name="salary" class="form-control" id="sal" placeholder="请输入工资">
                    </div>

                    <div class="form-group" style="text-align: center;">
                        <button type="submit" class="btn btn-primary">保存</button>
                    </div>
                </form>
            </div>

        </div><!-- /.modal-content -->
    </div><!-- /.modal-dialog -->
</div><!-- /.modal -->


</body>
</html>
```

