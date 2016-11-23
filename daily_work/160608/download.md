## 实现下下载
1. 后台读取问价家中的所有的文件
2. 将文件名存到一个数组中，文件名+路径为文件地址
3. 在页面上显示文件名
4. 点击下载按钮，实现文件下载

## 第一次开tomcat时，提示找不到struts.xml

1. 先开了tomcat，再运行？而非直接运行时加载tomcat？


## struts2 result 标签中的type类型，以及节点中的param

chain    
    
     用来处理Action链，被跳转的action中仍能获取上个页面的值，如request信息。    
    
     com.opensymphony.xwork2.ActionChainResult    
    
 dispatcher    struts的result默认类型
    
     用来转向页面，通常处理JSP    
    
     org.apache.struts2.dispatcher.ServletDispatcherResult    
    
 freemaker    
    
     处理FreeMarker模板    
    
     org.apache.struts2.views.freemarker.FreemarkerResult    
    
 httpheader    
    
     控制特殊HTTP行为的结果类型    
    
     org.apache.struts2.dispatcher.HttpHeaderResult    
   

stream    
    
     向浏览器发送InputSream对象，通常用来处理文件下载，还可用于返回AJAX数据    
    
     org.apache.struts2.dispatcher.StreamResult    
    
 velocity    
    
     处理Velocity模板    
    
     org.apache.struts2.dispatcher.VelocityResult    
    
 xslt    
    
     处理XML/XLST模板    
    
     org.apache.struts2.views.xslt.XSLTResult    
    
 plainText    
    
     显示原始文件内容，例如文件源代码    
    
     org.apache.struts2.dispatcher.PlainTextResult    
    
   
 plaintext    
    
     显示原始文件内容，例如文件源代码    
    
     org.apache.struts2.dispatcher.PlainTextResult  

redirect    
    
     重定向到一个URL ，被跳转的页面中丢失传递的信息，如request   
    
     org.apache.struts2.dispatcher.ServletRedirectResult    
    
 redirectAction    
    
     重定向到一个Action ，跳转的页面中丢失传递的信息，如request      
    
     org.apache.struts2.dispatcher.ServletActionRedirectResult    
    
 redirect-action   
    
     重定向到一个Action ，跳转的页面中丢失传递的信息，如request      
    
     org.apache.struts2.dispatcher.ServletActionRedirectResult 

   
 注：redirect与redirect-action区别

一、使用redirect需要后缀名 使用redirect-action不需要后缀名 
 二、type="redirect"　的值可以转到其它命名空间下的action,而redirect-action只能转到同一命名空下的 action，因此它可以省略.action的后缀直接写action的名称。


param标签主要用于为其他标签提供参数，例如bean和include标签,起到注入的作用    
param参数设置：    
name:可选属性，指定设置参数名称    
value：可选属性，指定参数的值    
id：可选属性，指定该元素引用id    


 	<result name="success"type="stream">  

   	<param name="contentType">image/jpeg</param> 

	//contentType 是stream对应的类中自带的，下面的几个都是自带的属性

   	<param name="inputName">imageStream</param> 

   	<param name="contentDisposition">attachment;filename="document.pdf"</param>  

  	<param name="bufferSize">1024</param> 
 	</result>

## 在超链接中设置点击链接时跳转的action
	<a href="<s:url action="Login_input"/>">Sign On</a>

	<a href="  
	<s:url action="">  
   	<s:param name=" " value=""></s:param>   
   	<s:param name=" " value=""></s:param>   
   	<s:param name=" " value=""></s:param>   
	</s:url>"  
	>测试连接</a>

## 简易版的download实现

download.jsp

	<body>
	download<a href="<s:url action="Download"></s:url>">test</a>
	</body>

struts.xml

	<action name="Download" class="com.hainu.cs.action.DownloadAction">
	<result name="SUCCESS" type="stream">
		<param name="contentType">text/plain</param>
		<param name="contentDisposition">attachment;fileName="${fileName}"</param>
		<param name="inputName">fileDownload</param>
		<param name="bufferSize">1024</param>
	</result>
	</action>

DownloadAction.java

	package com.hainu.cs.action;

	import java.io.InputStream;

	import org.apache.struts2.ServletActionContext;

	import com.opensymphony.xwork2.ActionSupport;

	public class DownloadAction extends ActionSupport{
		

		private String fileName;
		private InputStream fileDownload;

		
		public InputStream getFileDownload() {
			this.fileName = "example.uml" ;
			System.out.println(this.fileName);
			   //获取资源路径
			   return ServletActionContext.getServletContext().getResourceAsStream("/WEB-INF/upload/example.uml") ;
		}

		public void setFileDownload(InputStream fileDownload) {
			this.fileDownload = fileDownload;
		}

		public String getFileName() {
			return fileName;
		}

		public void setFileName(String fileName) {
			this.fileName = fileName;
		}
		
		@Override
		public String execute() throws Exception {
			
			return "SUCCESS";
		}

	}

在action类中直接生成好文件列表
	
	这样可以向action传值，但不知为何用<a href="action"><s:url><s:param name="" value=""></s:param></s:url></a>却接受不到值
	download<a href="Download.action?fileName=example.uml">test</a>