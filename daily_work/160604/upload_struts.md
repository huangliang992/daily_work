## struts 2 实现文件上传
## 1.结构

![](http://i.imgur.com/4CQd3eq.png)

## 2. FileUploadAction.java

	package com.hainu.action;

	import java.io.File;
	import java.io.FileInputStream;
	import java.io.FileOutputStream;


	import org.apache.struts2.ServletActionContext;

	import com.opensymphony.xwork2.ActionSupport;

	public class FileUploadAction extends ActionSupport{
	private File upload;
	private String uploadFileName;//XXXFileName is used to get the name of the file
	private String uploadContentType;// XXXContentType is used to get the type of the file
	//private String name;
	

	public String execute() throws Exception{
		String path = ServletActionContext.getServletContext().getRealPath("/WEB-INF/upload");  
        String filename = path+File.separator+uploadFileName; 
        System.out.println(uploadFileName);
        System.out.println(filename);
        FileInputStream in = new FileInputStream(upload);  
        FileOutputStream out = new FileOutputStream(filename);  
        byte[]b = new byte[1024];  
        int len = 0;  
        while((len=in.read(b))>0){  
            out.write(b,0,len);  
        }  
        out.close();  
        return "SUCCESS";  
	}


	public File getUpload() {
		return upload;
	}


	public void setUpload(File upload) {
		this.upload = upload;
	}


	public String getUploadFileName() {
		return uploadFileName;
	}


	public void setUploadFileName(String uploadFileName) {
		this.uploadFileName = uploadFileName;
	}


	public String getUploadContentType() {
		return uploadContentType;
	}


	public void setUploadContentType(String uploadContentType) {
		this.uploadContentType = uploadContentType;
	}

	
	
	}

## 3. struts.xml

	<package name="file" namespace="/fileoperate" extends="struts-default">
	<action name="FileUp">
	<result>upload.jsp</result>
	</action>
	
	<action name="Upload" class="com.hainu.action.FileUploadAction">
	<result name="SUCCESS">uploadsuccess.jsp</result>
	</action>
	</package>

## 4. upload.jsp

	<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
     <%@ taglib prefix="s" uri="/struts-tags"%>
	<!DOCTYPE html PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN" "http://www.w3.org/TR/html4/loose.dtd">
	<html>
	<head>
	<title>Upload File</title>
	</head>
	<body>
	<s:form action="Upload" method="post" enctype="multipart/form-data">
		<!-- 此处必须为multipart/form-data，而且必须使用post方法 -->  
    	<s:file name="upload" label="UploadFile"/>
    	<s:submit value="submit"/>
    </s:form>  
	</body>
	</html>

## 5. uploadsuccess.jsp

	<%@ page language="java" contentType="text/html; charset=ISO-8859-1"
    pageEncoding="ISO-8859-1"%>
	<!DOCTYPE html PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN" "http://www.w3.org/TR/html4/loose.dtd">
	<html>
	<head>
	<meta http-equiv="Content-Type" content="text/html; charset=ISO-8859-1">
	<title>Insert title here</title>
	</head>
	<body>
	success
	</body>
	</html>

## 6. 结果

![](http://i.imgur.com/v5l0rkD.png)

![](http://i.imgur.com/Wxo56lT.png)

![](http://i.imgur.com/fVwQK8N.png)