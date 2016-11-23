
1. jsp页面中设置全局变量（文件的路径名称）    
不叫全局变量，是session
2. 全局变量如何赋值
3. 在表单传值时，如何将全局变量也交给action类进行处理


## 结构

![](http://i.imgur.com/qFORlV5.png)

## 以寻找两个类之间的所有路径为例

FindPathAction.java

	package com.hainu.cs.action;

	import java.util.ArrayList;
	import java.util.Map;

	import com.hainu.cs.circulationchecking.controler.FindPathsOfTwoNode;
	import com.hainu.cs.circulationchecking.entity.PathOfG;
	import com.opensymphony.xwork2.ActionContext;
	import com.opensymphony.xwork2.ActionSupport;

	public class FindPathAction extends ActionSupport{
	private String begin;
	private String end;
	private String result="";
	
	public String getResult() {
		return result;
	}
	public void setResult(String result) {
		this.result = result;
	}
	public String getBegin() {
		return begin;
	}
	public void setBegin(String begin) {
		this.begin = begin;
	}
	public String getEnd() {
		return end;
	}
	public void setEnd(String end) {
		this.end = end;
	}

	public String execute(){
		ActionContext actionContext = ActionContext.getContext();   
        Map session = actionContext.getSession();   
        Object filepath=session.get("filepath"); 
        String fp=filepath.toString();
        //System.out.println(fp);
        //System.out.println(begin+"  "+end);
        
        FindPathsOfTwoNode ft=new FindPathsOfTwoNode();
        ft.setBegin(begin);
        ft.setEnd(end);
        ft.findPathsOfG(fp);
        
        ArrayList<PathOfG> t=ft.getPaths();
        for(int k=0;k<t.size();k++){
		PathOfG m=t.get(k);
		ArrayList<String> node=m.getNode();
		ArrayList<String> edge=m.getEdge();
		//System.out.println(node.size());
		result=result+"{path:";
		for(int i=0;i<node.size();i++){
			if(i<node.size()-1){
			result=result+node.get(i)+"--"+edge.get(i)+"--";
			//System.out.print(node.get(i)+" ");
			}else result=result+node.get(i);
		}
		result=result+"}";
        }
        return "SUCCESS";
	}
	}

structs.xml

	<action name="FindPath" class="com.hainu.cs.action.FindPathAction">
	<result name="SUCCESS">default.jsp</result>
	</action>

default.jsp

	<%@ page language="java" contentType="text/html; charset=ISO-8859-1"
    pageEncoding="ISO-8859-1"%>
        <%@ taglib prefix="s" uri="/struts-tags"%>
	<!DOCTYPE html PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN" "http://www.w3.org/TR/html4/loose.dtd">
	<html>
	<head>
	<meta http-equiv="Content-Type" content="text/html; charset=ISO-8859-1">
	<title>Insert title here</title>
	</head>
	<body>
	<s:include value="upload.jsp"></s:include>
	<s:include value="uploadsuccess.jsp"></s:include>
    <s:include value="findpath.jsp"></s:include>
    <s:include value="findcirculation.jsp"></s:include>
	</body>
	</html>

findpath.jsp

	<%@ page language="java" contentType="text/html; charset=ISO-8859-1"
    pageEncoding="ISO-8859-1"%>
    <%@ taglib prefix="s" uri="/struts-tags"%>
	<!DOCTYPE html PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN" "http://www.w3.org/TR/html4/loose.dtd">
	<html>
	<head>
	<meta http-equiv="Content-Type" content="text/html; charset=ISO-8859-1">
	<title>Insert title here</title>
	</head>
	<body>
	<h2>FIND THE PATHS BETWEEN TWO CLASSES IN THE CLASS DIAGRAM</h2>    
	<s:form action="FindPath">
		<h3><s:textfield name="begin" label="Input the begin class in the class diagram Class 1" /></h3>
		<h3><s:textfield name="end" label="Input the end class in the class diagram Class 2" /></h3>
    	<s:submit value="submit"/>
    </s:form>  
	The paths between the begin class and end class are:<s:property value="result"></s:property>
	</body>
	</html>


结果初始界面

![](http://i.imgur.com/cR6D2Nh.png)

上传test1.uml后测试结果
![](http://i.imgur.com/Zl0vf05.png)


findcirculations 和其他的处理也是类似的