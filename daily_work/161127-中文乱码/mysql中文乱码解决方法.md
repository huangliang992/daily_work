## hibernate 和 mysql中文乱码问题

（1）查看默认的编码格式

	show variables like "%char%";

（2）设置编码格式,设置成下图所示
 
	show variables like "%char%";
	SET character_set_database='utf8';
	SET character_set_server='utf8';
	show variables like "%char%";

![](http://i.imgur.com/XoGpYmX.jpg)

（3）查看数据库编码格式,发现是拉丁格式，把它改成utf-8
		
	show create table online_chat;

![](http://i.imgur.com/Z5kzlq2.jpg)

	ALTER DATABASE `online_chart` DEFAULT CHARACTER SET utf8 COLLATE utf8_general_ci;

（4）查看和设置表的默认编码格式

	user online_chart;
	show create table userbean;

![](http://i.imgur.com/KYaANV5.jpg)

发现是拉丁格式，改成utf8

	ALTER TABLE `userbean` DEFAULT CHARACTER SET utf8 COLLATE utf8_general_ci;
	ALTER TABLE `educationrecord` DEFAULT CHARACTER SET utf8 COLLATE utf8_general_ci;


（5）查看列属性的编码格式

使用右键表格，选alter tables（改变表格），可以改变列的编码格式。

![](http://i.imgur.com/6amYNyP.jpg)

（6）使用hibernate配置的话，配置源编码格式这样改

	<!-- 配置数据源 -->
	    <bean id="dataSource" class="org.apache.commons.dbcp.BasicDataSource"
			destroy-method="close">
			<property name="driverClassName" value="com.mysql.jdbc.Driver" />
			<property name="url" value="jdbc:mysql://localhost:3306/crawler_paper?useUnicode=true&amp;characterEncoding=UTF-8" />
			<property name="username" value="root" />
			<property name="password" value="root" />
		</bean>