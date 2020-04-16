## Freemarker

1.它是免费的开源的模版引擎技术

2.Freemarker具体写成：Freemarker Template Language

3.它提供了大量的内建函数来简化开发

#### sample1

```java
package com.imooc.freemarker.entity;

import java.util.Date;
import java.util.Map;

public class Computer {
    private String sn;//序列号
    private String model;//型号
    private int state;  //状态1-在用 2-闲置 3-报废
    private String user;//使用人
    private Date dop;   //采购时间
    private Float price;//采购价格
    private Map info;//电脑配置
    
    
	public Computer(String sn, String model, int state, String user, Date dop, Float price, Map info) {
		super();
		this.sn = sn;
		this.model = model;
		this.state = state;
		this.user = user;
		this.dop = dop;
		this.price = price;
		this.info = info;
	}
	public String getSn() {
		return sn;
	}
	public void setSn(String sn) {
		this.sn = sn;
	}
	public String getModel() {
		return model;
	}
	public void setModel(String model) {
		this.model = model;
	}
	public int getState() {
		return state;
	}
	public void setState(int state) {
		this.state = state;
	}
	public String getUser() {
		return user;
	}
	public void setUser(String user) {
		this.user = user;
	}
	public Date getDop() {
		return dop;
	}
	public void setDop(Date dop) {
		this.dop = dop;
	}
	public Float getPrice() {
		return price;
	}
	public void setPrice(Float price) {
		this.price = price;
	}
	public Map getInfo() {
		return info;
	}
	public void setInfo(Map info) {
		this.info = info;
	}
    
}

```



```java
package com.imooc.freemarker;

import java.io.IOException;
import java.io.OutputStreamWriter;
import java.util.Date;
import java.util.HashMap;
import java.util.Map;

import com.imooc.freemarker.entity.Computer;

import freemarker.core.ParseException;
import freemarker.template.Configuration;
import freemarker.template.MalformedTemplateNameException;
import freemarker.template.Template;
import freemarker.template.TemplateException;
import freemarker.template.TemplateNotFoundException;

public class FreemarkerSample1 {

	public static void main(String[] args) throws TemplateNotFoundException, MalformedTemplateNameException, ParseException, IOException, TemplateException {
		//1.加载模版
		//创建核心配置对象
		Configuration config=new Configuration(Configuration.VERSION_2_3_28);
		//设置加载目录:在FreemarkerSample1.class所在的包中加载ftl，“”双引号的意思是当前包
		config.setClassForTemplateLoading(FreemarkerSample1.class, "");
		//得到模版对象
		Template t=config.getTemplate("sample1.ftl");
		//2.创建数据
		Map<String,Object> data=new HashMap<String,Object>();
		data.put("site", "百度");
		data.put("url","http://www.baidu.com");
		data.put("date",new Date());
		data.put("number","123456.89999");
		Map<String,Object> info=new HashMap<String,Object>();
		info.put("cpu","i5");
		Computer computer=new Computer("01","mac",2,"张协圳",new Date(),12999f,info);
		data.put("computer",computer);
		//3.产生输出:把字节流转换成字符流 outputStreamWriter
		t.process(data, new OutputStreamWriter(System.out));
        
	}

}

```

```java
${site}-${url}
<#-- !默认值-->
${name!"张伟华"}
<#-- ?string：格式化数字或日期 -->
${date?string("yyyy年MM月dd日")}
${number}

SN:${computer.sn}
model:${computer.model}
<#if computer.state == 1>
state:正在使用
<#elseif computer.state == 2>
state:闲置
<#elseif computer.state == 3>
state:已作废
</#if>
       <#switch computer.state>
       <#case 1>
            state:正在使用
       <#break>
       <#case 2>
            state:闲置
       <#break>
       <#case 3>
            state:已作废
       <#break>
       <#default>
           state:无状态
        </#switch>
<#if computer.user??>
user:${computer.user}
</#if>
dop:${computer.dop?string("yyyy年MM月dd日")}
price:${computer.price?string("0.00")}

<#-- map的输出 -->
info:${computer.info["cpu"]}
<#--${conputer.info["memory"]!"不存在"}-->
```

#### sample2

```java
package com.imooc.freemarker;

import java.io.IOException;
import java.io.OutputStreamWriter;
import java.util.ArrayList;
import java.util.Date;
import java.util.HashMap;
import java.util.LinkedHashMap;
import java.util.List;
import java.util.Map;

import com.imooc.freemarker.entity.Computer;

import freemarker.core.ParseException;
import freemarker.template.Configuration;
import freemarker.template.MalformedTemplateNameException;
import freemarker.template.Template;
import freemarker.template.TemplateException;
import freemarker.template.TemplateNotFoundException;

public class FreemarkerSample2 {

	public static void main(String[] args) throws TemplateNotFoundException, MalformedTemplateNameException, ParseException, IOException, TemplateException {
		//1.加载模版
		//创建核心配置对象
		Configuration config=new Configuration(Configuration.VERSION_2_3_28);
		//设置加载目录:在FreemarkerSample1.class所在的包中加载ftl，“”双引号的意思是当前包
		config.setClassForTemplateLoading(FreemarkerSample2.class, "");
		//得到模版对象
		Template t=config.getTemplate("sample2.ftl");
		//2.创建数据
		Map<String,Object> data=new HashMap<String,Object>();
		List<Computer> list=new ArrayList<>();
		list.add(new Computer("1","mac1",1,"hhhh",new Date(),2222f,new HashMap()));
		list.add(new Computer("2","mac2",2,"hhhh",new Date(),2222.333f,new HashMap()));
		list.add(new Computer("3","mac3",3,"hhhh",new Date(),2222f,new HashMap()));
		list.add(new Computer("1","mac1",1,"hhhh",new Date(),2222f,new HashMap()));
		data.put("computer", list);
		//LinkedHashMap保证hashmap按顺序输出
		Map computer_map=new LinkedHashMap();
		for(Computer c:list) {
			computer_map.put(c.getSn(),c);
		}
		data.put("computer_map", computer_map);
		//3.产生输出:把字节流转换成字符流 outputStreamWriter
		t.process(data, new OutputStreamWriter(System.out));
        
	}

}

```

```java
<!-- list迭代map-->
<#list computer as computer>
      SN:${computer.sn}
model:${computer.model}
<#if computer.state == 1>
state:正在使用
<#elseif computer.state == 2>
state:闲置
<#elseif computer.state == 3>
state:已作废
</#if>    
<#if computer.user??>
user:${computer.user}
</#if>
dop:${computer.dop?string("yyyy-MM-dd")}
price:${computer.price?string("0.00")}
---------------------------------------------
</#list>
###############################################
<#list computer_map?keys as k>
${k}-${computer_map[k].model}
${computer_map[k].price}
</#list>

```

#### sample3

```java
package com.imooc.freemarker;

import java.io.IOException;
import java.io.OutputStreamWriter;
import java.util.ArrayList;
import java.util.Date;
import java.util.HashMap;
import java.util.LinkedHashMap;
import java.util.List;
import java.util.Map;

import com.imooc.freemarker.entity.Computer;

import freemarker.core.ParseException;
import freemarker.template.Configuration;
import freemarker.template.MalformedTemplateNameException;
import freemarker.template.Template;
import freemarker.template.TemplateException;
import freemarker.template.TemplateNotFoundException;

public class FreemarkerSample3 {

	public static void main(String[] args) throws TemplateNotFoundException, MalformedTemplateNameException, ParseException, IOException, TemplateException {
		//1.加载模版
		//创建核心配置对象
		Configuration config=new Configuration(Configuration.VERSION_2_3_28);
		//设置加载目录:在FreemarkerSample1.class所在的包中加载ftl，“”双引号的意思是当前包
		config.setClassForTemplateLoading(FreemarkerSample3.class, "");
		//得到模版对象
		Template t=config.getTemplate("sample3.ftl");
		//2.创建数据
		Map<String,Object> data=new HashMap<String,Object>();
		data.put("name","abby");
		data.put("brand","hhhhh");
		data.put("word","first blood");
		List<Computer> list=new ArrayList<>();
		list.add(new Computer("1","mac1",1,"hhhh",new Date(),2222f,new HashMap()));
		list.add(new Computer("2","mac2",2,"hhhh",new Date(),2222.333f,new HashMap()));
		list.add(new Computer("3","mac3",3,"hhhh",new Date(),2222f,new HashMap()));
		list.add(new Computer("1","mac5",1,"hhhh",new Date(),2222f,new HashMap()));
		data.put("computer", list);
		//3.产生输出:把字节流转换成字符流 outputStreamWriter
		t.process(data, new OutputStreamWriter(System.out));
        
	}

}

```

```java
${name?cap_first}
${brand?length}
${brand?upper_case}
${word?replace("first","hhhh")}
${word?index_of("first")}
<#-- 这个公式是，如果判断成立就输出前面的，不成立就输出后面的，这个indexof等于-1的时候就不包含该字符串-->
${(word?index_of("first") != -1) ?string("包含","不包含")}
一共有${computer?size}台电脑
    第${computer?first.sn}台
    ${computer?last.model}
    
    <#-- sort_by("price")?reverse降序 -->
  <#list computer?sort_by("price")?reverse as c>
      ${c.sn}-${c.price}
  </#list>
```

#### ftl在web的应用

```java
package com.imooc.servlet;

import java.io.IOException;
import java.util.ArrayList;
import java.util.List;

import javax.servlet.ServletException;
import javax.servlet.annotation.WebServlet;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;

/**
 * Servlet implementation class EmployeeServlet
 */
@WebServlet("/em")
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
		List list=new ArrayList();
		list.add(new Employee(01,"张协圳","营销部","经理",50000f));
		list.add(new Employee(02,"张伟华","国防部","将军",30000f));
		request.setAttribute("employee",list);
		request.getRequestDispatcher("/employee.ftl").forward(request, response);
	}

}

```

```ftl

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
</head>
<body>

<div class="container">
    <div class="row">
        <h1 style="text-align: center">IMOOC员工信息表</h1>
        <div class="panel panel-default">
            <div class="clearfix panel-heading ">
                <div class="input-group" style="width: 500px;">
                    
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
                <#list employee as em>
                <tr>
                    <td>${em_index+1}</td>
                    <td>${em.empno}</td>
                    <td>${em.ename}</td>
                    <td>${em.department}</td>
                    <td>${em.job}</td>
                    <td style="color: red;font-weight: bold">￥${em.salary?string("0.00")}</td>
                </tr>      
                </#list> 
                </tbody>
            </table>
        </div>
    </div>
</div>

</body>
</html>

```





