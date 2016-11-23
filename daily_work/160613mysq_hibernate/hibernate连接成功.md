## hibernate连接mysql

使用的url，用户名密码


		<property name="hibernate.connection.url">jdbc:mysql://localhost:3306/uml</property>
    	<property name="hibernate.connection.driver_class">com.mysql.jdbc.Driver</property>
    	<property name="hibernate.connection.username">root</property>
    	<property name="hibernate.connection.password">root</property>

测试代码

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

输出结果，可以看到确实正确获取了数据库中的用户名

![](http://i.imgur.com/pk2WWCp.png)

![](http://i.imgur.com/MOE2xdT.png)

注意事项：`<property name="username"/>`中如果不写columname，那么bean中的属性和数据库中的列名要互相匹配。