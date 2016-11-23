## 登陆注册设计
### 1.注册设计
jsp包含：username，password，email  (注册可以不要填那么多信息，登陆后再完善) 
数据库：mysql    
userbean：对应数据库中的用户表username,password,email,telephone,borntime,sex,work
userdao:操作数据库中的用户表
SignUpaction：调用userdao将用户输入的注册信息提交到数据库

### 2.登陆设计
jsp包含：username，password，验证码    
userbean：同注册    
loginaction：调用userdao验证用户的登陆信息

### 3.loginaction和signupaction可合并为useraction，通过不同的方法实现


### 4.DAO设计模式
包含数据库连接类DBConnector，数据库表的操作类UserDao，实体类UserBean

DAO设计模式一般分为几个类：    
1.VO(Value Object)：一个用于存放网页的一行数据即一条记录的类，比如网页要显示一个用户的信息，则这个类就是用户的类。    
2.DatabaseConnection：用于打开和关闭数据库。    
3.DAO接口：用于声明对于数据库的操作。    
4.DAOImpl：必须实现DAO接口，真实实现DAO接口的函数，但是不包括数据库的打开和关闭。    
5.DAOProxy：也是实现DAO接口，但是只需要借助DAOImpl即可，但是包括数据库的打开和关闭。    
6.DAOFactory：工厂类，含有getInstance()创建一个Proxy类。        

##### DAO与hibernet

dao层主要封装的是对数据库的CUID操作，hibernate其实封装的是JDBC的操作。也就是通过对对象层面的操作来实现对数据库的操作，Hibernate底层实现也是JDBC。所以呢，你以前在dao使用jdbc进行数据库操作，那么现在我们使用Hibernate把jdbc封装了方便你用jdbc的方法，那么也可以理解为把jdbc替换成了hibernate。所以要使用hibernate啦！！
