## hibernate 可选的主键自动生产的类型

	<id  name="属性名"column="数据库字段名" type="标识Hibernate类型的名字" 
	     unsaved-value="null|any|none|undefined|id_value"
	     access="field|property|ClassName">                    
	        
	     <generator class="assigned|increment|identity|sequence|hilo|seqhilo|native|uuid|guid|select|foreign"/>
	</id>

如果 name属性不存在，会认为这个类没有标识属性 
unsaved-value 参考http://blog.csdn.net/chunkyo/article/details/660050 
access(可选 - 默认为property): Hibernate用来访问属性值的策略。 （网上查不到什么资料） 

##generator 
###1、assigned： 
   在调用session.save()之前，必须手动setId(主键值)，否则出错。 
   <generator>没有指定时的默认生成策略。 
###2、increment：自增长，为long, short或者int类型生成主键 
   自动增长，由hibernate维护，适用于所有的数据库，每次插入数据前先查询最大id（select max(id) from xxx），即使id已经设好值，无视。 
   缺点：不适合多进程并发更新数据库，适合单一进程访问数据库。不能用于群集环境。 
###3、identity：自增长 
    与increment不同的是： 
    a）identity不是由hibernate控制，而是由数据库控制自增长，所插入数据前不会查询最大id。 
    b）以Mysql为例，如果主键字段没有开启auto_increment，会抛异常"Field 'id' doesn't have a default value"。 
    c）increment的insert语句包括id，identity的insert语句没有id字段。 
    c）假设有五条数据，id分别为1、2、3、4，删除（3、4）后，用increment增加数据id为3，用identity增加数据id为5。 
    不同的数据库有不同的自增长方式。如MySql是auto_increment，SQL Server 中是Identity。支持的数据库：MySql、SQL Server、DB2、Sybase和HypersonicSQL。 
    优点： 不需要Hibernate和用户的干涉，使用较为方便。 
    缺点：但不便于在不同的数据库之间移植程序。 
###4、sequence：自增长 
    需要底层数据库支持Sequence方式。 
    实现了Sequence的数据库：Oracle、DB2、PostgreSQL。 
    没有实现Sequence的数据库：  MySQL、SQL Server、Sybase。 
    如果在MySql中使用该方式，hibernate会根据Dialect判断报错。 
###5、hilo：hi/low（高低位）算法，
高低位算法使用一个高位值和一个低位值，然后把算法得到的两个值拼接起来作为数据库中的唯一主键。Hilo方式需要额外的数据库表和字段提供高位值来源。默认请况下使用的表是hibernate_unique_key，默认字段叫作next_hi。next_hi必须有一条记录否则会出现错误。

	<generator class="hilo" >   
	             <param name="table">hi_value</param>   
	             <param name="column">next_value</param>   
	             <param name="max_lo">100</param>   
	</generator>  
	<generator class="hilo" >
	             <param name="table">hi_value</param>
	             <param name="column">next_value</param>
	             <param name="max_lo">100</param>
	</generator>

    hibernate根据hilo生成器生成主键的过程： 
     (1)读取并记录数据库的hi_value表中next_value字段的值，数据库中此字段值加1保存 
     (2)hibernate取得lo值（0到max_lo-1循环，lo到max_lo时，执行步骤1，然后lo继续从0到max_lo循环） 
     (3)取得hi值和lo值后，根据下面的公式计算主键值：hi*(max_lo+1)+lo; 
     过程参考：http://hi.baidu.com/sai5d/blog/item/88e5f4db09e90277d0164e30.html 
    个人感觉没有自增长好，不仅多维护一个表，每次插入数据都要进行查和修改next_value的值。 
###6、seqhilo基于sequence的hilo算法。 
###7、native： 
　  Native主键生成方式会根据不同的底层数据库自动选择Identity、Sequence、Hilo主键生成方式 
　　特点：根据不同的底层数据库采用不同的主键生成方式。由于Hibernate会根据底层数据库采用不同的映射方式，因此便于程序移植，项目中如果用到多个数据库时，可以使用这种方式。 
     疑问：什么时候选择Hilo？ 
###8、uuid： 
     id必须是String类型。 
　  使用128位UUID算法生成主键，能够保证网络环境下的主键唯一性，也就能够保证在不同数据库及不同服务器下主键的唯一性。 
　   特点：能够保证数据库中的主键唯一性，生成的主键占用比较多的存贮空间。 
###9、guid 
      使用了一种特殊算法，保证生成主键的唯一性，支持SQL Server和MySQL。 
      MySql中，先执行select uuid()。 uuid()是MySql自带的函数，取得uuid值后再生成主键。 
###10、select：使用数据库触发器赋值  （待研究） 
###11、foreign：使用外键赋值，在一对一实体关系时，可以保证关系双方的id保持一致。（待研究） 