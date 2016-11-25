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
