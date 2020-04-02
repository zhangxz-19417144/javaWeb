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

```html
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

