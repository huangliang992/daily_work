#spring对redis的操作
	import java.util.UUID;
	
	import org.springframework.context.support.ClassPathXmlApplicationContext;
	import org.springframework.dao.DataAccessException;
	import org.springframework.data.redis.connection.RedisConnection;
	import org.springframework.data.redis.core.BoundHashOperations;
	import org.springframework.data.redis.core.RedisCallback;
	import org.springframework.data.redis.serializer.RedisSerializer;
	import org.springframework.stereotype.Repository;
	
	import com.hainan.cs.bean.User;
	@Repository(value="userDao")
	public class UserDaoImp extends RedisGeneratorDao<String,User>{
		//添加Hash hsetNX
		public boolean add(final User user){
			System.out.println(redisTemplate);
			boolean result=redisTemplate.execute(new RedisCallback<Boolean>(){
				public Boolean doInRedis(RedisConnection connection) throws DataAccessException {
					RedisSerializer<String> serializer=getRedisSerializer();
					byte[] key=serializer.serialize(user.getId());
					byte[] name=serializer.serialize(user.getUsername());
					byte[] password=serializer.serialize(user.getPassword());
					Boolean r=connection.hSetNX(key, name,password);//hash类型的提交<key,hash的key,hash的value>
					return r;
				}
			});
			return result;
		}
		//添加Hash hSet
		public boolean add1(final User user){
			System.out.println(redisTemplate);
			boolean result=redisTemplate.execute(new RedisCallback<Boolean>(){
				public Boolean doInRedis(RedisConnection connection) throws DataAccessException {
					RedisSerializer<String> serializer=getRedisSerializer();
					byte[] key=serializer.serialize(user.getId());
					byte[] name=serializer.serialize(user.getUsername());
					byte[] password=serializer.serialize(user.getPassword());
					Boolean r=connection.hSet(key, name, password);//和hSetNX一样
					return r;
				}
			});
			return result;
		}
		//添加String
		public boolean add2(final User user){
			System.out.println(redisTemplate);
			boolean result=redisTemplate.execute(new RedisCallback<Boolean>(){
				public Boolean doInRedis(RedisConnection connection) throws DataAccessException {
					RedisSerializer<String> serializer=getRedisSerializer();
					byte[] key=serializer.serialize(user.getId());
					byte[] name=serializer.serialize(user.getUsername());
					Boolean r=connection.setNX(key, name);//添加String类型
					return r;
				}
			});
			return result;
		}
		//添加List
		public void add3(final String key,final String value){
			redisTemplate.execute(new RedisCallback<Boolean>(){
				public Boolean doInRedis(RedisConnection connection) throws DataAccessException {
					RedisSerializer<String> serializer=getRedisSerializer();
					byte[] a=serializer.serialize(key);
					byte[] b=serializer.serialize(value);
					connection.lPush(a, b);
					return true;
				}
				});
		}
		//删除对象
		public void deleteKey(String key){
			redisTemplate.delete(key);
		}
		//删除hash中的一条记录
		public void deleteRecord(String key,String id){
			BoundHashOperations<String,Object,Object> bhops=redisTemplate.boundHashOps(key);
			bhops.delete(id);
		}
		//测试
		public static void main(String args[]){
			ClassPathXmlApplicationContext context=new ClassPathXmlApplicationContext("spring/application-config.xml");
			UserDaoImp udi=context.getBean(UserDaoImp.class);
			User u=new User();
			String id=UUID.randomUUID().toString();//自动生成一个id
			u.setId("test1");
			u.setUsername(id);
			u.setPassword("123456");
			udi.add1(u);
			udi.deleteKey("1");
			udi.deleteRecord("test1", "zhangsan");
			udi.add3("listtest", "test");
		}
	}

##2.将spring版本调到4以上报错
	Exception in thread "main" org.springframework.beans.factory.BeanCreationException: Error creating bean with name 'userDao': Injection of autowired dependencies failed; nested exception is org.springframework.beans.factory.BeanCreationException: Could not autowire field: protected org.springframework.data.redis.core.RedisTemplate com.hainan.cs.dao.RedisGeneratorDao.redisTemplate; nested exception is org.springframework.beans.factory.NoSuchBeanDefinitionException: No qualifying bean of type [org.springframework.data.redis.core.RedisTemplate] found for dependency: expected at least 1 bean which qualifies as autowire candidate for this dependency. Dependency annotations: {@org.springframework.beans.factory.annotation.Autowired(required=true)}
		at org.springframework.beans.factory.annotation.AutowiredAnnotationBeanPostProcessor.postProcessPropertyValues(AutowiredAnnotationBeanPostProcessor.java:292)
		at org.springframework.beans.factory.support.AbstractAutowireCapableBeanFactory.populateBean(AbstractAutowireCapableBeanFactory.java:1185)
		at org.springframework.beans.factory.support.AbstractAutowireCapableBeanFactory.doCreateBean(AbstractAutowireCapableBeanFactory.java:537)
		at org.springframework.beans.factory.support.AbstractAutowireCapableBeanFactory.createBean(AbstractAutowireCapableBeanFactory.java:475)
		at org.springframework.beans.factory.support.AbstractBeanFactory$1.getObject(AbstractBeanFactory.java:302)
		at org.springframework.beans.factory.support.DefaultSingletonBeanRegistry.getSingleton(DefaultSingletonBeanRegistry.java:228)
		at org.springframework.beans.factory.support.AbstractBeanFactory.doGetBean(AbstractBeanFactory.java:298)
		at org.springframework.beans.factory.support.AbstractBeanFactory.getBean(AbstractBeanFactory.java:193)
		at org.springframework.beans.factory.support.DefaultListableBeanFactory.preInstantiateSingletons(DefaultListableBeanFactory.java:703)
		at org.springframework.context.support.AbstractApplicationContext.finishBeanFactoryInitialization(AbstractApplicationContext.java:760)
		at org.springframework.context.support.AbstractApplicationContext.refresh(AbstractApplicationContext.java:482)
		at org.springframework.context.support.ClassPathXmlApplicationContext.<init>(ClassPathXmlApplicationContext.java:139)
		at org.springframework.context.support.ClassPathXmlApplicationContext.<init>(ClassPathXmlApplicationContext.java:83)
		at com.hainan.cs.dao.UserDaoImp.main(UserDaoImp.java:83)
	Caused by: org.springframework.beans.factory.BeanCreationException: Could not autowire field: protected org.springframework.data.redis.core.RedisTemplate com.hainan.cs.dao.RedisGeneratorDao.redisTemplate; nested exception is org.springframework.beans.factory.NoSuchBeanDefinitionException: No qualifying bean of type [org.springframework.data.redis.core.RedisTemplate] found for dependency: expected at least 1 bean which qualifies as autowire candidate for this dependency. Dependency annotations: {@org.springframework.beans.factory.annotation.Autowired(required=true)}
		at org.springframework.beans.factory.annotation.AutowiredAnnotationBeanPostProcessor$AutowiredFieldElement.inject(AutowiredAnnotationBeanPostProcessor.java:508)
		at org.springframework.beans.factory.annotation.InjectionMetadata.inject(InjectionMetadata.java:87)
		at org.springframework.beans.factory.annotation.AutowiredAnnotationBeanPostProcessor.postProcessPropertyValues(AutowiredAnnotationBeanPostProcessor.java:289)
		... 13 more
	Caused by: org.springframework.beans.factory.NoSuchBeanDefinitionException: No qualifying bean of type [org.springframework.data.redis.core.RedisTemplate] found for dependency: expected at least 1 bean which qualifies as autowire candidate for this dependency. Dependency annotations: {@org.springframework.beans.factory.annotation.Autowired(required=true)}
		at org.springframework.beans.factory.support.DefaultListableBeanFactory.raiseNoSuchBeanDefinitionException(DefaultListableBeanFactory.java:1103)
		at org.springframework.beans.factory.support.DefaultListableBeanFactory.doResolveDependency(DefaultListableBeanFactory.java:963)
		at org.springframework.beans.factory.support.DefaultListableBeanFactory.resolveDependency(DefaultListableBeanFactory.java:858)

我们来看下原因，主要是下面造成的，说明通过xml方式注入的redisTemplate有问题，所以通过@Autowired 关联有问题。
`No qualifying bean of type [org.springframework.data.redis.core.RedisTemplate] found for dependency`

也就是这个地方有问题

    <bean id="redisTemplate" class="org.springframework.data.redis.core.StringRedisTemplate">  
        <property name="connectionFactory"   ref="connectionFactory" />  
    </bean>   

主要是由下面几个插件造成的，版本升到4.0的话会出错

		<dependency>  
			<groupId>org.springframework</groupId>  
			<artifactId>spring-context</artifactId>  
			<version>3.2.3.RELEASE</version>  
		</dependency>  
  
		<dependency>  
			<groupId>org.springframework</groupId>  
			<artifactId>spring-context-support</artifactId>  
			<version>3.2.3.RELEASE</version>  
		</dependency>  

		<dependency>  
			<groupId>org.springframework</groupId>  
			<artifactId>spring-beans</artifactId>  
			<version>3.2.3.RELEASE</version>  
		</dependency>    