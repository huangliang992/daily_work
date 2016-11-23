## hibernate如何实现数据库的一些操作

## 对表的操作
包括新建表，删除表，查询表中的数据，修改表中的数据，往表中添加数据，如果表中又增加了一个字段怎么实现。

### 对表中数据的操作：

		//创建session
		SessionFactory sf=new Configuration().configure("hibernate.cfg.xml").buildSessionFactory();
	    Session session = sf.openSession();
		//创建transaction并开始
		session.beginTransaction();
		//执行操作，包括添加，修改，删除，查询
		//1.添加
		User user=new User();
		user.setId(3);
		user.setName("test");
		user.setPassword("111");
		session.save(user);
		//提交事务.把内存的改变提交到数据库上.
        session.getTransaction().commit();
		//2.修改
		User user1=new User();
		user.setId(3);
		user.setName("newtest");
		session.update(user);
		//提交事务.把内存的改变提交到数据库上.
        session.getTransaction().commit();
		//3.删除
		User user2=new User();
		user.setId(3);
		session.delete(user);
		//提交事务.把内存的改变提交到数据库上.
        session.getTransaction().commit();
		//4.查询
		Query query=session.createQuery("from User");
		List userlist=query.list();
		for(int i=0;i<userlist.size();i++){
		User t=(user) userlist.get(i);
		}

### 对表本身的操作，比如根据映射文件生成一张新的表，删除一张表。

在hibernate配置文件中添加，将会在更新和查询时如果找不到表会自动创建表，但一定要注意映射文件要写对，包括对主键的设置，如果不正确将不能穿件表。

	<!-- 根据映射文件自动生成数据库中的表 -->
    <property name="hibernate.hbm2ddl.auto">update</property> 
	<mapping resource="com/hainan/cs/hbm/User.hbm.xml"/>
     <mapping resource="com/hainan/cs/hbm/NewUser.hbm.xml"/>

update：表示自动根据model对象来更新表结构，启动hibernate时会自动检查数据库，如果缺少表，则自动建表；如果表里缺少列，则自动添加列。 

还有其他的参数： 
create：启动hibernate时，自动删除原来的表，新建所有的表，所以每次启动后的以前数据都会丢失。 

create-drop：启动hibernate时，自动创建表，程序关闭时，自动把相应的表都删除。所以程序结束时，表和数据也不会再存在。 

PS：数据库要预先建立好，因为hibernate只会建表，不会建库

### 出现Hibernate: insert into NewUser (name, password, email, age, birthday, userid) values (?, ?, ?, ?, ?, ?)是因为没有commit，`session.getTransaction().commit();`数据库表中也不会有相应的数据

![](http://i.imgur.com/Db4KIAQ.png)

现在可以看到数据库中有显示了

之一主键`<generator class="uuid">`值的是由hibernate自动生成主键，所以看到的userid是那样的