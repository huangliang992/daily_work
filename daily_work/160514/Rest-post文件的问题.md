# Rest 提交文件
## 1. 前提是依赖和web.xml中jersey的servlet都配置好了
## 2. maven依赖库的冲突

	<dependency>
			<groupId>com.sun.jersey</groupId>
			<artifactId>jersey-servlet</artifactId>
			<version>1.19</version>
		</dependency>
		<dependency>
			<groupId>com.sun.jersey</groupId>
			<artifactId>jersey-client</artifactId>
			<version>1.19</version>
		</dependency>
		<dependency>
			<groupId>com.sun.jersey.contribs</groupId>
			<artifactId>jersey-multipart</artifactId>
			<version>1.7</version>
		</dependency>
加上上述依赖的话会报错，要去掉。
## 3. 上传和下载的代码的一个例子

	import java.io.File;
	import java.io.FileOutputStream;
	import java.io.IOException;
	import java.io.InputStream;
	import java.io.OutputStream;
	import javax.activation.MimetypesFileTypeMap;
	import javax.ws.rs.Consumes;
	import javax.ws.rs.GET;
	import javax.ws.rs.POST;
	import javax.ws.rs.Path;
	import javax.ws.rs.Produces;
	import javax.ws.rs.core.MediaType;
	import javax.ws.rs.core.Response;
	import javax.ws.rs.Consumes;
	import javax.ws.rs.core.MediaType;
	import com.sun.jersey.core.header.FormDataContentDisposition;
	import com.sun.jersey.multipart.FormDataParam;
	@Path("/files")
	public class FileResource {
	private static final String filepath = "E:/circulation-checking-rest/src/resources/download/test1.uml";
	private static final String serverLocation = "E:/circulation-checking-rest/src/resources/upload/";
	@GET
	@Path("download")
	@Consumes(MediaType.APPLICATION_JSON)
	@Produces(MediaType.APPLICATION_OCTET_STREAM)
	public Response downloadFile() {

	    File file = new File(filepath);
	    if (file.isFile() && file.exists()) {
	        String mt = new MimetypesFileTypeMap().getContentType(file);
	        String fileName = file.getName();

	        return Response
	                .ok(file, mt)
	                .header("Content-disposition",
	                        "attachment;filename=" + fileName)
	                .header("ragma", "No-cache")
	                .header("Cache-Control", "no-cache").build();

	    } else {
	        return Response.status(Response.Status.NOT_FOUND)
	                .entity("下载失败，未找到该文件").build();
	    }
	}
	
	
	 @POST
	 @Path("upload")
	 @Consumes(MediaType.MULTIPART_FORM_DATA)
	 @Produces("application/json")
	 public Response uploadFile(
	         @FormDataParam("file") InputStream fileInputStream,
	         @FormDataParam("file") FormDataContentDisposition contentDispositionHeader) 
	             throws IOException {

	     String fileName = contentDispositionHeader.getFileName();
	     String t=contentDispositionHeader.getName();

	     System.out.println(fileName+" "+t);

	     File file = new File(serverLocation + "a.uml"); 
	     File parent = file.getParentFile(); 
	     //判断目录是否存在，不在创建 
	     if(parent!=null&&!parent.exists()){ 
	         parent.mkdirs(); 
	     } 
	     file.createNewFile(); 

	     OutputStream outpuStream = new FileOutputStream(file);
	     int read = 0;
	     byte[] bytes = new byte[1024];

	     while ((read = fileInputStream.read(bytes)) != -1) {
	         outpuStream.write(bytes, 0, read);
	     }

	     outpuStream.flush();
	     outpuStream.close();

	     fileInputStream.close();

	     return Response.status(Response.Status.OK)
	             .entity("Upload Success!").build();
	 }
	}

## 4. 上传文件和下载文件的JSP页面

	<%@ page language="java" contentType="text/html; charset=ISO-8859-1"
    pageEncoding="ISO-8859-1"%>
	<!DOCTYPE html PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN" "http://www.w3.org/TR/html4/loose.dtd">
	<html>
	<head>
	<meta http-equiv="Content-Type" content="text/html; charset=ISO-8859-1">
	<title>Insert title here</title>
	</head>
	<body>
	<p><a href="http://localhost:8080/circulation-checking-rest/test/files/download">Download</a>
	<h3>Upload a File</h3>
	<form action="http://localhost:8080/circulation-checking-rest/test/files/upload" method="post" enctype="multipart/form-data">
    <p>Select a file : <input type="file" name="file" /></p>
    <input type="submit" value="Upload It" style="color: Fuchsia; "/>
	</form>
	</body>
	</html>

## 5. 效果

*上传和下载页面*


![](http://i.imgur.com/jZlt0Rf.png)


*点击下载*


![](http://i.imgur.com/Zf3dszB.png) 


上传文件，问价名解析错误

		@FormDataParam("file") InputStream fileInputStream,
	    @FormDataParam("file") FormDataContentDisposition contentDispositionHeader
	     
		 String fileName = contentDispositionHeader.getFileName();
	     String t=contentDispositionHeader.getName();
	     System.out.println(fileName+" "+t);

FormDataContentDisposition 这个类在解析文件的地址时，出问题了，路径中的斜杠没了。

![](http://i.imgur.com/TPdyo06.png)