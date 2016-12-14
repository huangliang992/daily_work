#spring  + redis整合实例
##实例目录
redisrest目录下是直接连redis的例子并不通过spring

![](http://i.imgur.com/dwhppwQ.jpg)

##1.pom.xml
	<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
	  <modelVersion>4.0.0</modelVersion>
	  <groupId>org.springframework.samples.service.service</groupId>
	  <artifactId>test</artifactId>
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
		<!-- cofig spring jar -->
			<dependency>  
				<groupId>org.springframework</groupId>  
				<artifactId>spring-core</artifactId>  
				<version>${spring-framework.version}</version>  
			</dependency>  
	  
			<dependency>  
				<groupId>org.springframework</groupId>  
				<artifactId>spring-expression</artifactId>  
				<version>${spring-framework.version}</version>  
			</dependency>  
	  
			<dependency>  
				<groupId>org.springframework</groupId>  
				<artifactId>spring-beans</artifactId>  
				<version>${spring-framework.version}</version>  
			</dependency>  
	  
			<dependency>  
				<groupId>org.springframework</groupId>  
				<artifactId>spring-aop</artifactId>  
				<version>${spring-framework.version}</version>  
			</dependency>  
	  
			<dependency>  
				<groupId>org.springframework</groupId>  
				<artifactId>spring-context</artifactId>  
				<version>${spring-framework.version}</version>  
			</dependency>  
	  
			<dependency>  
				<groupId>org.springframework</groupId>  
				<artifactId>spring-context-support</artifactId>  
				<version>${spring-framework.version}</version>  
			</dependency>  
			<!-- Spring MVC -->
			<dependency>
				<groupId>org.springframework</groupId>
				<artifactId>spring-webmvc</artifactId>
				<version>${spring-framework.version}</version>
			</dependency>
			<!-- Spring redis -->
			 <dependency>  
		        <groupId>org.springframework.data</groupId>  
		        <artifactId>spring-data-redis</artifactId>  
		        <version>1.3.4.RELEASE</version>  
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
	
			<!-- https://mvnrepository.com/artifact/commons-pool/commons-pool -->
	<dependency>
	    <groupId>commons-pool</groupId>
	    <artifactId>commons-pool</artifactId>
	    <version>1.6</version>
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
		<!-- https://mvnrepository.com/artifact/redis.clients/jedis -->
		<dependency>
		    <groupId>redis.clients</groupId>
		    <artifactId>jedis</artifactId>
		    <version>2.6.0</version>
		</dependency>
	
		</dependencies>	
	</project>

##2.application-config.xml
	<?xml version="1.0" encoding="UTF-8"?>
	
	<beans 
		xmlns="http://www.springframework.org/schema/beans"  
	    xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" 
	    xmlns:p="http://www.springframework.org/schema/p"  
	    xmlns:context="http://www.springframework.org/schema/context"  
	    xmlns:jee="http://www.springframework.org/schema/jee" 
	    xmlns:tx="http://www.springframework.org/schema/tx"  
	    xmlns:aop="http://www.springframework.org/schema/aop"  
	    xsi:schemaLocation="  
	            http://www.springframework.org/schema/beans 
	            http://www.springframework.org/schema/beans/spring-beans.xsd  
	            http://www.springframework.org/schema/context 
	            http://www.springframework.org/schema/context/spring-context.xsd">  
	    
	         <context:component-scan
	            base-package="com.hainan.cs"/>  
	<context:property-placeholder location="classpath:redis.properties" />  
	<bean id="poolConfig" class="redis.clients.jedis.JedisPoolConfig">  
	        <property name="maxIdle" value="${redis.maxIdle}" />  
	        <property name="testOnBorrow" value="${redis.testOnBorrow}" />  
	    </bean>  
	      
	    <bean id="connectionFactory" class="org.springframework.data.redis.connection.jedis.JedisConnectionFactory"  
	        p:host-name="${redis.host}" p:port="${redis.port}"   p:pool-config-ref="poolConfig" p:password="${redis.pass}"/>  
	      
	    <bean id="redisTemplate" class="org.springframework.data.redis.core.StringRedisTemplate">  
	        <property name="connectionFactory"   ref="connectionFactory" />  
	    </bean>     
	    <bean id="userDao" class="com.hainan.cs.dao.UserDaoImp"></bean>  
	</beans>

##3.redis.properties
    # Redis settings  
    redis.host=127.0.0.1
    redis.port=6379  
    redis.pass=
    redis.maxIdle=300  
    redis.maxActive=600  
    redis.maxWait=1000  
    redis.testOnBorrow=true  

注意我的redis是没设密码，所有redis.pass不需要填写（依个人情况而定）

##4.user.java
	package com.hainan.cs.bean;
	
	import java.io.Serializable;
	
	public class User implements Serializable{
	
		
		private static final long serialVersionUID = -3317413873854588822L;
		private String id;
		private String username;
		private String password;
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
	}

##5.RedisGeneratorDao.java
	package com.hainan.cs.dao;
	
	import java.io.Serializable;
	
	import org.springframework.beans.factory.annotation.Autowired;
	import org.springframework.data.redis.core.RedisTemplate;
	import org.springframework.data.redis.serializer.RedisSerializer;
	
	public class RedisGeneratorDao<k extends Serializable, v extends Serializable> {
	
		@Autowired
		protected RedisTemplate<k,v> redisTemplate;
		
		public void setRedisTemplate(RedisTemplate<k,v> redisTemplate){
			this.redisTemplate=redisTemplate;
		}
		protected RedisSerializer<String> getRedisSerializer(){
			return redisTemplate.getStringSerializer();
		}
	}

##6.UserDaoImp.java
	package com.hainan.cs.dao;
	
	import org.springframework.context.support.ClassPathXmlApplicationContext;
	import org.springframework.dao.DataAccessException;
	import org.springframework.data.redis.connection.RedisConnection;
	import org.springframework.data.redis.core.RedisCallback;
	import org.springframework.data.redis.serializer.RedisSerializer;
	import org.springframework.stereotype.Repository;
	
	import com.hainan.cs.bean.User;
	@Repository(value="userDao")
	public class UserDaoImp extends RedisGeneratorDao<String,User>{
		//添加对象
		public boolean add(final User user){
			System.out.println(redisTemplate);
			boolean result=redisTemplate.execute(new RedisCallback<Boolean>(){
				public Boolean doInRedis(RedisConnection connection) throws DataAccessException {
					RedisSerializer<String> serializer=getRedisSerializer();
					byte[] key=serializer.serialize(user.getId());
					byte[] name=serializer.serialize(user.getUsername());
					byte[] password=serializer.serialize(user.getPassword());
					return connection.setNX(key, name);
				}
			});
			return result;
		}
		//测试
		public static void main(String args[]){
			ClassPathXmlApplicationContext context=new ClassPathXmlApplicationContext("spring/application-config.xml");
			UserDaoImp udi=context.getBean(UserDaoImp.class);
			User u=new User();
			u.setId("1");
			u.setUsername("zhangsan");
			udi.add(u);
		}
	}

##7.结果
![](http://i.imgur.com/DKtfT92.jpg)


##8.demo下载地址
https://github.com/huangliang992/spring_redis_demo