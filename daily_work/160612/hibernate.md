## hibernate连接mysql数据库
### hibernate.cfg.xml配置
	<?xml version="1.0" encoding="utf-8"?>
	<!DOCTYPE hibernate-configuration
  	PUBLIC "-//Hibernate/Hibernate Configuration DTD//EN"
  	"http://hibernate.sourceforge.net/hibernate-configuration-3.0.dtd">
 
	<hibernate-configuration>
  		<session-factory >
 
    <!-- local connection properties -->
    <property name="hibernate.connection.url">jdbc:mysql://localhost:3306/uml</property>
    <property name="hibernate.connection.driver_class">com.mysql.jdbc.Driver</property>
    <property name="hibernate.connection.username">uml</property>
    <property name="hibernate.connection.password">user</property>
    <!-- property name="hibernate.connection.pool_size"></property -->
 
    <!-- dialect for MySQL -->
    <property name="dialect">org.hibernate.dialect.MySQLDialect</property>
 
    <property name="hibernate.show_sql">true</property>
    <property name="hibernate.transaction.factory_class">org.hibernate.transaction.JDBCTransactionFactory</property>
     <mapping resource="com/hainan/cs/hbm/User.hbm.xml"/>
 		</session-factory>
	</hibernate-configuration>

`<property name="hibernate.connection.url">`是连接到的数据库的名uml，`<property name="hibernate.connection.driver_class">`是mysql数据库驱动，`<property name="hibernate.connection.username"> <property name="hibernate.connection.password">`是数据库的用户名和密码，`<property name="dialect">`是方言，` <mapping resource="com/hainan/cs/hbm/User.hbm.xml"/>`是映射文件的路径

### User.hbm.xml配置
	<?xml version="1.0" ?>  
	<!DOCTYPE hibernate-mapping PUBLIC "-//Hibernate/Hibernate Mapping DTD 3.0//EN"  
        "http://www.hibernate.org/dtd/hibernate-mapping-3.0.dtd">  
	<hibernate-mapping package="com.hainan.cs.bean">  
      
    <class name="User">  
        <id name="id">  
            <generator class="native"/>  
        </id>  
    <property name="Username"/>  
    <property name="password"/> 
    <property name="email"/> 
    <property name="tel"/>
    <property name="adress"/>
    <property name="work"/>
    </class>  
      
	</hibernate-mapping>  

`<hibernate-mapping package="com.hainan.cs.bean"> `是指映射到哪个javabean


### User.java
	package com.hainan.cs.bean;

	public class User {
	private String id;
	private String username;
	private String password;
	private String email;
	private String adress;
	private String tel;
	private String work;
	
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
	public String getEmail() {
		return email;
	}
	public void setEmail(String email) {
		this.email = email;
	}
	public String getAdress() {
		return adress;
	}
	public void setAdress(String adress) {
		this.adress = adress;
	}
	public String getTel() {
		return tel;
	}
	public void setTel(String tel) {
		this.tel = tel;
	}
	public String getWork() {
		return work;
	}
	public void setWork(String work) {
		this.work = work;
	}
	
	}

### SessionUtil.java

	package com.hainan.cs.dao;

	import org.hibernate.Session;
	import org.hibernate.SessionFactory;
	import org.hibernate.cfg.Configuration;

	public class SessionUtil {
	  public static Session getSession()
	  {
	    SessionFactory sf = new Configuration().configure("hibernate.cfg.xml").buildSessionFactory();
	        Session session = sf.openSession();
	        return session;
	  }
	 
	}

### UserDao.java

	package com.hainan.cs.dao;

	import java.util.List;
	import org.apache.log4j.Logger;
	import org.hibernate.Session;
	import com.hainan.cs.bean.User;

	public class UserDao {
	private static final Logger logger = Logger.getLogger(UserDao.class); 
	  public static void main(String[] args) {
	 
	     Session session = SessionUtil.getSession();
	  org.hibernate.Transaction tx =  session.beginTransaction();
	 
	  String hql="from User user";
	  List list= session.createQuery(hql).list();
	  for(int i=0;i<list.size();i++)
	  {
	        	 User obj=(User)list.get(i);  
	        	 System.out.println(obj.getUsername());
	  }
	 
	  logger.debug("mes!");
	 
	  }
	}

### 数据库表user

![](http://i.imgur.com/xs3g2fu.png)


### 报错
	Exception in thread "main" org.hibernate.exception.GenericJDBCException: Cannot open connection

	Caused by: java.sql.SQLException: Access denied for user 'uml'@'localhost' (using password: YES)

出现password yes说明进入了数据库用户验证这部分，并且密码也是正确的说明hibernate.cfg.xml密码之类的配置没问题，但为何会报错。

