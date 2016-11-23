### java 中常用的设计模式
### 1.单例模式（有的书上说叫单态模式其实都一样）

该模式主要目的是使内存中保持1个对象。看下面的例子：

    package org.sp.singleton;
    
    //方法一
    public class Singleton {
    //将自身的实例对象设置为一个属性,并加上Static和final修饰符
    private static final Singleton instance = new Singleton();
    //将构造方法设置成私有形式
    private Singleton() {
    }
    //通过一个静态方法向外界提供这个类的实例
    public static Singleton getInstance() {
       return instance;
    }
    
    }
    
    //方法二
    class Singleton2 {
    
    private static Singleton2 instance2 = null;
    
    public static synchronized Singleton2 getInstance() {
    
       if (instance2 == null)
    instance2 = new Singleton2();
       return instance2;
    }
    }

注：这二个方法实现了一样的功能，但个人推荐采用第一种方法。

### 2.工厂模式

该模式主要功能是统一提供实例对象的引用。看下面的例子：

    public class Factory{
    public ClassesDao getClassesDao(){
       ClassesDao cd = new ClassesDaoImpl();
       return cd;
    }
    }
    
    interface ClassesDao{
    public String getClassesName();
    
    }
    
    class ClassesDaoImpl implements ClassesDao {
    public String getClassesName(){
       System.out.println("A班");
    }
    }
    
    class test
    {
    public static void main(String[] args){
       Factory f = new Factory();
       f.getClassesDao().getClassesName();
    }
    }

这个是最简单的例子了，就是通过工厂方法通过接口获取对象的引用

###3.建造模式

该模式其实就是说，一个对象的组成可能有很多其他的对象一起组成的，比如说，一个对象的实现非常复杂，有很多的属性，而这些属性又是其他对象的引用，可能这些对象的引用又包括很多的对象引用。封装这些复杂性，就可以使用建造模式。

具体看看下面的例子：

###4.门面模式

这个模式个人感觉像是Service层的一个翻版。比如Dao我们定义了很多持久化方法，我们通过Service层将Dao的原子方法组成业务逻辑，再通过方法向上层提供服务。门面模式道理其实是一样的。

具体看看这个例子：

    interface ClassesDao{
    public String getClassesName();
    
    }
    
    class ClassesDaoImpl implements ClassesDao {
    public String getClassesName(){
       return "A班";
    }
    }
    
    interface ClassesDao2{
    public String getClassesName();
    
    }
    
    class ClassesDaoImpl2 implements ClassesDao {
    public String getClasses2Name(){
       return "B班";
    }
    }
    
    class ServiceManager
    {
    private ClassesDao cd = new ClassesDaoImpl();
    private ClassesDao2 cd2 = new ClassesDaoImpl2();
    public void printOut(){
       System.out.println(cd.getClassesName()+"   "+cd2.getClassesName());
    }
    };

虽然这个例子不全，但基本意思已经很明显了。

###5.策略模式

这个模式是将行为的抽象，即当有几个类有相似的方法，将其中通用的部分都提取出来，从而使扩展更容易。

看这个例子：

    package org.sp.strategy;
    
    /**
    * 加法具体策略类
    * @author 无尽de华尔兹
    *
    */
    public class Addition extends Operation {
    
    @Override
    public float parameter(float a, float b) {
       return a+b;
    }
    
    }
    
    package org.sp.strategy;
    
    /**
    * 除法具体策略类
    * @author 无尽de华尔兹
    *
    */
    public class Division extends Operation {
    
    @Override
    public float parameter(float a, float b) {
       return a/b;
    }
    
    }
    
    package org.sp.strategy;
    
    /**
    * 乘法具体策略类
    * @author 无尽de华尔兹
    *
    */
    public class Multiplication extends Operation{
    
    @Override
    public float parameter(float a, float b) {
       return a*b;
    }
    
    }
    
     
    
    package org.sp.strategy;
    
    /**
    * 减法具体策略类
    * @author 无尽de华尔兹
    *
    */
    public class Subtration extends Operation {
    
    @Override
    public float parameter(float a, float b) {
       return a-b;
    }
    
    }
    
     
    
    package org.sp.strategy;
    
    /**
    * 抽象策略类也可以使用接口来代替
    * @author 无尽de华尔兹
    *
    */
    public abstract class Operation {
    
    public abstract float parameter(float a, float b);
    }
    
    package org.sp.strategy;
    
    /**
    * 策略环境类 
    * @author 无尽de华尔兹
    *
    */
    public class Condition {
    
    public static final Addition add = new Addition();
    
    public static final Subtration sub = new Subtration();
    
    public static final Multiplication mul = new Multiplication();
    
    public static final Division div = new Division();
    
    }
    
    package org.sp.strategy;
    
    /**
    * 测试客户端
    * @author 无尽de华尔兹
    *
    */
    public class Client {
    
    public static void main(String[] args) {
       float a = 100;
       float b = 25;
      
       System.out.println(Condition.div.parameter(a, b));
    }
    
    }