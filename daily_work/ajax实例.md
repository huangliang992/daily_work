##Ajax实战
###（1）为什么用Ajax
ajax可以实现异步交互，不需要刷新整个页面，做到局部刷新
###（2）用Ajax验证用户名是否冲突的例子(spring 整合 ajax)
注册页面    

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
	<script src="${pageContext.request.contextPath}/resources/layoutit/src/js/scripts.js"></script>
	<script src="${pageContext.request.contextPath}/resources/jquery_validation/lib/jquery.js"></script>
	<script src="${pageContext.request.contextPath}/resources/jquery_validation/dist/jquery.validate.min.js"></script>
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
	function check(){
		$.ajax({
			data:"username="+$("#username").val(),
			dataType:'json',
			type:"GET",
			url:"${pageContext.request.contextPath}/signup/check",
			success:function(data){
				$("#userlabel").html("user name:<span style=\"color:red\">"+data.msg+"</span>");
				},
			error:function(data){
				alert("error");
				}
			});
	}
	</script>
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
								 
								<label for="username" id="userlabel">
									user name:
								</label>
								<input type="text" class="form-control" id="username" name="username" onBlur="check();"/>
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

登陆的controller

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
				mav.setViewName("signup");
			}else{
				//用户名已经存在
				mav.setViewName("signup");
				mav.addObject("tag", 1);
			}
			return mav;
		}
		@RequestMapping(value="/check")
		public @ResponseBody Map<String,Object> checkUserName(String username){
			System.out.println("用户名是"+username);
			Map<String,Object> map=new HashMap<String,Object>();
			ClassPathXmlApplicationContext context=new ClassPathXmlApplicationContext("spring/application-config.xml");
			UserDaoImp udi=context.getBean(UserDaoImp.class);
			List<UserBean> userlist=udi.queryUser(username);
			if(userlist.size()!=0){
				System.out.println("用户名存在");
				map.put("msg", "用户名已经存在");
			}else {
				System.out.println("用户名不存在");
				map.put("msg", "用户名可以使用");
			}
			return map;
		}
	}

用到的UserBean和UserDao

UserBean.java

	public class UserBean {
		//test
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

UserDaoImp.java
	
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


pom必要的插件，spring用 ResponseBody返回时会自动将Map数据转化成json格式前端才能接收，要依赖以下的几个jackson插件，不加上会报错
	
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

其他的向数据库配置，sping环境配置略过

###（三）效果截图
当焦点发生变化时出发ajax的事件，向后台请求用户数据判断

![](http://i.imgur.com/XmBDBcL.jpg)
