## 在线聊天室websocket+spring实现
###说明
 web socket支持长连接，全双工通信。用户在登陆后进入聊天室（与服务器建立连接），可以与其他与服务器建立连接的用户通信。

客户端：chat3.jsp

	<%@ page language="java" contentType="text/html; charset=UTF-8"
	    pageEncoding="UTF-8"%>
	<!DOCTYPE html PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN" "http://www.w3.org/TR/html4/loose.dtd">
	<html>
	<head>
	<meta http-equiv="Content-Type" content="text/html; charset=UTF-8">
	<link href="${pageContext.request.contextPath}/resources/layoutit/src/css/bootstrap.min.css" rel="stylesheet">
	<link href="${pageContext.request.contextPath}/resources/layoutit/src/css/style.css" rel="stylesheet">
	<script src="${pageContext.request.contextPath}/resources/layoutit/src/js/jquery.min.js"></script>
	<script src="${pageContext.request.contextPath}/resources/layoutit/src/js/bootstrap.min.js"></script>
	<title>聊天室</title>
	</head>
	<body>
	<script type="text/javascript">
			var websocket;
			if('WebSocket' in window){
				websocket=new WebSocket("ws://localhost:8080/chat_websocket_demo/ws");
				}
			websocket.onopen = function(event) {
				console.log("WebSocket:已连接");
				console.log(event);
			};
			websocket.onmessage = function(event) {
				var data=JSON.parse(event.data);
				console.log("WebSocket:收到一条消息",data);
				$("#content").append("<p style=\"color:green\">"+data.username+"</p><p style=\"color:white\">"+data.msg+"</p>");
			};
			websocket.onerror = function(event) {
				console.log("WebSocket:发生错误 ");
				console.log(event);
			};
			websocket.onclose = function(event) {
				console.log("WebSocket:已关闭");
				console.log(event);
			}
			function send(){
				var message=$("#message").val();
				var data={};
				data["username"]="${username}";
				data["msg"]=message;
				$("#content").append("<p style=\"color:red\">"+'${username}'+"</p><p style=\"color:white\">"+message+"</p>");
				websocket.send(JSON.stringify(data));
				}
	</script>
	<div class="container-fluid">
		<div class="row">
			<div class="col-md-3"></div>
			<div class="col-md-6">
				<div class="row">
					<div class="col-md-12">
						<h2 style="text-align:center">${username},聊天室欢迎你</h2>
						<!-- 这里显示聊天室的内容 -->
						<div class="row" id="content" style="height:600px; background-color:gray"></div>
					</div>
				</div>
				<div class="row">
					<div class="col-md-12">
							<input type="text" name="message" class="form-control" id="message"/>
							<button type="button" class="btn btn-primary" onclick="send()">提交</button>
					</div>
				</div>
			</div>
			<div class="col-md-3"></div>
		</div>
	</div>
	</body>
	</html>

服务器端

ChatConfig.java

	import javax.annotation.Resource;
	
	import org.springframework.stereotype.Component;
	import org.springframework.web.servlet.config.annotation.WebMvcConfigurerAdapter;
	import org.springframework.web.socket.config.annotation.EnableWebSocket;
	import org.springframework.web.socket.config.annotation.WebSocketConfigurer;
	import org.springframework.web.socket.config.annotation.WebSocketHandlerRegistry;
	
	@Component
	@EnableWebSocket
	public class ChatConfig extends WebMvcConfigurerAdapter implements WebSocketConfigurer{
		@Resource
		private ChatHandler handler;
		@Override
		public void registerWebSocketHandlers(WebSocketHandlerRegistry registry) {
			// TODO Auto-generated method stub
			registry.addHandler(handler, "/ws").addInterceptors(new ChatHandShake());
		}
		
	}

ChatHandler.java

	import java.util.HashMap;
	import java.util.Map;
	
	import org.springframework.stereotype.Component;
	import org.springframework.web.socket.CloseStatus;
	import org.springframework.web.socket.WebSocketHandler;
	import org.springframework.web.socket.WebSocketMessage;
	import org.springframework.web.socket.WebSocketSession;
	
	@Component
	public class ChatHandler implements WebSocketHandler{
		
		private  Map<String, WebSocketSession> usermap=new HashMap<String, WebSocketSession>();
		
		//连接关闭后的操作
		@Override
		public void afterConnectionClosed(WebSocketSession wsession, CloseStatus status) throws Exception {
			System.out.println("用户"+wsession.getAttributes().get("username")+"已经关闭");
			for(Map.Entry<String, WebSocketSession>entry:usermap.entrySet()){
				String key=entry.getKey();
				WebSocketSession value=entry.getValue();
				if(key.equals(wsession.getAttributes().get("username"))){
					usermap.remove(key);
					System.out.println("用户"+key+"已经移除");
				}
			}
		}
	
		@Override
		public void afterConnectionEstablished(WebSocketSession wsession) throws Exception {
			String uname=(String) wsession.getAttributes().get("username");
			System.out.println("用户"+uname+"已经加入");
			if(usermap.get(uname)==null){
				usermap.put(uname, wsession);
			}
		}
		
		// message 是前台传过来的json数据，包括用户名，和发送的消息，要将消息转发给所有的其他用户
		@Override
		public void handleMessage(WebSocketSession wsession, WebSocketMessage<?> message) throws Exception {
			//TextMessage tm=new TextMessage(message.toString());
			System.out.println("开始发送消息"+usermap.size());
			for(Map.Entry<String,WebSocketSession>entry:usermap.entrySet()){
				String key=entry.getKey();
				WebSocketSession value=entry.getValue();
				if(key!=wsession.getAttributes().get("username")){
					if(value!=null&&value.isOpen()){
						value.sendMessage(message);
					}
				}
			}
		}
	
		@Override
		public void handleTransportError(WebSocketSession wsession, Throwable th) throws Exception {
			if(wsession.isOpen()){
				wsession.close();
			}
			for(Map.Entry<String, WebSocketSession>entry:usermap.entrySet()){
				String key=entry.getKey();
				WebSocketSession value=entry.getValue();
				if(key.equals(wsession.getAttributes().get("username"))){
					usermap.remove(key);
					System.out.println("传输出错！用户"+key+"已经移除");
				}
			}
		}
	
		@Override
		public boolean supportsPartialMessages() {
			return false;
		}
	
	}

ChatHandShake.java

	import java.util.Map;
	
	import javax.servlet.http.HttpSession;
	
	import org.springframework.http.server.ServerHttpRequest;
	import org.springframework.http.server.ServerHttpResponse;
	import org.springframework.http.server.ServletServerHttpRequest;
	import org.springframework.web.socket.WebSocketHandler;
	import org.springframework.web.socket.server.HandshakeInterceptor;
	
	public class ChatHandShake implements HandshakeInterceptor{
	
		@Override
		public void afterHandshake(ServerHttpRequest arg0, ServerHttpResponse arg1, WebSocketHandler arg2, Exception arg3) {
			// TODO Auto-generated method stub
			
		}
	
		@Override
		public boolean beforeHandshake(ServerHttpRequest request, ServerHttpResponse response, WebSocketHandler handlar,
				Map<String, Object> map) throws Exception {
			// TODO Auto-generated method stub
			System.out.println("用户"+((ServletServerHttpRequest) request).
					getServletRequest().getSession(false).getAttribute("username")+"已经建立连接");
			if(request instanceof ServletServerHttpRequest){
				ServletServerHttpRequest srequest=(ServletServerHttpRequest)request;
				HttpSession session=srequest.getServletRequest().getSession(false);
				if(session.getAttribute("username")!=null){
					map.put("username", session.getAttribute("username"));
				}else {
					return false;
					}
			}
			return true;
		}
	
	}

###配置环境

（1）spring 4.0以上
（2）web 3.0以上

maven pom.xml

	<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
	  <modelVersion>4.0.0</modelVersion>
	  <groupId>org.springframework.samples.service.service</groupId>
	  <artifactId>chat_websocket_demo</artifactId>
	  <version>0.0.1-SNAPSHOT</version>
	  <packaging>war</packaging>
	  
	    <properties>
	
			<!-- Generic properties -->
			<java.version>1.6</java.version>
			<project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
			<project.reporting.outputEncoding>UTF-8</project.reporting.outputEncoding>
			
			<!-- Web -->
			<jsp.version>2.2</jsp.version>
			<jstl.version>1.2</jstl.version>
			<servlet.version>2.5</servlet.version>
			
	
			<!-- Spring -->
			<spring-framework.version>4.0.6.RELEASE</spring-framework.version>
	
			<!-- Hibernate / JPA -->
			<hibernate.version>4.2.1.Final</hibernate.version>
	
			<!-- Logging -->
			<logback.version>1.0.13</logback.version>
			<slf4j.version>1.7.5</slf4j.version>
	
			<!-- Test -->
			<junit.version>4.11</junit.version>
	
		</properties>
		
		<dependencies>
		
			<!-- Spring MVC -->
			<dependency>
				<groupId>org.springframework</groupId>
				<artifactId>spring-webmvc</artifactId>
				<version>${spring-framework.version}</version>
			</dependency>
			<dependency>
				<groupId>org.springframework</groupId>
				<artifactId>spring-messaging</artifactId>
				<version>${spring-framework.version}</version>
			</dependency>
			
			<!-- web socket -->
			<dependency>
				<groupId>org.springframework</groupId>
				<artifactId>spring-websocket</artifactId>
				<version>${spring-framework.version}</version>
			</dependency>
			
			<!-- Spring ORM support -->
			<dependency>
				<groupId>org.springframework</groupId>
				<artifactId>spring-orm</artifactId>
				<version>${spring-framework.version}</version>
			</dependency>
			
			<!-- Other Web dependencies -->
			<dependency>
				<groupId>javax.servlet</groupId>
				<artifactId>jstl</artifactId>
				<version>${jstl.version}</version>
			</dependency>
			<dependency>
				<groupId>javax.servlet</groupId>
				<artifactId>servlet-api</artifactId>
				<version>${servlet.version}</version>
				<scope>provided</scope>
			</dependency>
			<dependency>
				<groupId>javax.servlet.jsp</groupId>
				<artifactId>jsp-api</artifactId>
				<version>${jsp.version}</version>
				<scope>provided</scope>
			</dependency>
		
			<!-- Spring and Transactions -->
			<dependency>
				<groupId>org.springframework</groupId>
				<artifactId>spring-tx</artifactId>
				<version>${spring-framework.version}</version>
			</dependency>
	
			<!-- Logging with SLF4J & LogBack -->
			<dependency>
				<groupId>org.slf4j</groupId>
				<artifactId>slf4j-api</artifactId>
				<version>${slf4j.version}</version>
				<scope>compile</scope>
			</dependency>
			<dependency>
				<groupId>ch.qos.logback</groupId>
				<artifactId>logback-classic</artifactId>
				<version>${logback.version}</version>
				<scope>runtime</scope>
			</dependency>
	
			<!-- Hibernate -->
			<dependency>
				<groupId>org.hibernate</groupId>
				<artifactId>hibernate-entitymanager</artifactId>
				<version>${hibernate.version}</version>
			</dependency>
	
			
			<!-- Test Artifacts -->
			<dependency>
				<groupId>org.springframework</groupId>
				<artifactId>spring-test</artifactId>
				<version>${spring-framework.version}</version>
				<scope>test</scope>
			</dependency>
			<dependency>
				<groupId>junit</groupId>
				<artifactId>junit</artifactId>
				<version>${junit.version}</version>
				<scope>test</scope>
			</dependency>
			<!-- mysql -->
			<dependency>
				<groupId>mysql</groupId>
				<artifactId>mysql-connector-java</artifactId>
				<version>5.1.9</version>
			</dependency>
			<dependency>
				<groupId>commons-dbcp</groupId>
				<artifactId>commons-dbcp</artifactId>
				<version>1.4</version>
			</dependency>
			
			<!-- jackson spring通过response向前台发送数据是转成json格式要用到-->
	        <dependency>  
	            <groupId>org.codehaus.jackson</groupId>  
	            <artifactId>jackson-mapper-asl</artifactId>  
	            <version>1.9.13</version>  
	        </dependency>  
	        <dependency>  
	            <groupId>org.codehaus.jackson</groupId>  
	            <artifactId>jackson-core-asl</artifactId>  
	            <version>1.9.13</version>  
	        </dependency> 
	        <dependency>  
	            <groupId>com.fasterxml.jackson.core</groupId>  
	            <artifactId>jackson-databind</artifactId>  
	            <version>2.5.0</version>  
	        </dependency> 
	     	<!-- http://mvnrepository.com/artifact/org.json/json -->
			<dependency>
				<groupId>org.json</groupId>
				<artifactId>json</artifactId>
				<version>20160212</version>
			</dependency>
			
		</dependencies>	
	<build>  
	    <plugins>  
	        <plugin>  
	            <groupId>org.apache.maven.plugins</groupId>  
	            <artifactId>maven-compiler-plugin</artifactId>  
	            <version>2.0.2</version>  
	            <configuration>  
	                <source>1.7</source>  
	                <target>1.7</target> 
	                <encoding>utf8</encoding> 
	            </configuration>  
	        </plugin>  
	    </plugins>  
	</build>  
	</project>

数据源配置：spring-application.xml

	<?xml version="1.0" encoding="UTF-8"?>
	
	<beans xmlns="http://www.springframework.org/schema/beans" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
		xmlns:context="http://www.springframework.org/schema/context"
		xsi:schemaLocation="http://www.springframework.org/schema/beans   
	        http://www.springframework.org/schema/beans/spring-beans-4.0.xsd   
	        http://www.springframework.org/schema/context   
	        http://www.springframework.org/schema/context/spring-context-4.0.xsd   
	        http://www.springframework.org/schema/mvc   
	        http://www.springframework.org/schema/mvc/spring-mvc-4.0.xsd  ">
	    
	    <!-- Uncomment and add your base-package here:
	         <context:component-scan
	            base-package="org.springframework.samples.service"/>  -->
	            
	<!-- 配置数据源 -->
	    <bean id="dataSource" class="org.apache.commons.dbcp.BasicDataSource"
			destroy-method="close">
			<property name="driverClassName" value="com.mysql.jdbc.Driver" />
			<property name="url" value="jdbc:mysql://localhost:3306/online_chart?useUnicode=true&amp;characterEncoding=UTF-8" />
			<property name="username" value="root" />
			<property name="password" value="root" />
		</bean>
	
		<!-- 配置session factory -->
		<!-- Hibernate 4 SessionFactory Bean definition -->
		<bean id="hibernate4SessionFactory"
			class="org.springframework.orm.hibernate4.LocalSessionFactoryBean">
			<property name="dataSource" ref="dataSource" />
			<property name="hibernateProperties">
				<props>
					<prop key="hibernate.dialect">org.hibernate.dialect.MySQLDialect</prop>
					<prop key="hibernate.hbm2ddl.auto">update</prop>
					<prop key="hibernate.show_sql">true</prop>
					<prop key="hibernate.format_sql">true</prop>
				</props>
			</property>
			<property name="mappingLocations">
				<list>
					<value>classpath:/com/hainan/cs/hbm/*.hbm.xml</value>
				</list>
			</property>
		</bean>
		<bean id="userdao" class="com.hainan.cs.dao.UserDaoImp">
			<property name="sessionFactory" ref="hibernate4SessionFactory"></property>
		</bean>
	</beans>

mvc-config.xml

	<?xml version="1.0" encoding="UTF-8"?>
	
	<beans xmlns="http://www.springframework.org/schema/beans" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
		xmlns:mvc="http://www.springframework.org/schema/mvc" xmlns:context="http://www.springframework.org/schema/context"
		xsi:schemaLocation="http://www.springframework.org/schema/beans   
	        http://www.springframework.org/schema/beans/spring-beans-4.0.xsd   
	        http://www.springframework.org/schema/context   
	        http://www.springframework.org/schema/context/spring-context-4.0.xsd   
	        http://www.springframework.org/schema/mvc   
	        http://www.springframework.org/schema/mvc/spring-mvc-4.0.xsd  ">
	
	    <!-- Uncomment and your base-package here:-->
	         <context:component-scan
	            base-package="com.hainan.cs"/>
		
		<!-- 让springmvc放过存放静态资源的请求 -->
		<mvc:resources mapping="/resources/**" location="/resources/" />
	
	    <mvc:annotation-driven />
	
		<bean class="org.springframework.web.servlet.view.InternalResourceViewResolver">
		        <!-- Example: a logical view name of 'showMessage' is mapped to '/WEB-INF/jsp/showMessage.jsp' -->
		        <property name="prefix" value="/WEB-INF/view/"/>
		        <property name="suffix" value=".jsp"/>
		</bean>
	
	</beans>
###截图
登陆

![](http://i.imgur.com/DsB1xXO.jpg)

聊天

![](http://i.imgur.com/CQKf18H.jpg)

![](http://i.imgur.com/N1dhR99.jpg)

###其他的文件可以在demo中找到
地址：https://github.com/huangliang992/chat_websocket_demo