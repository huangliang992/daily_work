## Spring简介
### 为什么要用Spring
1.不用自己去管理众多的对象，方便开发；    
2.框架对对象的管理已经优化的很好了，有可能会有性能上的优势；    
3.方便维护，为后期功能的增加或是更改都做好了松耦合的基础，这样是很好的。  
4.架构师在项目初期认为项目以后会变得复杂时肯定会使用依赖注入的方式【解耦】，提高项目可维护性，防止以后陷入到处new的困境

之前写的时候，就不知道new了多少。依赖注入方式和反转控制确实很有用。

### 怎么用spring，spring入门

添加beans.xml;

	<?xml version="1.0" encoding="UTF-8"?>
	<beans xmlns="http://www.springframework.org/schema/beans"
    xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
    xsi:schemaLocation="http://www.springframework.org/schema/beans
    http://www.springframework.org/schema/beans/spring-beans-3.0.xsd">

  	 <bean id="helloWorld" class="com.hainan.cs.bean.HelloBean">
       <property name="a" value="Hello World!"/>
   	</bean>
	</beans>

pom.xml中添加依赖

	<!-- Spring 核心依赖文件 -->
		<!-- http://mvnrepository.com/artifact/org.springframework/spring-core -->
		<dependency>
			<groupId>org.springframework</groupId>
			<artifactId>spring-core</artifactId>
			<version>4.2.6.RELEASE</version>
		</dependency>

		<!-- http://mvnrepository.com/artifact/org.springframework/spring-context -->
		<dependency>
			<groupId>org.springframework</groupId>
			<artifactId>spring-context</artifactId>
			<version>4.2.6.RELEASE</version>
		</dependency>

例子：HelloBean.java

	package com.hainan.cs.bean;

	public class HelloBean {
	private  String a;

	public String getA() {
		return a;
	}

	public void setA(String a) {
		this.a = a;
	}
	
	}

SayHello.java

	package com.hainan.cs.springtest;

	import com.hainan.cs.bean.HelloBean;
	import org.springframework.context.ApplicationContext;
	import org.springframework.context.support.ClassPathXmlApplicationContext;
	public class SayHello {
	//传统的写法
	private HelloBean hb;
	public void traditionnal(){
		hb=new HelloBean();
		hb.setA("Hello World!");
		System.out.println(hb.getA());
	}
	//Spring 写法
	public void nowwrite(){
		ApplicationContext context = 
	             new ClassPathXmlApplicationContext("beans.xml");
		hb=(HelloBean) context.getBean("helloWorld");
		System.out.println("new"+hb.getA());
	}
	public static void main(String [] args){
		SayHello s=new SayHello();
		s.traditionnal();
		s.nowwrite();
	}
	}

输出结果

![](http://i.imgur.com/RegNct5.png)


下面是实现反转控制，依赖注入部分要引入的jar    
![](http://i.imgur.com/kbkstjI.png)