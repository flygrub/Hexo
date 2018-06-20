title: linux搭建apache-mysql-PHP网站
date: 2018-06-16 11:37:28
tags: linux php apache mysql git
---

---
>教程支持centos6  
centos 查看操作系统版本信息  
```
cat /proc/version  
cat /etc/issue  
getconf LONG_BIT  
```
---

#### 安装Apache  

1. 检查、删除、安装

```
rpm -qa|grep http     //检查是否安装apache  
rpm -qa|grep httpd    //检查是否安装apache
rpm -e 包名 --nodeps    //若有则删除  
yum install httpd     //安装，根据提示，输入Y即可安装成功  
```

2. 启动、测试  
```
/etc/init.d/httpd start
```
>备注：可以使用`/etc/init.d/httpd status/stop/start/restart // 分别对应查看状态/停止/启动/重启`  
也可以使用`service httpd  status/stop/start/restart`命令，效果一样的  

在windows浏览器输入服务器IP，可以看到Apache欢迎界面，即表示apache安装成功

3. 配置  

>打开 `/etc/httpd/conf/httpd.cnf`  
找到`DocumentRoot`修改自己的路径，例如
`DocumentRoot "/var/www/html/test"`  
找到`<Directory /> `，把里面的`none`改为`all`  
找到`<Directory "/var/www/html">`，修改自己的路径，把里面的`none`改为`all`

#### 安装MySql  

1. 安装  
查看有没有安装过:  
```
yum list installed mysql* // 查看已经安装的mysql
rpm -qa | grep mysql*
```
查看yum有没有安装包  
```
yum list mysql*
```
安装mysql  

```
yum install mysql mysql-server mysql-devel
```

2. 启动 停止  

启动mysql服务  

```
service mysqld start  
或者  
/etc/init.d/mysqld start
```

停止  
```
service mysqld stop
```
3. 登录  

创建root管理员：  
```
mysqladmin -u root password 123456
```
登录：  
```
mysql -u root -p  // 回车然后输入密码即可
```
忘记密码(如果忘记的话)：

```
service mysqld stop   
mysqld_safe --user=root --skip-grant-tables    
mysql -u root  
use mysql;  
update user set password=password("new_pass") where user="root";  
flush privileges;  
```

4. 远程访问  

```
mysql -u root -p   //-u后面是用户名,
use mysql;  
select Host,User from user;  
update user set Host='%' where User='root'; //出现错误不用理睬  
flush privileges;  
select Host,User from user;  
```
5. Linux MySQL的几个重要目录

>数据库目录：`/var/lib/mysql/`  
配置文件：`/usr/share /mysql（mysql.server命令及配置文件）`  
相关命令：`/usr/bin（mysqladmin mysqldump等命令）`  
启动脚本：`/etc/rc.d/init.d/（启动脚本文件mysql的目录）`  

#### 安装PHP  

查看安装服务器当前安装版本  
```
php -V
```
更新yum  
```
yum -y update
```
运行如下命令  
```
yum clean all
yum makecache
```

根据自己的操作系统安装yum源  
CentOS/RHEL 7.x:  
```
rpm -Uvh https://mirror.webtatic.com/yum/el7/epel-release.rpm  
rpm -Uvh https://mirror.webtatic.com/yum/el7/webtatic-release.rpm
```
CentOS/RHEL 6.x:  
需要执行下面两句  
```
rpm -Uvh https://dl.fedoraproject.org/pub/epel/epel-release-latest-6.noarch.rpm  
rpm -Uvh https://mirror.webtatic.com/yum/el6/latest.rpm
```
CentOS/RHEL 5.x:  
```
rpm -Uvh https://mirror.webtatic.com/yum/el5/latest.rpm
```

查看当前yum源中是否有自己需要安装的PHP版本  
```
yum list php* 
```
查看当前安装包，并将其记录下来，方便后面安装  
```
yum list installed | grep php  
```
删除原来PHP包  
```
yum remove php*  
```
安装新的版本  
```
yum install -y php56w php56w-bcmath php56w-cli php56w-common php56w-dba php56w-devel php56w-embedded php56w-enchant php56w-fpm php56w-gd php56w-imap php56w-interbase php56w-intl php56w-ldap php56w-mbstring php56w-mcrypt php56w-mssql php56w-mysql php56w-odbc php56w-opcache php56w-pdo php56w-pear.noarch php56w-pecl-apcu php56w-pecl-apcu-devel php56w-pecl-gearman php56w-pecl-geoip php56w-pecl-igbinary php56w-pecl-igbinary-devel php56w-pecl-imagick php56w-pecl-imagick-devel php56w-pecl-memcache php56w-pecl-memcached php56w-pecl-mongodb php56w-pecl-redis php56w-pecl-xdebug php56w-pgsql php56w-phpdbg php56w-process php56w-pspell php56w-recode php56w-snmp php56w-soap php56w-tidy php56w-xml php56w-xmlrpc
```

写完php程序或者修改程序后，执行命令`service php-fpm reload`重新加载  

#### 安装Git  

系统CentOS 64 下载  
1. 介绍  

使用Coding管理项目，上面要求使用的git版本为1.8.0以上，而很多yum源上自动安装的git版本为1.7，所以需要掌握手动编译安装git方法。  

2. 安装git依赖包  

```
yum install curl-devel expat-devel gettext-devel openssl-devel zlib-devel gcc perl-ExtUtils-MakeMaker
```
3. 删除已有的git  

```
yum remove git
```
4. 下载git源码  

切换到你的包文件存放目录下  
```
cd /usr/src
```
下载git安装包  
```
wget https://www.kernel.org/pub/software/scm/git/git-2.8.3.tar.gz
```
解压git安装包  
```
tar -zxvf git-2.8.3.tar.gz
cd git-2.8.3
```
配置git安装路径  
```
./configure prefix=/usr/local/git/
```
编译并且安装  
```
make && make install
```
查看git版本号  
```
git --version
```
git已经安装完毕  

5. 将git指令添加到bash中  

打开配置
```
vi /etc/profile
```
在最后一行加入  
```
export PATH=$PATH:/usr/local/git/bin
```
让该配置文件立即生效  
```
source /etc/profile
```