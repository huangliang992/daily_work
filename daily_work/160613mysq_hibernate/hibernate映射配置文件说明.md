## hibernate映射配置文件中的属性说明

	<?xml version="1.0"?>

	<!DOCTYPE hibernate-mapping PUBLIC

        "-//Hibernate/Hibernate Mapping DTD 3.0//EN"

        "http://hibernate.sourceforge.net/hibernate-mapping-3.0.dtd">

	<hibernate-mapping>

    <class name="cn.itcast.f_hbm_component.User" table="user">

       <id name="id">

            <generator class="native"/>

       </id>

       <property name="name"></property>

    </class>

	</hibernate-mapping>

 

一、映射主键的配置方法：

 

主要是由generator子元素是指定主键生成策略，详细说明如下：

	<id name="id">

    <generator class="native"/>

	</id>

	<!-- identity，使用数据库的自动增长，在保存时会忽略手工指定的主键值而由数据库生成，要求此属性要是数字类型

      <generator class="identity"/>

	-->

	<!-- assigned，手工指定，比如指定UUID

      <generator class="assigned"/>

	-->

	<!-- uuid，由Hibernate生成UUID并指定为主键值，要求此属性要是String型

      <generator class="uuid"/>

	-->

	<!-- hilo，高低位生成主键，需要用到一个额外的表，所有的数据库都可以使用这种类型

      <generator class="hilo">

      <param name="table">hi_value</param>

      <param name="column">next_value</param>

      <param name="max_lo">100</param>

      </generator>

	-->

 	<!-- native，根据底层数据库的能力选择 identity、sequence 或者 hilo 中的一个

      <generator class="native"/>

	-->

 

二、普通属性的声明方法

 

	<property name="name" type="string" column="name" not-null="true" length="35"/>

	<property name="name"></property>     

	<property name="gender"></property>

	<!-- 日期要指定什么类型 -->

	<property name="birthday" type="date"></property>

	<!-- 大文本类型，最好指定长度 -->

	<property name="desc" column="`desc`" type="text" length="5000"></property>

	<!-- 二进制类型，最好指定长度 -->

	<property name="photo" type="binary" length="512000"></property>

	<!--

    ---------------------------说明---------------------------------

    name：对象中的属性名，必须要有

    type：数据的类型，不写时会自动检测

    column：对应的列名，不写时默认为属性的名称

    not-null：true/false，是否有非空约束，默认为false

    length：长度，默认为255

	-->

最好都指定类型，类型制定的有两种包括：Hibernate类型制定和Java基本数据类型制定，详细的指定方法如下所示：

 

三、组成关系映射

 

直接新建一张表，表结构如下：

	<component name="userAddress"   class="cn.itcast.UserAddress">

    <property name="address"></property>

    <property name="code"></property>

    <property name="phone"></property>

	</component>

 

四、集合关系映射

	<set name="addressSet" table="user_addressSet">

    <key column="userId"/> 关联列 == id

    <element column="address" type="string"></element>

	</set>

### 特别要注意主键的设置，如果主键不对，不能正常创建表