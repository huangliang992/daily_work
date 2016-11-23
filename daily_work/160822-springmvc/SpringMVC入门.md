## spring MVC 入门

### 1. pom.xml

	<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
	<modelVersion>4.0.0</modelVersion>
	<groupId>CrunchifySpringMVCTutorial</groupId>
	<artifactId>CrunchifySpringMVCTutorial</artifactId>
	<version>0.0.1-SNAPSHOT</version>
	<build>
		<sourceDirectory>src</sourceDirectory>
		<plugins>
			<plugin>
				<artifactId>maven-compiler-plugin</artifactId>
				<version>3.1</version>
				<configuration>
					<source>1.8</source>
					<target>1.8</target>
				</configuration>
			</plugin>
 
		</plugins>
	</build>
	<dependencies>
		<dependency>
			<groupId>org.springframework</groupId>
			<artifactId>spring-context</artifactId>
			<version>4.3.1.RELEASE</version>
		</dependency>
		<dependency>
			<groupId>org.springframework</groupId>
			<artifactId>spring-aop</artifactId>
			<version>4.3.1.RELEASE</version>
		</dependency>
		<dependency>
			<groupId>org.springframework</groupId>
			<artifactId>spring-webmvc</artifactId>
			<version>4.3.1.RELEASE</version>
		</dependency>
		<dependency>
			<groupId>org.springframework</groupId>
			<artifactId>spring-web</artifactId>
			<version>4.3.1.RELEASE</version>
		</dependency>
 
		<dependency>
			<groupId>javax.servlet</groupId>
			<artifactId>jstl</artifactId>
			<version>1.2</version>
		</dependency>
	</dependencies>
	</project>

### 2. XXXX-servlet.xml

	<beans xmlns="http://www.springframework.org/schema/beans"
	xmlns:mvc="http://www.springframework.org/schema/mvc" xmlns:context="http://www.springframework.org/schema/context"
	xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	xsi:schemaLocation="
        http://www.springframework.org/schema/beans     
        http://www.springframework.org/schema/beans/spring-beans.xsd
        http://www.springframework.org/schema/mvc 
        http://www.springframework.org/schema/mvc/spring-mvc.xsd
        http://www.springframework.org/schema/context 
        http://www.springframework.org/schema/context/spring-context.xsd">
 
	<!--使得Spring可以浏览该包下的所有组件内容-->
	<context:component-scan base-package="com.crunchify.controller" />
 
	<bean id="viewResolver"
		class="org.springframework.web.servlet.view.UrlBasedViewResolver">
		<property name="viewClass"
			value="org.springframework.web.servlet.view.JstlView" />
		<property name="prefix" value="/WEB-INF/jsp/" />
		<property name="suffix" value=".jsp" />
	</bean>
	</beans>

### 3. web.xml

	<?xml version="1.0" encoding="UTF-8"?>
	<web-app xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns="http://java.sun.com/xml/ns/javaee" xsi:schemaLocation="http://java.sun.com/xml/ns/javaee http://java.sun.com/xml/ns/javaee/web-app_3_0.xsd" version="3.0">
  	<display-name>CrunchifySpringMVCTutorial</display-name>
 	 <welcome-file-list>
    <welcome-file>index.jsp</welcome-file>
 	 </welcome-file-list>
  
    <servlet>
        <servlet-name>XXXX</servlet-name>
        <servlet-class>
            org.springframework.web.servlet.DispatcherServlet
        </servlet-class>
        <load-on-startup>1</load-on-startup>
    </servlet>
    <servlet-mapping>
        <servlet-name>XXXX</servlet-name>
        <url-pattern>/welcome.jsp</url-pattern>
        <url-pattern>/welcome.html</url-pattern>
        <url-pattern>*.html</url-pattern>
    </servlet-mapping>
  
	</web-app>

DispatcherServlet 将在web-inf目录下查找XXXX-servlet.xml文件 =[servletpname]-servlet.xml

### 4. CrunchifyHelloWorld.java 

	package com.crunchify.controller;
	 
	import org.springframework.stereotype.Controller;
	import org.springframework.web.bind.annotation.RequestMapping;
	import org.springframework.web.servlet.ModelAndView;
	 
	/*
	 * author: Crunchify.com
	 * 
	 */
	 
	@Controller
	public class CrunchifyHelloWorld {
	 
		@RequestMapping("/welcome")
		public ModelAndView helloWorld() {
	 
			String message = "<br><div style='text-align:center;'>"
					+ "<h3>********** Hello World, Spring MVC Tutorial</h3>This message is coming from CrunchifyHelloWorld.java **********</div><br><br>";
			return new ModelAndView("welcome", "message", message);
		}
	}

@controller 表示这是controller bean 在处理的过程中，@RequestMapping表示这个controller bean 处理这个开头的url请求，例如@RequestMapping("/welcome")处理/welcome或者/welcome.html 等。 通常返回的是java bean可以被视图显示。

### 5. index.jsp
	
	<html>
	<head>
	<title>Spring MVC Tutorial Series by Crunchify.com</title>
	<style type="text/css">
	body {
		background-image: url('http://crunchify.com/bg.png');
	}
	</style>
	</head>
	<body>
		<br>
		<div style="text-align:center">
			<h2>
				Hey You..!! This is your 1st Spring MCV Tutorial..<br> <br>
			</h2>
			<h3>
				<a href="welcome.html">Click here to See Welcome Message... </a>(to
				check Spring MVC Controller... @RequestMapping("/welcome"))
			</h3>
		</div>
	</body>
	</html>

### 6. welcome.html

	<html>
	<head>
	<title>Spring MVC Tutorial by Crunchify - Hello World Spring MVC
		Example</title>
	<style type="text/css">
	body {
		background-image: url('http://crunchify.com/bg.png');
	}
	</style>
	</head>
	<body>${message}
	 
		<br>
		<br>
		<div style="font-family: verdana; padding: 10px; border-radius: 10px; font-size: 12px; text-align:center;">
	 
			Spring MCV Tutorial by <a href="http://crunchify.com">Crunchify</a>.
			Click <a
				href="http://crunchify.com/category/java-web-development-tutorial/"
				target="_blank">here</a> for all Java and <a
				href='http://crunchify.com/category/spring-mvc/' target='_blank'>here</a>
			for all Spring MVC, Web Development examples.<br>
		</div>
	</body>
	</html>