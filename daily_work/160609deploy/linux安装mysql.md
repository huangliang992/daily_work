## linux 上安装mysql

###1. 下载mysql

![](http://i.imgur.com/Xsr7XWH.jpg)

###2. 安装mysql
进入安装包所在目录，执行命令：tar mysql-5.6.17-linux-glibc2.5-i686.tar.gz

复制解压后的mysql目录到系统的本地软件目录:
执行命令：cp mysql-5.6.17-linux-glibc2.5-i686 /usr/local/mysql -r
注意：目录结尾不要加/

添加系统mysql组和mysql用户：
执行命令：groupadd mysql和useradd -r -g mysql mysql

安装数据库：
进入安装mysql软件目录：执行命令 cd /usr/local/mysql
修改当前目录拥有者为mysql用户：执行命令 chown -R mysql:mysql ./
安装数据库：执行命令 ./scripts/mysql_install_db --user=mysql
修改当前目录拥有者为root用户：执行命令 chown -R root:root ./
修改当前data目录拥有者为mysql用户：执行命令 chown -R mysql:mysql data
到此数据库安装完毕

启动mysql服务和添加开机启动mysql服务：
添加开机启动：执行命令cp support-files/mysql.server /etc/init.d/mysql，把启动脚本放到开机初始化目录
启动mysql服务：执行命令service mysql start
执行命令：ps -ef|grep mysql 看到mysql服务说明启动成功，如图

修改mysql的root用户密码，root初始密码为空的：
执行命令：./bin/mysqladmin -u root password '密码'

把mysql客户端放到默认路径：
ln -s /usr/local/mysql/bin/mysql /usr/local/bin/mysql
注意：建议使用软链过去，不要直接包文件复制，便于系统安装多个版本的mysql