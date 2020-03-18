##  JSON入门

**一些重要的概念 **

1.全称：javascript object notation

2.json是轻量级的文本数据交换格式

3.json独立于语言，具有自我描述性，更容易理解

4.语法规则：不同对象之间用逗号隔开，eg：customer=[{cname="xxx"},{cname="xxx"}]

​                      相同对象可以放在一个花括号里面，eg：customer=[{cname="xxx",cno=111....}]



**json与字符串的相互转换**

JSON.parse():将字符串转换为json对象

JSON.stringify():将json对象转换为字符串

```json
[
	{
		"empno": 1111,
		"ename": "eddie",
		"job": "programmer",
		"salary": 40000
	},
	
	{
		"empno": 1112,
		"ename": "jack",
		"job": "president",
		"salary": 40000,
		"customer":[{"cname":"james"},{"cname":"kobe"}]
	}
]
```

```html
//sample1
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>Insert title here</title>
</head>
<script>
     var json=[
    		{
    			"empno": 1111,
    			"ename": "eddie",
    			"job": "programmer",
    			"salary": 40000
    		},
    		
    		{
    			"empno": 1112,
    			"ename": "jack",
    			"job": "president",
    			"salary": 40000,
    			"customer":[{"cname":"james"},{"cname":"kobe"}]
    		}
    	];
    	console.log(json);
        for(var i=0;i<json.length;i++){
    		var emp=json[i];
    		document.write("<h1>");
    		document.write(emp.empno);
    		document.write(","+emp.ename);
    		document.write(","+emp.job);
    		document.write(","+emp.salary);
            document.write("</h1>");	
              if(emp.customer!=null){
            	  document.write("<h1>");
        	for(var j=0;j<emp.customer.length;j++){
        		       var customer= emp.customer[i];
            	document.write(customer.cname+",");
            	
        	}
        	document.write("</h1>");
        }
        
    	} 
        
</script>
<body>

</body>
</html>
```

```html
//sample2
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>JSON 转化成字符串</title>
<script type="text/javascript">
     var str="{\"class_name\":\"ddd\"}";
     var json=JSON.parse(str);
      console.log(json);
</script>
</head>

<body>

</body>
</html>
```

```html
//sample3
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>字符串转化成json</title>
 <script type="text/javascript">
       var json1={"class_name":"niubi"};
       var str=JSON.stringify(json1);
       console.info(str);
       console.info(json1);
       var json2={};
       json2.name="hhh";
       json2.eno=12;
       json2.department="jjj";
       console.info(json2);
</script>
</head>
<body>

</body>
</html>
```



**json与java交互**

常见的java的json工具包有FastJson,Jackson,Gson...

FastJson的序列化，反序列化和json注解

```java
package json;

import java.util.Date;

import com.alibaba.fastjson.annotation.JSONField;

public class Employee {
   private Integer empno;
   private String ename;
   private String job;
   @JSONField(name="hiredate" ,format="yyyy-MM-dd HH:mm:ss")
   private Date hdate;
   private Float salary;
   @JSONField(serialize=false)
   private String dname;
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
public String getJob() {
	return job;
}
public void setJob(String job) {
	this.job = job;
}
public Date getHdate() {
	return hdate;
}
public void setHdate(Date hdate) {
	this.hdate = hdate;
}
public Float getSalary() {
	return salary;
}
public void setSalary(Float salary) {
	this.salary = salary;
}
public String getDname() {
	return dname;
}
public void setDname(String dname) {
	this.dname = dname;
}
   
}

```

```java
//单个对象的序列化和反序列化
package json;

import java.util.Calendar;

import com.alibaba.fastjson.JSON;

public class FastJsonSample1 {

	public static void main(String[] args) {
		Employee emp=new Employee();
		emp.setEmpno(1111);
		emp.setEname("zhangxiezhen");
		emp.setJob("player");
		emp.setDname("program");
		emp.setSalary(44000f);
		Calendar c=Calendar.getInstance();
		c.set(2020, 3, 18, 8, 14, 20);
		emp.setHdate(c.getTime());
		String json=JSON.toJSONString(emp);
		System.out.println(json);
		Employee employee=JSON.parseObject(json,Employee.class);
		System.out.println(employee.getJob());
		

	}

}

```



```java
//字符串的序列化和反序列化
package json;

import java.util.ArrayList;
import java.util.List;

import com.alibaba.fastjson.JSON;

public class FastJsonSample2 {

	public static void main(String[] args) {
		// TODO Auto-generated method stub
		List list=new ArrayList();
		for(int i=0;i<100;i++) {
			Employee emp=new Employee();
			emp.setEmpno(1111+i);
			emp.setEname("员工"+i);
			list.add(emp);
		}
		String json=JSON.toJSONString(list);
		System.out.println(json);
      List<Employee> emp1=JSON.parseArray(json,Employee.class);
      for(Employee e:emp1) {
    	  System.out.println(e.getEname()+":"+e.getEmpno());
      }
	}

}

```



