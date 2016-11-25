##Spring MVC + Spring + Hibernate + mysql 注册登陆入门实例
###（1） 结构
（说明目的是要做在线聊天室的，也包含登陆注册部分，先用这部分做个例子）开发环境用的是STS（spring tool suite）基于Eclipse开发的spring整合开发环境。    
主要的几个文件说明：userbean.java 用户的bean文件，userbean.hbm.xml是他的hibernate映射文件。application-config.xml文件是spring的配置文件，里面包含数据源配置（连数据库），bean设置等。mvc-config.xml文件是springmvc 的配置文件。

![](http://i.imgur.com/CuYvuAI.jpg)

### （2）代码

1. userbean.java
	
		public class UserBean {
			private String id;
			private String username;
			private String password;
			private String phone;
			private String email;
			private String address;
			public String getId() {
				return id;
			}
			public void setId(String id) {
				this.id = id;
			}
			public String getUsername() {
				return username;
			}
			public void setUsername(String username) {
				this.username = username;
			}
			public String getPassword() {
				return password;
			}
			public void setPassword(String password) {
				this.password = password;
			}
			public String getPhone() {
				return phone;
			}
			public void setPhone(String phone) {
				this.phone = phone;
			}
			public String getEmail() {
				return email;
			}
			public void setEmail(String email) {
				this.email = email;
			}
			public String getAddress() {
				return address;
			}
			public void setAddress(String address) {
				this.address = address;
			}
			
		}

2. UserDao.java

		public interface UserDao {
			public UserBean getUser(String id);
			public void insert(UserBean user);
			void delete(String id);
			List<UserBean> queryUser(String username);
			void updateAddress(String userid, String address);
			void updatePhone(String userid, String phone);
			void updateEmail(String userid, String email);
			void updatePassword(String userid, String password);
			void updateName(String userid, String username);
		}

3. UserDaoImp.java

		public class UserDaoImp implements UserDao{
			
			private SessionFactory sessionFactory;
		
			public void setSessionFactory(SessionFactory sessionFactory) {
				this.sessionFactory = sessionFactory;
			}
			
			@Override
			public UserBean getUser(String id){
				Session session=sessionFactory.openSession();
				session.beginTransaction();
				UserBean user=(UserBean) session.get(UserBean.class, id);
				session.close();
				return user;
			}
			@Override
			public void insert(UserBean user){
				Session session=sessionFactory.openSession();
				session.beginTransaction();
				session.save(user);
				session.getTransaction().commit();
				session.close();
			}
			@Override
			public void delete(String id){
				Session session=sessionFactory.openSession();
				session.beginTransaction();
				UserBean user=(UserBean) session.get(UserBean.class, id);
				session.delete(user);
				session.getTransaction().commit();
				session.close();
			}
			@Override
			public List<UserBean> queryUser(String username){
				Session session=sessionFactory.openSession();
				session.beginTransaction();
				String str="from UserBean user where username='"+username+"'";
				Query query=session.createQuery(str);
				List<UserBean> userlist=query.list();
				session.close();
				return userlist;
			}
			@Override
			public void updateName(String userid,String username){
				Session session=sessionFactory.openSession();
				session.beginTransaction();
				UserBean user=(UserBean) session.get(UserBean.class, userid);
				user.setUsername(username);
				session.update(user);
				session.getTransaction().commit();
				session.close();
			}
			@Override
			public void updatePassword(String userid,String password){
				Session session=sessionFactory.openSession();
				session.beginTransaction();
				UserBean user=(UserBean) session.get(UserBean.class, userid);
				user.setPassword(password);;
				session.update(user);
				session.getTransaction().commit();
				session.close();
			}
			@Override
			public void updateEmail(String userid,String email){
				Session session=sessionFactory.openSession();
				session.beginTransaction();
				UserBean user=(UserBean) session.get(UserBean.class, userid);
				user.setEmail(email);;
				session.update(user);
				session.getTransaction().commit();
				session.close();
			}
			@Override
			public void updatePhone(String userid,String phone){
				Session session=sessionFactory.openSession();
				session.beginTransaction();
				UserBean user=(UserBean) session.get(UserBean.class, userid);
				user.setPhone(phone);;
				session.update(user);
				session.getTransaction().commit();
				session.close();
			}
			@Override
			public void updateAddress(String userid,String address){
				Session session=sessionFactory.openSession();
				session.beginTransaction();
				UserBean user=(UserBean) session.get(UserBean.class, userid);
				user.setAddress(address);;
				session.update(user);
				session.getTransaction().commit();
				session.close();
			}
			public static void main(String args[]){
				ClassPathXmlApplicationContext context = new ClassPathXmlApplicationContext("spring/application-config.xml");
				UserDaoImp udi=context.getBean(UserDaoImp.class);
				UserBean user=new UserBean();
				user.setUsername("zhangsan");
				user.setPassword("zhangsan");
				udi.insert(user);
				List<UserBean> ulist=udi.queryUser("zhangsan");
				for(int i=0;i<ulist.size();i++){
					System.out.println(ulist.get(i).getId());
				}
			}
			
		}

4. SignupController.java

		@Controller
		@RequestMapping(value="/signup")
		public class SignupController {
			@RequestMapping()
			public ModelAndView signup(){
				ModelAndView mav=new ModelAndView();
				mav.setViewName("signup");
				mav.addObject("tag", 0);
				return mav;
			}
			@RequestMapping(value="/adduser")
			public ModelAndView addUser(String username,String password,String email,String phone,String address){
				ModelAndView mav=new ModelAndView();
				UserBean user=new UserBean();
				user.setEmail(email);
				user.setPassword(password);
				user.setPhone(phone);
				user.setUsername(username);
				user.setAddress(address);
				ClassPathXmlApplicationContext context = new ClassPathXmlApplicationContext("spring/application-config.xml");
				UserDaoImp udi=context.getBean(UserDaoImp.class);
				List<UserBean> userlist=udi.queryUser(username);
				if(userlist.size()==0){
					udi.insert(user);
					mav.setViewName("login");
				}else{
					//用户名已经存在
					mav.setViewName("signup");
					mav.addObject("tag", 1);
				}
				return mav;
			}
		}

5. UserBean.hbm.xml

		<?xml version="1.0"?>
		<!DOCTYPE hibernate-mapping PUBLIC "-//Hibernate/Hibernate Mapping DTD 3.0//EN"
		"http://hibernate.sourceforge.net/hibernate-mapping-3.0.dtd">
		<!-- Generated 2016-11-25 11:28:34 by Hibernate Tools 3.4.0.CR1 -->
		<hibernate-mapping>
		    <class name="com.hainan.cs.bean.UserBean" table="USERBEAN">
		        <id name="id" type="java.lang.String">
		            <column name="ID" />
		            <generator class="uuid" />
		        </id>
		        <property name="username" type="java.lang.String">
		            <column name="USERNAME" />
		        </property>
		        <property name="password" type="java.lang.String">
		            <column name="PASSWORD" />
		        </property>
		        <property name="phone" type="java.lang.String">
		            <column name="PHONE" />
		        </property>
		        <property name="email" type="java.lang.String">
		            <column name="EMAIL" />
		        </property>
		        <property name="address" type="java.lang.String">
		            <column name="ADDRESS" />
		        </property>
		    </class>
		</hibernate-mapping>

6. signup.jsp

		<%@ page language="java" contentType="text/html; charset=ISO-8859-1"
		    pageEncoding="ISO-8859-1"%>
		<!DOCTYPE html PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN" "http://www.w3.org/TR/html4/loose.dtd">
		<html>
		<head>
		<meta http-equiv="Content-Type" content="text/html; charset=ISO-8859-1">
		<link href="${pageContext.request.contextPath}/resources/layoutit/src/css/bootstrap.min.css" rel="stylesheet">
		<link href="${pageContext.request.contextPath}/resources/layoutit/src/css/style.css" rel="stylesheet">
		<title>Sign Up</title>
		</head>
		<style>
		.error{
		color:red;
			}
		</style>
		<body>
		<script type="text/javascript">
		$(document).ready(function(){
			var name_exist_tag=0;
			name_exist_tag=${tag};
			if(name_exist_tag==1){
				alert("user name exist");
				}
		});
		</script>
		<script src="${pageContext.request.contextPath}/resources/layoutit/src/js/jquery.min.js"></script>
		<script src="${pageContext.request.contextPath}/resources/layoutit/src/js/bootstrap.min.js"></script>
		<script src="${pageContext.request.contextPath}/resources/layoutit/src/js/scripts.js"></script>
		<script src="${pageContext.request.contextPath}/resources/jquery_validation/lib/jquery.js"></script>
		<script src="${pageContext.request.contextPath}/resources/jquery_validation/dist/jquery.validate.min.js"></script>
		<div class="container-fluid">
			<div class="row" style="background-color:#39D8D8;height:768px">
				<div class="col-md-12">
					<div class="row">
						<div class="col-md-4">
						</div>
						<div class="col-md-4" style="background-color:#f5f5f5;margin-top:80px">
						<br><br>
							<h2 style="text-align:center">Sign</h2>
							<br><br>
							<form role="form" id="signup" action="${pageContext.request.contextPath}/signup/adduser">
								<div class="form-group">
									 
									<label for="username">
										user name:
									</label>
									<input type="text" class="form-control" id="username" name="username"/>
								</div>
								<div class="form-group">
									 
									<label for="password">
										Password:
									</label>
									<input type="password" class="form-control" id="password" name="password"/>
								</div>
								<div class="form-group">
									 
									<label for="rpassword">
										Password again:
									</label>
									<input type="password" class="form-control" id="rpassword" name="rpassword"/>
								</div>
								<div class="form-group">
									 
									<label for="email">
										email:
									</label>
									<input type="email" class="form-control" id="email" name="email"/>
								</div>
								<div class="form-group">
									 
									<label for="phone">
										phone:
									</label>
									<input type="text" class="form-control" id="phone" name="phone"/>
								</div>
								<div class="form-group">
									 
									<label for="address">
										address:
									</label>
									<input type="text" class="form-control" id="address" name="address"/>
								</div>
								<button type="submit" class="btn btn-default" id="btn1">
									Submit
								</button>
								<br><br>
							</form>
							<script type="text/javascript">
								$("#signup").validate({
									rules:{
											username: "required",
											password: "required",
											rpassword: {
												required: true,
												equalTo: "#password",
												},
											email: "required",
											phone: "required"
											},
											
									errorPlacement: function(error, element) {
									// Append error within linked label
										$( element )
											.closest( "form" )
												.find( "label[for='" + element.attr( "id" ) + "']" )
													.append( error );
										},
									errorElement: "span",
									messages:{
											password:"please input password",
											rpassword:{
												required:"please input password",
												equalTo: "password not same"
												}
										}
								});
							</script>
						
						</div>
						<div class="col-md-4">
						</div>
					</div>
				</div>
			</div>
		</div>
		</body>
		</html>

7. login.jsp

		<%@ page language="java" contentType="text/html; charset=ISO-8859-1"
		    pageEncoding="ISO-8859-1"%>
		<!DOCTYPE html PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN" "http://www.w3.org/TR/html4/loose.dtd">
		<html>
		<head>
		<meta http-equiv="Content-Type" content="text/html; charset=ISO-8859-1">
		<link href="${pageContext.request.contextPath}/resources/layoutit/src/css/bootstrap.min.css" rel="stylesheet">
		<link href="${pageContext.request.contextPath}/resources/layoutit/src/css/style.css" rel="stylesheet">
		<script src="${pageContext.request.contextPath}/resources/layoutit/src/js/jquery.min.js"></script>
		<script src="${pageContext.request.contextPath}/resources/layoutit/src/js/bootstrap.min.js"></script>
		<script src="${pageContext.request.contextPath}/resources/layoutit/src/js/scripts.js"></script>
		<title>login</title>
		</head>
		<body>
		<script type="text/javascript">
		$(document).ready(function(){
			var password_right=0;
			password_right=${tag};
			if(password_right==1){
				alert("password or username is wrong!");
				}
		});
		</script>
		<div class="container-fluid">
			<div class="row" style="background-color:#39D8D8;height:768px">
				<div class="col-md-12">
					<div class="row">
						<div class="col-md-4">
						</div>
						<div class="col-md-4" style="background-color:#f5f5f5;margin-top:180px">
							<h2 style="text-align: center">Log In</h2>
							<br>
							<form role="form" action="${pageContext.request.contextPath }/login/userlogin">
								<div class="form-group">
									 
									<label for="username">
										user name:
									</label>
									<input type="text" class="form-control" id="username" name="username"/>
								</div>
								<div class="form-group">
									 
									<label for="password">
										Password:
									</label>
									<input type="password" class="form-control" id="password" name="password"/>
								</div>
								<div class="form-group">
									<div class="col-md-3">
										<button type="submit" class="btn btn-default">
											Log in
										</button>
									</div>
									<div class="col-md-3">
										<a href="${pageContext.request.contextPath }/signup" class="btn btn-default">Sign up</a>
									</div>
									<div class="col-md-6"></div>
								</div>
								<br><br><br><br>
							</form>
						</div>
						<div class="col-md-4">
						</div>
					</div>
				</div>
			</div>
		</div>
		</body>
		</html>

8. application-confg.xml

		<?xml version="1.0" encoding="UTF-8"?>
		
		<beans xmlns="http://www.springframework.org/schema/beans" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
			xmlns:context="http://www.springframework.org/schema/context"
			xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd
				http://www.springframework.org/schema/context http://www.springframework.org/schema/context/spring-context.xsd">
		    
		    <!-- Uncomment and add your base-package here:
		         <context:component-scan
		            base-package="org.springframework.samples.service"/>  -->
		            
		<!-- 配置数据源 -->
		    <bean id="dataSource" class="org.apache.commons.dbcp.BasicDataSource"
				destroy-method="close">
				<property name="driverClassName" value="com.mysql.jdbc.Driver" />
				<property name="url" value="jdbc:mysql://localhost:3306/online_chart" />
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

9. mvc-config.xml

		<?xml version="1.0" encoding="UTF-8"?>
		
		<beans xmlns="http://www.springframework.org/schema/beans" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
			xmlns:mvc="http://www.springframework.org/schema/mvc" xmlns:context="http://www.springframework.org/schema/context"
			xsi:schemaLocation="http://www.springframework.org/schema/mvc http://www.springframework.org/schema/mvc/spring-mvc.xsd
				http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd
				http://www.springframework.org/schema/context http://www.springframework.org/schema/context/spring-context.xsd">
		
		    <!-- Uncomment and your base-package here:-->
		         <context:component-scan
		            base-package="com.hainan.cs.controller"/>
			<!-- 让springmvc放过存放静态资源的请求 -->
			<mvc:resources mapping="/resources/**" location="/resources/" />
		
		    <mvc:annotation-driven />
		
			<bean class="org.springframework.web.servlet.view.InternalResourceViewResolver">
			        <!-- Example: a logical view name of 'showMessage' is mapped to '/WEB-INF/jsp/showMessage.jsp' -->
			        <property name="prefix" value="/WEB-INF/view/"/>
			        <property name="suffix" value=".jsp"/>
			</bean>
		
		</beans>

10. web.xml

		<?xml version="1.0" encoding="ISO-8859-1"?>
		<web-app xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
		         xmlns="http://java.sun.com/xml/ns/javaee"
		         xsi:schemaLocation="http://java.sun.com/xml/ns/javaee
		http://java.sun.com/xml/ns/javaee/web-app_2_5.xsd"
		         id="WebApp_ID" version="2.5">
		
		    <display-name>online_chart</display-name>
		    
		   <!--
				- Location of the XML file that defines the root application context.
				- Applied by ContextLoaderListener.
			-->
		    <context-param>
		        <param-name>contextConfigLocation</param-name>
		        <param-value>classpath:spring/application-config.xml</param-value>
		    </context-param>
		
		    <listener>
		        <listener-class>org.springframework.web.context.ContextLoaderListener</listener-class>
		    </listener>
		    
		    
		    <!--
				- Servlet that dispatches request to registered handlers (Controller implementations).
			-->
		    <servlet>
		        <servlet-name>dispatcherServlet</servlet-name>
		        <servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class>
		        <init-param>
		            <param-name>contextConfigLocation</param-name>
		            <param-value>/WEB-INF/mvc-config.xml</param-value>
		        </init-param>
		        <load-on-startup>1</load-on-startup>
		    </servlet>
		
		    <servlet-mapping>
		        <servlet-name>dispatcherServlet</servlet-name>
		        <url-pattern>/</url-pattern>
		    </servlet-mapping>
		
		</web-app>

11. pom.xml

		<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
		  <modelVersion>4.0.0</modelVersion>
		  <groupId>org.springframework.samples.service.service</groupId>
		  <artifactId>online_chart</artifactId>
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
				<spring-framework.version>3.2.3.RELEASE</spring-framework.version>
		
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
				<!-- Spring ORM support -->
				<dependency>
					<groupId>org.springframework</groupId>
					<artifactId>spring-orm</artifactId>
					<version>${spring-framework.version}</version>
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
				
			</dependencies>	
		</project>

### （3）效果截图

注册页面

![](http://i.imgur.com/LXEDazn.jpg)

提交后数据库显示

![](http://i.imgur.com/oaboUj0.jpg)

###（4）其他

注册页面还包含邮箱验证等，关于在线聊天室的开始会在这个的基础上后期进行开发。