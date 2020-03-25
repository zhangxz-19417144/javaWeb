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
    //level表示第几级，parent表示第一级的code
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
   //code表示课程码，name表示课程名称
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

