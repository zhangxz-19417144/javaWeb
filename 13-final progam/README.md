这次是第一次采用mvc的模式，在javaweb基础中，

mvc的意思是：m-model----javabean

​                           v-view----jsp

​                           c-control----servlet

项目java有六个包

​                     domain-放javabean 

​                     service-放接口（处理数据的接口）

​                     service.impl-放实现接口的类

​                     utils-放个别特殊类，如这个项目实现了jpg前面的东西乱码

​                     listener-初始化数据，使数据一直都在（除非项目暂停）

​                     servlet-处理逻辑，作为control

三个jsp：login.jsp,register.jsp,header.jsp

                  ```java
package com.imooc.domain;

public class User {
      private String name;
      private String password;
      private String path;
      
	public String getName() {
		return name;
	}
	public void setName(String name) {
		this.name = name;
	}
	public String getPassword() {
		return password;
	}
	public void setPassword(String password) {
		this.password = password;
	}
	public String getPath() {
		return path;
	}
	public void setPath(String path) {
		this.path = path;
	}
	@Override
	public String toString() {
		return "User [name=" + name + ", password=" + password + ", path=" + path + "]";
	}
	
      
}
                  ```

```java
package com.imooc.service;

import java.util.List;

import com.imooc.domain.User;

public interface UserService {
       public void regist(List<User> userList,User user);

	public User login(List<User> userList,User user);
}
            
```

```java
package com.imooc.service.impl;

import java.util.List;

import com.imooc.domain.User;
import com.imooc.service.UserService;

public class UserServiceImpl implements UserService{
      
	 @Override
	 public void regist(List<User> userList,User user) {
		 userList.add(user);
	 }

	@Override
	public User login(List<User> userList, User user) {
		
		for(User existUser:userList) {
			if(existUser.getUsername().equals(user.getUsername())&&existUser.getPassword().equals(user.getPassword()))
				{
				   return existUser;
				}
		}
		return null;
	}
}

```

```java
package com.imooc.utils;

import java.util.UUID;

public class UploadUtils {
       public static String getUuidFileName(String fileName) {
    	   //解决文件重名的问题
    	   //a.jpg ---获得后缀名.jpg--生成一段随机字符串将a替换成sdasdasd.jpg
    	   int idx=fileName.lastIndexOf(".");
    	   //从idx开始截
    	   String subName=fileName.substring(idx);
    	  String uuidFileName= UUID.randomUUID().toString().replace("-", "")+subName;
    	  return uuidFileName;
    	   
       }
       public static void main(String[] args) {
    	   System.out.println(UploadUtils.getUuidFileName("a.jpg"));
       }
}

```

```java
package com.imooc.web.listener;

import java.util.ArrayList;
import java.util.List;

import javax.servlet.ServletContextEvent;
import javax.servlet.ServletContextListener;
import javax.servlet.annotation.WebListener;

import com.imooc.domain.User;

@WebListener
public class InitServletContextListener implements ServletContextListener {

    public InitServletContextListener() {
        // TODO Auto-generated constructor stub
    }

	/**
     * @see ServletContextListener#contextDestroyed(ServletContextEvent)
     */
    public void contextDestroyed(ServletContextEvent sce)  { 
         // TODO Auto-generated method stub
    
    }

	/**
     * @see ServletContextListener#contextInitialized(ServletContextEvent)
     */
    public void contextInitialized(ServletContextEvent sce)  { 
    	 System.out.println("数据初始化中。。。。");
         //创建一个用于保存User的集合
    	 List<User> userList=new ArrayList<>(); 
    	//将list集合放在servletcontext域中
    	 sce.getServletContext().setAttribute("userList", userList);
    }
	
}

```

```java
package com.imooc.web.servlet;

import java.awt.Color;
import java.awt.Font;
import java.awt.Graphics;
import java.awt.Graphics2D;
import java.awt.image.BufferedImage;
import java.io.IOException;
import java.util.Random;

import javax.imageio.ImageIO;
import javax.servlet.ServletException;
import javax.servlet.annotation.WebServlet;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;

/**
 * 生成验证码图片
 */
@WebServlet("/CheckImgServlet")
public class CheckImgServlet extends HttpServlet {

	public void doGet(HttpServletRequest request, HttpServletResponse response)
			throws ServletException, IOException {

		int width = 120;
		int height = 30;

		// 步骤一 绘制一张内存中图片
		BufferedImage bufferedImage = new BufferedImage(width, height,
				BufferedImage.TYPE_INT_RGB);

		// 步骤二 图片绘制背景颜色 ---通过绘图对象
		Graphics graphics = bufferedImage.getGraphics();// 得到画图对象 --- 画笔
		// 绘制任何图形之前 都必须指定一个颜色
		graphics.setColor(getRandColor(200, 250));
		graphics.fillRect(0, 0, width, height);

		// 步骤三 绘制边框
		graphics.setColor(Color.WHITE);
		graphics.drawRect(0, 0, width - 1, height - 1);

		// 步骤四 四个随机数字
		Graphics2D graphics2d = (Graphics2D) graphics;
		// 设置输出字体
		graphics2d.setFont(new Font("宋体", Font.BOLD, 18));
		//生成一个存随机数的字符串
		StringBuffer sBuffer=new StringBuffer();

		String words =
		 "ABCDEFGHIJKLMNOPQRSTUVWXYZabcdefghijklmnopqrstuvwxyz1234567890";
		// String words = "\u7684\u4e00\u4e86\u662f\u6211\u4e0d\u5728\u4eba\u4eec\u6709\u6765\u4ed6\u8fd9\u4e0a\u7740\u4e2a\u5730\u5230\u5927\u91cc\u8bf4\u5c31\u53bb\u5b50\u5f97\u4e5f\u548c\u90a3\u8981\u4e0b\u770b\u5929\u65f6\u8fc7\u51fa\u5c0f\u4e48\u8d77\u4f60\u90fd\u628a\u597d\u8fd8\u591a\u6ca1\u4e3a\u53c8\u53ef\u5bb6\u5b66\u53ea\u4ee5\u4e3b\u4f1a\u6837\u5e74\u60f3\u751f\u540c\u8001\u4e2d\u5341\u4ece\u81ea\u9762\u524d\u5934\u9053\u5b83\u540e\u7136\u8d70\u5f88\u50cf\u89c1\u4e24\u7528\u5979\u56fd\u52a8\u8fdb\u6210\u56de\u4ec0\u8fb9\u4f5c\u5bf9\u5f00\u800c\u5df1\u4e9b\u73b0\u5c71\u6c11\u5019\u7ecf\u53d1\u5de5\u5411\u4e8b\u547d\u7ed9\u957f\u6c34\u51e0\u4e49\u4e09\u58f0\u4e8e\u9ad8\u624b\u77e5\u7406\u773c\u5fd7\u70b9\u5fc3\u6218\u4e8c\u95ee\u4f46\u8eab\u65b9\u5b9e\u5403\u505a\u53eb\u5f53\u4f4f\u542c\u9769\u6253\u5462\u771f\u5168\u624d\u56db\u5df2\u6240\u654c\u4e4b\u6700\u5149\u4ea7\u60c5\u8def\u5206\u603b\u6761\u767d\u8bdd\u4e1c\u5e2d\u6b21\u4eb2\u5982\u88ab\u82b1\u53e3\u653e\u513f\u5e38\u6c14\u4e94\u7b2c\u4f7f\u5199\u519b\u5427\u6587\u8fd0\u518d\u679c\u600e\u5b9a\u8bb8\u5feb\u660e\u884c\u56e0\u522b\u98de\u5916\u6811\u7269\u6d3b\u90e8\u95e8\u65e0\u5f80\u8239\u671b\u65b0\u5e26\u961f\u5148\u529b\u5b8c\u5374\u7ad9\u4ee3\u5458\u673a\u66f4\u4e5d\u60a8\u6bcf\u98ce\u7ea7\u8ddf\u7b11\u554a\u5b69\u4e07\u5c11\u76f4\u610f\u591c\u6bd4\u9636\u8fde\u8f66\u91cd\u4fbf\u6597\u9a6c\u54ea\u5316\u592a\u6307\u53d8\u793e\u4f3c\u58eb\u8005\u5e72\u77f3\u6ee1\u65e5\u51b3\u767e\u539f\u62ff\u7fa4\u7a76\u5404\u516d\u672c\u601d\u89e3\u7acb\u6cb3\u6751\u516b\u96be\u65e9\u8bba\u5417\u6839\u5171\u8ba9\u76f8\u7814\u4eca\u5176\u4e66\u5750\u63a5\u5e94\u5173\u4fe1\u89c9\u6b65\u53cd\u5904\u8bb0\u5c06\u5343\u627e\u4e89\u9886\u6216\u5e08\u7ed3\u5757\u8dd1\u8c01\u8349\u8d8a\u5b57\u52a0\u811a\u7d27\u7231\u7b49\u4e60\u9635\u6015\u6708\u9752\u534a\u706b\u6cd5\u9898\u5efa\u8d76\u4f4d\u5531\u6d77\u4e03\u5973\u4efb\u4ef6\u611f\u51c6\u5f20\u56e2\u5c4b\u79bb\u8272\u8138\u7247\u79d1\u5012\u775b\u5229\u4e16\u521a\u4e14\u7531\u9001\u5207\u661f\u5bfc\u665a\u8868\u591f\u6574\u8ba4\u54cd\u96ea\u6d41\u672a\u573a\u8be5\u5e76\u5e95\u6df1\u523b\u5e73\u4f1f\u5fd9\u63d0\u786e\u8fd1\u4eae\u8f7b\u8bb2\u519c\u53e4\u9ed1\u544a\u754c\u62c9\u540d\u5440\u571f\u6e05\u9633\u7167\u529e\u53f2\u6539\u5386\u8f6c\u753b\u9020\u5634\u6b64\u6cbb\u5317\u5fc5\u670d\u96e8\u7a7f\u5185\u8bc6\u9a8c\u4f20\u4e1a\u83dc\u722c\u7761\u5174\u5f62\u91cf\u54b1\u89c2\u82e6\u4f53\u4f17\u901a\u51b2\u5408\u7834\u53cb\u5ea6\u672f\u996d\u516c\u65c1\u623f\u6781\u5357\u67aa\u8bfb\u6c99\u5c81\u7ebf\u91ce\u575a\u7a7a\u6536\u7b97\u81f3\u653f\u57ce\u52b3\u843d\u94b1\u7279\u56f4\u5f1f\u80dc\u6559\u70ed\u5c55\u5305\u6b4c\u7c7b\u6e10\u5f3a\u6570\u4e61\u547c\u6027\u97f3\u7b54\u54e5\u9645\u65e7\u795e\u5ea7\u7ae0\u5e2e\u5566\u53d7\u7cfb\u4ee4\u8df3\u975e\u4f55\u725b\u53d6\u5165\u5cb8\u6562\u6389\u5ffd\u79cd\u88c5\u9876\u6025\u6797\u505c\u606f\u53e5\u533a\u8863\u822c\u62a5\u53f6\u538b\u6162\u53d4\u80cc\u7ec6";
		Random random = new Random();// 生成随机数

		// 定义x坐标
		int x = 10;
		for (int i = 0; i < 4; i++) {
			// 随机颜色
			graphics2d.setColor(new Color(20 + random.nextInt(110), 20 + random
					.nextInt(110), 20 + random.nextInt(110)));
			// 旋转 -30 --- 30度
			int jiaodu = random.nextInt(60) - 30;
			// 换算弧度
			double theta = jiaodu * Math.PI / 180;

			// 生成一个随机数字
			int index = random.nextInt(words.length()); // 生成随机数 0 到 length - 1
			// 获得字母数字
			char c = words.charAt(index);
			//将随机数存入字符串
			sBuffer.append(c);
            
			// 将c 输出到图片
			graphics2d.rotate(theta, x, 20);
			graphics2d.drawString(String.valueOf(c), x, 20);
			graphics2d.rotate(-theta, x, 20);
			x += 30;
		}
		//stringbuffer的内容放在session中
		request.getSession().setAttribute("checkCode", sBuffer.toString());

		// 步骤五 绘制干扰线
		graphics.setColor(getRandColor(160, 200));
		int x1;
		int x2;
		int y1;
		int y2;
		for (int i = 0; i < 30; i++) {
			x1 = random.nextInt(width);
			x2 = random.nextInt(12);
			y1 = random.nextInt(height);
			y2 = random.nextInt(12);
			graphics.drawLine(x1, y1, x1 + x2, x2 + y2);
		}

		// 将上面图片输出到浏览器 ImageIO
		graphics.dispose();// 释放资源
		ImageIO.write(bufferedImage, "jpg", response.getOutputStream());

	}


	public void doPost(HttpServletRequest request, HttpServletResponse response)
			throws ServletException, IOException {
		doGet(request, response);
	}

	/**
	 * 取其某一范围的color
	 * 
	 * @param fc
	 *            int 范围参数1
	 * @param bc
	 *            int 范围参数2
	 * @return Color
	 */
	private Color getRandColor(int fc, int bc) {
		// 取其随机颜色
		Random random = new Random();
		if (fc > 255) {
			fc = 255;
		}
		if (bc > 255) {
			bc = 255;
		}
		int r = fc + random.nextInt(bc - fc);
		int g = fc + random.nextInt(bc - fc);
		int b = fc + random.nextInt(bc - fc);
		return new Color(r, g, b);
	}

}

```

```java
package com.imooc.web.servlet;

import java.io.IOException;
import java.util.List;

import javax.servlet.ServletException;
import javax.servlet.annotation.WebServlet;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;

import com.imooc.domain.User;
import com.imooc.service.UserService;
import com.imooc.service.impl.UserServiceImpl;

@WebServlet("/LoginServlet")
public class LoginServlet extends HttpServlet {
	private static final long serialVersionUID = 1L;

	protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
		//验证码校验
		//获取session里面的随机生成的验证码
		String code1=(String)request.getSession().getAttribute(com.google.code.kaptcha.Constants.KAPTCHA_SESSION_KEY);
		//获取login.jsp输入的验证码
		String code2=request.getParameter("checkCode");
		//校验
		if(code2==null || !code2.equalsIgnoreCase(code1)) {
			request.setAttribute("msg", "验证码错误");
			request.getRequestDispatcher("/login.jsp").forward(request, response);
			return;
		}
		//接收数据
		String username=request.getParameter("username");
		String password=request.getParameter("password");
		//封装数据
		User user=new User();
		user.setUsername(username);
		user.setPassword(password);
		//处理数据：完成登录
		UserService userService=new UserServiceImpl();
		  //获得用户列表集合
		List<User> list=(List<User>)getServletContext().getAttribute("userList");
		User existUser=userService.login(list,user);
		//显示结果
		if(existUser==null) {
			//登录失败
			request.setAttribute("msg", "用户名或者密码错误");
			request.getRequestDispatcher("/login.jsp").forward(request, response);
		}else {
			//登录成功
			//将用户信息保存起来：用session
			request.getSession().setAttribute("existUser", existUser);
			response.sendRedirect(request.getContextPath()+"/header.jsp");
		}
	}

	protected void doPost(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
		// TODO Auto-generated method stub
		doGet(request, response);
	}

}

```

```java
package com.imooc.web.servlet;

import java.io.FileOutputStream;
import java.io.IOException;
import java.io.InputStream;
import java.io.OutputStream;
import java.util.HashMap;
import java.util.List;
import java.util.Map;

import javax.servlet.ServletException;
import javax.servlet.annotation.WebServlet;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;

import org.apache.commons.fileupload.FileItem;
import org.apache.commons.fileupload.FileUploadException;
import org.apache.commons.fileupload.servlet.ServletFileUpload;

import com.imooc.domain.User;
import com.imooc.service.UserService;
import com.imooc.service.impl.UserServiceImpl;
import com.imooc.utils.UploadUtils;

@WebServlet("/RegistServlet")
public class RegistServlet extends HttpServlet {
	private static final long serialVersionUID = 1L;

	protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
		//接收数据
//      String username=request.getParameter("username");
//      String password=request.getParameter("password");
		//文件上传的代码
		 Map<String,String> map=new HashMap<>();
		//1.创建磁盘文件项工厂
		org.apache.commons.fileupload.disk.DiskFileItemFactory diskFileItemFactory = new org.apache.commons.fileupload.disk.DiskFileItemFactory();
	    //2.创建核心解析类
	    ServletFileUpload fileUpload=new ServletFileUpload(diskFileItemFactory);
	    //3.解析请求对象，将请求分成几个部分（FileItem）
	    try {
			List<FileItem> list=fileUpload.parseRequest(request);
		    //4.遍历集合，获得每个部分的对象
			for(FileItem fileItem:list) {
				//判断普通项还是文件上传项
				if(fileItem.isFormField()) {
					//普通项--用户名，密码，确认密码
					//获得普通项的名称
					String name=fileItem.getFieldName();
					//获得普通项的值
					String value=fileItem.getString("UTF-8");
					map.put(name, value);
				}else {
					//文件上传项
					//获得文件名称
					String fileName=fileItem.getName();
					String uuidFileName=UploadUtils.getUuidFileName(fileName);
					//获得文件的输入流
					InputStream is=fileItem.getInputStream();
					//需要将文件写入到服务器的某个路径即可，全局的真实路径
					String path=getServletContext().getRealPath("/upload");
					System.out.println(path);
					//显示图片<img src="regist_login/upload/xxxx"
					//创建输出流和输入流对接
                    //这段代码的意思见笔记
					String url=path+"//"+uuidFileName;
					//getContextPath()的意思是项目的虚拟路径
					map.put("path", request.getContextPath()+"/upload/"+uuidFileName);
					OutputStream os=new FileOutputStream(url);
					int len=0;
					byte[] b=new byte[1024];
					while((len=is.read(b))!=-1) {
						os.write(b,0,len);
					}
					is.close();
					os.close();
				}
			}
			
		} catch (FileUploadException e) {
			// TODO Auto-generated catch block
			e.printStackTrace();
		}
		//封装数据
		User user=new User();
		user.setUsername(map.get("username"));
		user.setPassword(map.get("password"));
        user.setPath(map.get("path"));
        System.out.println(user);
		//处理数据
		//ServletContext在一个web工程中只有一个，所以这里因为在前面设置过了，直接用就可以
		UserService userService=new UserServiceImpl();
		//getServletContext()前面加或者不加request都可以
		List<User> userList=(List<User>)getServletContext().getAttribute("userList");
		userService.regist(userList, user);
		System.out.println(userList);
		//显示处理结果
		//getContextPath得到的是项目的虚拟路径,是为了在任何情况下保证路径的正确性
		response.sendRedirect(request.getContextPath()+"/login.jsp");
	}

	/**
	 * @see HttpServlet#doPost(HttpServletRequest request, HttpServletResponse response)
	 */
	protected void doPost(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
		// TODO Auto-generated method stub
		doGet(request, response);
	}

}

```

```html
<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<title>用户注册</title>
<link href="style/common.css" type="text/css" rel="stylesheet">
<link href="style/add.css" type="text/css" rel="stylesheet">
<link rel="stylesheet" href="style/login.css">
<script type="text/javascript">
	function validate_form() {
		//获得用户名的值
		var username = document.getElementById("username").value;
		if (username == null || username == "") {
			alert("用户名不能为空");
			return false;
		}
		//获得密码的值
		var password = document.getElementById("password").value;
		if (password == null || password == "") {
			alert("密码不能为空");
			return false;
		}

		//获得确认密码的值
		var repassword = document.getElementById("repassword").value;
		if (repassword != password) {
			alert("确认密码和密码不一致");
			return false;
		}
		return true;

	}
</script>
</head>
<body>
	<div class="header">
		<div class="logo f1">
			<img src="image/logo.png">
		</div>
		<div class="auth fr">
			<ul>
				<li><a href="login.html">登录</a></li>
				<li><a href="register.html">注册</a></li>
			</ul>
		</div>
	</div>
	<div class="content">
		<div class="center">
			<div class="center-login">
				<div class="login-banner">
					<a href="#"><img src="image/login_banner.png" alt=""></a>
				</div>
				<div class="user-login">
					<div class="user-box">
						<div class="user-title">
							<p>用户注册</p>
						</div>
						<form id="regForm" enctype="multipart/form-data" class="login-table" onsubmit="validate_form()" action="/regist_login/RegistServlet" method="post">
							<div class="login-left">
								<label class="username">用户名&nbsp&nbsp&nbsp&nbsp</label> <input
									type="text" id="username" class="yhmiput" name="username">
							</div>
							<div class="login-left">
								<label class="username">密码&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp</label>
								<input type="password" id="password" class="yhmiput" name="password">
							</div>
							<div class="login-left">
								<label class="username">确认密码</label> <input type="password"
									id="repassword" class="yhmiput" name="repwd">
							</div>
							<div class="login-left">
								<label class="username">上传头像</label> <input type="file"
									class="yhmiput" name="file">
							</div>
							<!-- <div class="login-left">
            <label class="username">验证码&nbsp&nbsp&nbsp&nbsp</label>
            <input type="text" class="codeiput" name="code">
			<input type="text" class="codeiput" name="code">
          </div> -->
							<div class="login-btn">
								<button>注册</button>
							</div>
						</form>

					</div>
				</div>
			</div>
		</div>
	</div>
	<div class="footer">
		<p>
			<span>M-GALLARY</span> ©2017 POWERED BY IMOOC.INC
		</p>
	</div>

</body>
</html>
```

```html
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>

<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
	<title>用户登录</title>
	
	<link href="style/common.css" type="text/css" rel="stylesheet">
  <link href="style/add.css" type="text/css" rel="stylesheet">
  <link rel="stylesheet" href="style/login.css">
  <script type="text/javascript">
        function change(){
        	var codeImg=document.getElementById("codeImg");
        	codeImg.src="${pageContext.request.contextPath }/KaptchaServlet?time="+new Date().getTime();
        }    
  </script>
</head>
<body>
  <div class="header">
   <div class="logo f1">
    <img src="image/logo.png">
  </div>
  <div class="auth fr">
    <ul>
     <li><a href="login.html">登录</a></li>
     <li><a href="register.html">注册</a></li>
   </ul>
 </div>
</div>
<div class="content">
  <div class="center">
    <div class="center-login">
      <div class="login-banner">
       <a href="#"><img src="image/login_banner.png" alt=""></a>
     </div>
     <div class="user-login">
       <div class="user-box">
         <div class="user-title">
           <p>用户登录</p>
           <p>${ msg }</p>
         </div>
         <form  action="/regist_login/LoginServlet" class="login-table"  method="post">
          <div class="login-left">
            <label class="username">用户名</label>
            <input type="text" class="yhmiput" name="username">
          </div>
          <div class="login-left">
            <label class="username">密码&nbsp&nbsp&nbsp</label>
            <input type="password" class="yhmiput" name="password">
          </div>
		  <div class="login-left">
            <label class="username">验证码</label>
            <input type="text" class="codeiput" name="checkCode">
			<img id="codeImg" onclick="change()" src="${pageContext.request.contextPath }/KaptchaServlet" name="code">
          </div>
       
        <div class="login-btn"><button>登录</button></div>
         </form>
      </div>
    </div>
  </div>
</div>
</div>
<div class="footer">
 <p><span>M-GALLARY</span> ©2017 POWERED BY IMOOC.INC</p>
</div>

</body>
</html>
```

```html
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
<%@ taglib uri="http://java.sun.com/jsp/jstl/core" prefix="c"%>
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>Insert title here</title>
</head>
<body>
   <c:if test="${empty existUser}">
     <ul>
         <li><a href="${pageContext.request.contextPath}/login.jsp">登录</a></li>
         <li><a href="${pageContext.request.contextPath}/register.html">注册</a></li>
     </ul>
    </c:if>
    <c:if test="${not empty existUser}">
         <img src="${existUser.path}">
    </c:if>
</body>
</html>
```

