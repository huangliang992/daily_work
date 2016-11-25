##Spring MVC + Spring + Hibernate + mysql 注册登陆入门实例
### 结构（说明目的是要做在线聊天室的，也包含登陆注册部分，先用这部分做个例子）开发环境用的是STS（spring tool suite）基于Eclipse开发的spring整合开发环境
主要的几个文件说明：userbean.java 用户的bean文件，userbean.hbm.xml是他的hibernate映射文件。application-config.xml文件是spring的配置文件，里面包含数据源配置（连数据库），bean设置等。mvc-config.xml文件是springmvc 的配置文件。

![](http://i.imgur.com/CuYvuAI.jpg)

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

