## mysql入门教程
### 目的：
是为了熟悉一些sql的基本操作，包括权限设置，数据库连接，实例创建，数据库的操作，原因是之前装了mysql因为数据库权限问题，老出错，卸了又重装了。
创建一个数据库实例uml，在里面新建一张表user，添加一些属性和数据。



### 过程：
1. 配置mysql环境变量，类似java环境变量配置


# 在cmd操作
2. cmd 输入 `mysql --version`查看版本信息
3. 输入 `mysql -u root -p` 登陆数据库（按提示输入root用户密码）

![](http://i.imgur.com/HNV9HRI.png)

4. 输入 `show databases；`（记得加分号）    
![](http://i.imgur.com/h8rH0ta.png)    
mysql是数据库服务的名称

5. 输入 `use mysql； show tables` 进入数据库mysql查看数据库中的表    
![](http://i.imgur.com/55WF2sn.png)    

6. 输入 `select * from help_category` 查看表 help_category中所有的数据    
![](http://i.imgur.com/DjcqiPt.png)

7. 输入 `create database sam_db` 创建名为`sam_db`的数据库    
![](http://i.imgur.com/yUX8KAO.png)    
8. 选择数据库还可以使用`mysql -D sam_db -u root -p`来进入数据库；或者向之前那样登陆后输入`use sam_db;`    

![](http://i.imgur.com/AoZwKtc.png)

9. 输入     创建一张students表，容易输错

		create table students（
		id int unsigned not null auto_increment primary key,
		name char(8) not null,
		sex char(4) not null,
		age tinyint unsigned not null,
		tel char(13) null default "-");

![](http://i.imgur.com/kYS1S8C.png)

10. 往数据库表中插入数据，`insert into students values(NULL, "王刚", "男", 20, "13811371377")` 

![](http://i.imgur.com/Omcs4l3.png)

11. 插入部分数据，`insert into students (name,sex,age) values("孙丽华", "女", 21)`

![](http://i.imgur.com/RtGTicy.png)

 12. 列表查询,`select name,age from students;`， 可以看到刚刚插入的两条数据。

![](http://i.imgur.com/hzwiWG4.png)

或者使用通配符*号

![](http://i.imgur.com/5V7thmG.png)

13. 特定条件查询，`select * from students where sex="女"；`

![](http://i.imgur.com/pYjGUSD.png)

14. 使用update更新数据，`update students set name="孙例化" where id="2";`

![](http://i.imgur.com/M3yE6WR.png)

15. 删除表中数据，`delete from students where id=1`;

16. 修改表的属性（添加列，删除列等），`alter table students add address char(60);`添加adress列。`alter table students add birthday date after age;`在age后面添加birthday列。

![](http://i.imgur.com/gi6nLh9.png)

`alter table 表名 change 列名称 列新名称 新数据类型;` 修改列名。

`alter table 表名 drop 列名称;`删除列。

`alter table 表名 rename 新表名;`重命名整张表。

`drop table 表名;`删除整张表。

`drop database 数据库名;`删除整个数据库。

`mysqladmin -u root -p password 新密码`修改root密码。

或者mysql> set password for root@localhost = password("root");设密码


# 在mysql workbench 操作

控制台初始界面

![](http://i.imgur.com/xuylCtB.png)

创建一个新的数据库

![](http://i.imgur.com/2D2l27a.png)

右键set as default schema。设置为此次连接的默认数据库

创建一张user表

![](http://i.imgur.com/i7f3rod.png)

插入数据

![](http://i.imgur.com/aqMNdXl.png)


## 故障分析

首先命令台输入 net start mysql；//启动mysql服务

如果提示服务名无效，那么输入 mysqld install；//安装服务

再启动。

感觉好蛋碎，原来数据地址在C:\ProgramData\MySQL\MySQL Server 5.7\data，现在跑到了C:\Program Files\MySQL\MySQL Server 5.7\data。把原来数据拷过来不知道有没有用。发现还是有用的。就是不知道hibernate连会不会有问题。

![](http://i.imgur.com/coJD3xZ.png)

## 数据库显示不全
因为权限问题
直接输mysql只能出现两个数据库
使用mysql -u root -p接着输密码显示正确