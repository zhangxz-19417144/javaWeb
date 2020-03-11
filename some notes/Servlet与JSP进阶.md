**http的请求结构**

​                       请求头：user-agent（多端应用底层的实现）

​					   请求行：POST/GET ; URL;HTTP协议版本

​                       请求体：url的参数

***

**http的响应结构**

 						响应行：版本协议及版本；状态码及状态描述（200，404，500）

​                         响应头：content-type:text/html(paintext):决定以哪种方式响应

​                         响应体：<html>....</html>

***

**post和get的区别**

  					 get的请求参数会出现在url中，而post不会出现，安全性高

***

**请求转发和重定向**

​                       request.getRequestDispatcher("/...").forword(request,response)---请求转发--发生一次请求

​                       response.sendRedict()-----重定向--发生两次请求

***

**设置和获得请求属性**

​                       request.setAttribute(name,value)

​                       request.getAttribute(name)     

***

**cookie(生命长度--浏览器关闭之前，也可以设置生存时间)**

​		                是浏览器保存在本地的文本内容

​        				常用语保存登录状态，用户资料等小文本

​						具有时效性，伴随着请求发送给tomcat

***

**session(生命长度--无人用的情况下30分钟)**

​     			   	资料保存在服务器中

​                        <u>浏览器关闭再打开时，就会变成两一个session的内容，这和cookie不一样，cookie直接就销毁了</u>

​						<u>而session他仍在服务器中，一直有该session的空间在</u>

​                        session通过cookie的session-id的值提取用户数据

  					  ps：参考ppt          

***

**ServletContext**

​						不会随着浏览器的关闭而消失（具体操作看web-xml）

​						具体操作看web-xml

***

**JSP九大内置对象**

