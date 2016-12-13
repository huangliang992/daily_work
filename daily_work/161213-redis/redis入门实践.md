#redis入门教程
##1.ubuntu 采用docker安装redis
两条指令完成：`docker pull redis:3.2`; 然后 `docker run -p 6379:6379 --name myredis -d redis:3.2 redis-server --appendonly yes`.
当然也可以使用Eclipse的docker tool 和启动管理redis运行容器。当然采用其他方式安装的一样有效。
##2.redis的可视化编辑器redis desktop manager
连接到之前之前创建和运行的redis后界面
![](http://i.imgur.com/SgezzQM.jpg)
##3.java使用redis
首先导入redis的包，采用maven的话可以直接在pom.xml里面加入下面的依赖。

	<!-- https://mvnrepository.com/artifact/redis.clients/jedis -->
	<dependency>
	    <groupId>redis.clients</groupId>
	    <artifactId>jedis</artifactId>
	    <version>2.9.0</version>
	</dependency>

（1）连接示例：

	package redistest;
	
	import redis.clients.jedis.Jedis;
	
	public class ConnectionTest {
		public static void main(String[] args) {
		      //连接本地的 Redis 服务
		      Jedis jedis = new Jedis("localhost");
		      System.out.println("Connection to server sucessfully");
		      //查看服务是否运行
		      System.out.println("Server is running: "+jedis.ping());
		 }
	}

![](http://i.imgur.com/09O9HmN.jpg)

（2）字符串示例：往redis中加一个键值对
	
	import redis.clients.jedis.Jedis;
	
	public class StringTest {
		public static void main(String[] args) {
		      //连接本地的 Redis 服务
		      Jedis jedis = new Jedis("localhost");
		      System.out.println("Connection to server sucessfully");
		      //设置 redis 字符串数据
		      jedis.set("runoobkey", "Redis tutorial");
		     // 获取存储的数据并输出
		     System.out.println("Stored string in redis:: "+ jedis.get("runoobkey"));
		 }
	}

![](http://i.imgur.com/aK1EMQN.jpg)

再用redis desktop manager看一下，可以看到新增加了一个runoobkey键

![](http://i.imgur.com/THtO1iA.jpg)

（3）list示例

	package redistest;
	
	import java.util.List;
	
	import redis.clients.jedis.Jedis;
	
	public class ListTest {
		public static void main(String[] args) {
		      //连接本地的 Redis 服务
		      Jedis jedis = new Jedis("localhost");
		      System.out.println("Connection to server sucessfully");
		      //存储数据到列表中
		      jedis.lpush("tutorial-list", "Redis");
		      jedis.lpush("tutorial-list", "Mongodb");
		      jedis.lpush("tutorial-list", "Mysql");
		     // 获取存储的数据并输出
		     List<String> list = jedis.lrange("tutorial-list", 0 ,5);
		     for(int i=0; i<list.size(); i++) {
		       System.out.println("Stored string in redis:: "+list.get(i));
		     }
		     
		 }
	}

![](http://i.imgur.com/g68n2TY.jpg)

![](http://i.imgur.com/7MI7Xzj.jpg)

##4.Redis的数据类型
redis的数据类型有，string，set，hash，list，zset

（1）String    

![](http://i.imgur.com/88mZJph.jpg)

（2）set 是无序集合，集合里面元素是不重复的

![](http://i.imgur.com/aaNYFEu.jpg)

（3）list 按插入顺序排列

![](http://i.imgur.com/bvZ2OZ3.jpg)

（4）hash 哈希表

![](http://i.imgur.com/EkXR4Qc.jpg)

##5.Hibernate整合redis