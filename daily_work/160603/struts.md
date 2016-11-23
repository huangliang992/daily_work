# struts 实现文件上传和下载
## 1. Struts配置
个人理解：Struts是一种拦截机制，页面提交的的action不直接指向一个页面而是一个行为，然后由struts拦截，做相应的处理。
## 2. struts机制

## 3. 注意
struts.xml中没有相应的action不能添加

## 4. 例子
4.1 结构

![](http://i.imgur.com/apvpolk.png)

struts.xml配置

	<?xml version="1.0" encoding="UTF-8" ?>

	<!DOCTYPE struts PUBLIC
	"-//Apache Software Foundation//DTD Struts Configuration 2.0//EN"
	"http://struts.apache.org/dtds/struts-2.0.dtd">

	<struts>

	<package name="user" namespace="/" extends="struts-default">
		<action name="Login">
			<result>login.jsp</result>
		</action>
		<action name="Welcome" class="com.hainu.action.UserAction">
			<result name="SUCCESS">welcom.jsp</result>
		</action>


	</package>

	<package name="p1" namespace="/user" extends="struts-default">
		<action name="Detail">
			<result>detail.jsp</result>
		</action>
		<action name="CKDetail" class="com.hainu.action.UserDetailAction"
			method="execute">
			<result name="SUCCESS">detail.jsp</result>
		</action>
	</package>

	</struts>

web.xml配置

	<!DOCTYPE web-app PUBLIC
 		"-//Sun Microsystems, Inc.//DTD Web Application 2.3//EN"
 		"http://java.sun.com/dtd/web-app_2_3.dtd" >
	<web-app>
    <display-name>Archetype Created Web Application</display-name>
    <filter>
        <filter-name>struts2</filter-name>
        <filter-class>
            org.apache.struts2.dispatcher.ng.filter.StrutsPrepareAndExecuteFilter
        </filter-class>
    </filter>
    <filter-mapping>
        <filter-name>struts2</filter-name>
        <url-pattern>/*</url-pattern>
    </filter-mapping>
    
    <welcome-file-list>  
    <welcome-file>index.jsp</welcome-file>  
  	</welcome-file-list>  
    
	</web-app>

pom.xml配置，添加struts依赖

	<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://	www.w3.org/2001/XMLSchema-instance"
 	 xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/	maven-v4_0_0.xsd">
  	<modelVersion>4.0.0</modelVersion>
  	<groupId>hainu.cs</groupId>
 	 <artifactId>Struts2Example</artifactId>
  	<packaging>war</packaging>
 	 <version>0.0.1-SNAPSHOT</version>
  	<name>Struts2Example Maven Webapp</name>
  	<url>http://maven.apache.org</url>
 	 <dependencies>
    <dependency>
      <groupId>junit</groupId>
      <artifactId>junit</artifactId>
      <version>3.8.1</version>
      <scope>test</scope>
    </dependency>
    
            <dependency>
            <groupId>org.apache.struts</groupId>
            <artifactId>struts2-core</artifactId>
            <version>2.3.16</version>
        </dependency>
    
  	</dependencies>
  	<build>
    <finalName>Struts2Example</finalName>
  	</build>
	</project>

第一个例子

index.jsp

	<%@ page contentType="text/html; charset=UTF-8"%>
	<%@ taglib prefix="s" uri="/struts-tags"%>
	<html>
	<head></head>
	<body>
    <h1>Struts 2 Hello World Example</h1>
 
    <s:form action="Welcome">
        <s:textfield name="username" label="Username" />
        <s:password name="password" label="Password" />
        <s:submit />
    </s:form>
 
	</body>
	</html>

welcome.jsp

	<%@ page contentType="text/html; charset=UTF-8"%>
	<%@ taglib prefix="s" uri="/struts-tags"%>
	<html>
	<head></head>
	<body>
    <h1>Struts 2 Hello World Example</h1>
 
    <h4>
        Hello
        <s:property value="username" />
    </h4>
 
	</body>
	</html>

UserAction.java

	package com.hainu.action;

	public class UserAction {
	private String username;

	public String getUsername() {
		return username;
	}

	public void setUsername(String username) {
		this.username = username;
	}
	
	public  String execute(){
		System.out.println("welcom using struts!");
		return "SUCCESS";
	}

	}

![](http://i.imgur.com/xoBta7L.png)

![](http://i.imgur.com/Cap5lk0.png)

第二个例子

detail.jsp，s：include 是该页面包含某个页面，user对应UserDetailAction中的user对象。

	<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
    <%@ taglib prefix="s" uri="/struts-tags"%>
	<!DOCTYPE html PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN" "http://www.w3.org/TR/html4/loose.dtd">
	<html>
	<head>
	<meta http-equiv="Content-Type" content="text/html; charset=ISO-8859-1">
	<title>User Detail</title>
	</head>
	<body>
	<s:form action="CKDetail">
        <s:textfield key="user.username" label="Username" />
        <s:textfield key="user.tel" label="Telephone" />
        <s:textfield key="user.adress" label="Adress"/>
        <s:textfield key="user.school" label="School"/>
        <s:textfield key="user.age" label="Age"/>
        <s:submit />
    </s:form>
    <s:include value="ckdetail.jsp"/>
	</body>
	</html>

ckdetail.jsp

	<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
    <%@ taglib prefix="s" uri="/struts-tags"%>
	<!DOCTYPE html PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN" "http://www.w3.org/TR/html4/loose.dtd">
	<html>
	<head>
	<meta http-equiv="Content-Type" content="text/html; charset=ISO-8859-1">
	<title>Insert title here</title>
	</head>
	<body>
	<h1>Struts 2 User Detail Example</h1>
 
    <h4>
        Hello
    </h4>
        <h3>
        姓名：<s:property value="user.username" /></br>
       年龄： <s:property value="user.age" /></br>
        地址：<s:property value="user.adress" /></br>
        学校：<s:property value="user.school" /></br>
    </h3>
	</body>
	</html>


UserDetail.java，这是javabean

	package com.hainu.bean;

	public class UserDetail {
	private String username;
	private String tel;
	private String adress;
	private String school;
	private String age;
	public String getUsername() {
		return username;
	}
	public void setUsername(String username) {
		this.username = username;
	}
	public String getTel() {
		return tel;
	}
	public void setTel(String tel) {
		this.tel = tel;
	}
	public String getAdress() {
		return adress;
	}
	public void setAdress(String adress) {
		this.adress = adress;
	}
	public String getSchool() {
		return school;
	}
	public void setSchool(String school) {
		this.school = school;
	}
	public String getAge() {
		return age;
	}
	public void setAge(String age) {
		this.age = age;
	}
	}

UserDetailAction.java 是控制类，它里面有bean的对象

	package com.hainu.action;

	import com.hainu.bean.UserDetail;

	public class UserDetailAction {
	private UserDetail user;

	public UserDetail getUser() {
		return user;
	}

	public void setUser(UserDetail user) {
		this.user = user;
	}
	public String execute(){
		return "SUCCESS";
	}
	}

struts.xml 对应的部分

	<package name="p1" namespace="/user" extends="struts-default">
		<action name="Detail">
			<result>detail.jsp</result>
		</action>
		<action name="CKDetail" class="com.hainu.action.UserDetailAction"
			method="execute">
			<result name="SUCCESS">detail.jsp</result>
		</action>
	</package>

![](http://i.imgur.com/xRHO8aO.png)

submit 后结果

![](http://i.imgur.com/uQkey43.png)