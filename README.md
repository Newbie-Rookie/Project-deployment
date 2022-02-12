# 手把手教你在Linux系统进行项目部署

**百度网盘：[https://pan.baidu.com/s/11wMIXT68GX7banf3Gaflrw](https://pan.baidu.com/s/11wMIXT68GX7banf3Gaflrw) （提取码：lzt1）**

**GitHub：[https://github.com/Newbie-Rookie/Project-deployment](https://github.com/Newbie-Rookie/Project-deployment)**

已经将项目部署需要用到的工具放到网盘中和GitHub上

在进行项目部署前，你先需要拥有**一台服务器、XShell软件、XFtp软件**

**（1） 服务器**

上阿里云、腾讯云租一台即可，可以看看官网的最新活动的限时秒杀（[腾讯云](https://cloud.tencent.com/act/new?from=15099)），每天固定时间有优惠活动。

我目前用的是腾讯云的云服务器，学生有优惠https://cloud.tencent.com/act/campus（需要学生认证）

用购买服务器的账号登录服务器控制台（[腾讯云控制台](https://cloud.tencent.com/login?s_url=https%3A%2F%2Fconsole.cloud.tencent.com%2F)），找到购买的云服务器，进入实例

我们需要记住服务器初始登录名为root，密码我们进行重置密码，更改为我们容易记住的密码，用于登录XShell和XFtp

我们还要记住公网IP

![](https://img2022.cnblogs.com/blog/2180460/202202/2180460-20220213051952113-2118606732.png)

**(2) XShell和XFtp**

XShell是命令操作服务器上资源的软件，XFtp是可视化操作服务器上资源的软件。

官网下载XShell 7和XFtp 7即可，但目前不知道为什么进不去官网，我把软件放网盘和github上，需要的自取。

**① XShell连接云服务器：**

![](https://img2022.cnblogs.com/blog/2180460/202202/2180460-20220213052137919-1241757029.png)

点击该连接进行连接，需要再次输入登录名root和你所重置的密码即可连接成功。

**② XFtp连接云服务器：**

填写云服务器公网IP、登录名和密码，进行连接即可

![](https://img2022.cnblogs.com/blog/2180460/202202/2180460-20220213052422709-2066666923.png)


以上将服务器、XShell和XFtp准备好后，我们就需要搭建项目运行环境，分四个步骤。

# 1、Linux安装jdk8（或其他版本）

## 1.1 下载jdk8.0

**Oracle官网：https://www.oracle.com/java/technologies/downloads/#java8**

需要Oracle账户（百度搜索Oracle账户，嫖一个下载就ok）

**下载：jdk-8u321-linux-x64.tar.gz**（已为你们准备了jdk-8u311-linux-x64.tar.gz在网盘和GitHub中）

![](https://img2022.cnblogs.com/blog/2180460/202202/2180460-20220213052500945-4612710.png)


## 1.2 源代码解压

XFtp进入/usr/local，将jdk-8u311-linux-x64.tar.gz进去

![](https://img2022.cnblogs.com/blog/2180460/202202/2180460-20220213052622024-1399245860.png)


以下在XShell中进行：

进入/usr/local，即jdk源代码压缩包位置：

```shell
cd /usr/local
```

解压jdk源代码压缩包：

```shell
tar -zxvf jdk-8u311-linux-x64.tar.gz
```

删除jdk源代码压缩包：

```shell
rm -f jdk-8u311-linux-x64.tar.gz
```

解压成功：

![](https://img2022.cnblogs.com/blog/2180460/202202/2180460-20220213052721952-333526765.png)


## 1.3 配置jdk环境变量

/etc/profile文件的改变会涉及到系统的环境，也就是有关Linux环境变量的东西

所以，我们要将jdk配置到/etc/profile，才可以在任何一个目录访问jdk

```shell
vim /etc/profile
```

按i进入编辑，在profile文件尾部添加如下内容：

```shell
export JAVA_HOME=/usr/local/jdk1.8.0_311  #jdk安装目录
 
export JRE_HOME=${JAVA_HOME}/jre
 
export CLASSPATH=.:${JAVA_HOME}/lib:${JRE_HOME}/lib:$CLASSPATH
 
export JAVA_PATH=${JAVA_HOME}/bin:${JRE_HOME}/bin
 
export PATH=$PATH:${JAVA_PATH}
```

添加后如下：

![](https://img2022.cnblogs.com/blog/2180460/202202/2180460-20220213052820198-1229406345.png)


然后按Esc退出编辑，按:wq退出配置文件即可

通过命令source /etc/profile让profile文件立即生效：

```shell
source /etc/profile
```

## 1.4 测试jdk是否安装成功

```shell
javac
```

![](https://img2022.cnblogs.com/blog/2180460/202202/2180460-20220213053018724-1531120135.png)


```java
java -version
```

![](https://img2022.cnblogs.com/blog/2180460/202202/2180460-20220213053120135-912570304.png)



# 2、Linux安装Tomcat

## 2.1 下载安装包

我部署的项目中使用的是apache-tomcat-9.0.56，所以网盘和GitHub为你们准备了**apache-tomcat-9.0.56.tar.gz**

项目中使用的9及9以下版本都可以用，如果使用的是10以上版本，需要自己到[官网](https://tomcat.apache.org/)下载，因为10以上版本的包名已经和9及9以下版本的包名不同，9及9以下版本

## 2.2 上传文件/解压

XFtp进入/usr/local，将apache-tomcat-9.0.56.tar.gz进去

解压：

```shell
tar -zxvf apache-tomcat-9.0.56.tar.gz
```

重命名：

```shell
mv apache-tomcat-9.0.56 tomcat
```

删除压缩包：

```shell
rm -f apache-tomcat-9.0.56.tar.gz
```

解压成功：

![](https://img2022.cnblogs.com/blog/2180460/202202/2180460-20220213053356850-1873191273.png)


## 2.3 启动tomcat

进入目录tomcat的bin目录：

```shell
cd /usr/local/tomcat/bin/
```

启动：

```shell
./startup.sh
```

![](https://img2022.cnblogs.com/blog/2180460/202202/2180460-20220213053417953-160579894.png)


查看tomcat启动效果：

在浏览器输入公网IP + 8080 （8080默认端口）

![](https://img2022.cnblogs.com/blog/2180460/202202/2180460-20220213053428924-1731294403.png)


这就表示成功在Linux上安装了Tomcat。

# 3、Linux安装MySQL

我的项目使用的是MySQL8.0，所以我安装MySQL8.0

## 3.1 安装前清理工作（卸载mysql）

### 3.1.1 清理原有的mysql数据库

使用以下命令查找出安装的mysql软件包和依赖包：

```shell
rpm -pa | grep mysql
```

可能显示结果如下：

```shell
mysql80-community-release-el7-1.noarch
mysql-community-server-8.0.11-1.el7.x86_64
mysql-community-common-8.0.11-1.el7.x86_64
mysql-community-libs-8.0.11-1.el7.x86_64
mysql-community-client-8.0.11-1.el7.x86_64
```

使用以下命令依次删除上面的程序：

```shell
yum remove 名字
```

删除mysql的配置文件，卸载不会自动删除配置文件，首先使用如下命令查找出所用的配置文件：

```shell
find / -name mysql
```

可能显示结果如下：

```shell
/etc/selinux/targeted/active/modules/100/mysql
/usr/share/mysql
/usr/lib64/mysql
```

使用以下命令依次删除上面的程序：

```shell
rm -rf 名字
```

### 3.1.2 删除MariaDB的文件

由于MySQL在CentOS 7中收费了，所以已经不支持MySQL了，取而代之在CentOS7内部集成了mariadb，而安装MySQL的话会和MariaDB的文件冲突，所以需要先卸载掉MariaDB.

使用rpm命令查找出要删除的mariadb文件：

```shell
rpm -pa | grep mariadb

可能的显示结果如下：
mariadb-libs-5.5.56-2.el7.x86_64  

rpm -e mariadb-libs-5.5.56-2.el7.x86_64  #删除上面的程序
```

可能出现错误提示如下：

依赖检测失败：

```shell
libmysqlclient.so.18()(64bit) 被 (已安裝) postfix-2:2.10.1-6.el7.x86_64 需要

libmysqlclient.so.18(libmysqlclient_18)(64bit) 被 (已安裝) postfix-2:2.10.1-6.el7.x86_64 需要

libmysqlclient.so.18(libmysqlclient_18)(64bit) 被 (已安裝) postfix-2:2.10.1-6.el7.x86_64 需要
```

使用强制删除：

```shell
rpm -e --nodeps mariadb-libs-5.5.56-2.el7.x86_64
```

至此就将原来有的mysql 和mariadb数据库删除了。

## 3.2 安装MySQL

目前我用的是腾讯云CentOS 7.6 64位，将安装MySQL8.0

由于从CentOS 7系统开始,MariaDB成为yum源中默认1的数据库安装包，如果我们的Linux操作系统为CentOS 7及以上版本，使用yum命令安装MySQL包时可能将无法启动MySQL，且自己从官网下载Linux系统的MySQL安装包到Linux系统中也一样，也会出现无法启动MySQL服务的问题，我尝试了许多方法，暂时没有找到解决方法。

直到我发现了一篇博文的方法比较靠谱，而且尝试可行：[CentOS 7.6 安装MySQL8.0](https://blog.csdn.net/java_andorid/article/details/107512502)

即手动下载mysql的Yum Repository

### 3.2.1 下载Yum Repository

到MySQL官网下载mysql的Yum Repository：[**MySQL Yum Repository**](https://dev.mysql.com/downloads/repo/yum/)

![](https://img2022.cnblogs.com/blog/2180460/202202/2180460-20220213053553837-743099218.png)


根据不同版本的CentOS系统进行下载，点击Download，我的是Linux 7

![](https://img2022.cnblogs.com/blog/2180460/202202/2180460-20220213053620291-1778819945.png)




使用XShell，连接服务器，用wget下载mysql的rpm包：

```shell
wget https://repo.mysql.com//mysql80-community-release-el7-5.noarch.rpm
```

![](https://img2022.cnblogs.com/blog/2180460/202202/2180460-20220213053630100-1096938875.png)


如上图，则下载成功。

### 3.2.3 安装MySQL（官网最新）

```shell
yum -y install mysql-community-server
```

![](https://img2022.cnblogs.com/blog/2180460/202202/2180460-20220213053827179-1364263374.png)


最后结果如上图，则安装成功。

### 3.2.4 启动mysql服务并查看mysql状态

启动mysql服务：

```shell
systemctl start mysqld.service
```

查看mysql状态：

```shell
systemctl status mysqld.service
```

![](https://img2022.cnblogs.com/blog/2180460/202202/2180460-20220213054048558-1802042608.png)


active表示mysql服务已启动。

### 3.2.5 获取mysql的root用户初始密码并修改

获取root用户初始密码：

```shell
grep "password" /var/log/mysqld.log 
```

![](https://img2022.cnblogs.com/blog/2180460/202202/2180460-20220213054114422-554747319.png)


初始密码先记住，最好复制，第一次登陆时要用，如果忘记，可以百度一下怎么无密码登录（在这里我就不讲了）

因为安装了Yum Repostiory，以后每次yum操作都自动更新，需要把这个卸载掉：

```shell
yum -y remove mysql80-community-release-el7-5.noarch
```

关闭和重启mysql：

```shell
systemctl stop mysqld.service   #关闭mysql
service mysqld restart		    #重启mysql
```

登录root用户并修改密码：

```shell
mysql -uroot -p
```

回车后将刚刚生成的初始密码复制进去，再回车即可

修改密码：

```shell
ALTER USER 'root'@'localhost' IDENTIFIED BY '新密码';
```

如果出现以下错误：是密码安全性未达到要求，你可以先设置比较复杂，你容易记的密码先，因为第一次登陆必须先修改密码才能进行其他操作

```shell
ERROR 1819 (HY000): Your password does not satisfy the current policy requirements
```

登陆之后，可查看MySQL完整的初始密码规则：

```shell
SHOW VARIABLES LIKE 'validate_password%';
```

![](https://img2022.cnblogs.com/blog/2180460/202202/2180460-20220213054141405-1813190606.png)


可以临时修改validate_password.length和validate_password.policy的值，再设置较为简单的密码：

```shell
set global validate_password_length=4;
set global validate_password_policy=0;
```

这样就可以通过上面的修改密码语句将密码修改为较为简单的密码。

### 3.2.6 设置远程连接（Navicat）

```shell
use mysql;                   # 使用mysql数据库
select user,host from user;  # 查看用户及用户登录权限
```

![](https://img2022.cnblogs.com/blog/2180460/202202/2180460-20220213054214925-80621016.png)


上面表示的的四个用户均只能本地登录，不允许远程登录，所以我们需要新增用户，并赋予远程连接权限

**（1）新增远程连接用户：**

```shell
# CREATE USER '用户名'@'主机' IDENTIFIED BY '密码';
CREATE USER 'lzt'@'%' IDENTIFIED BY 'lzt';
```

**（2）设置远程连接权限**

```shell
grant all privileges on *.* to 'lzt'@'%';
```

**（3）刷新权限**

```shell
flush privileges;
```

很多用户在使用Navicat Premium 12（我就是用这个版本，不用破解，即下即用）连接MySQL数据库时会出现Authentication plugin 'caching_sha2_password' cannot be loaded的错误。

出现这个原因是MySQL8.0之前的版本中加密规则是mysql_native_password，而MySQL8.0之后，加密规则是caching_sha2_password,  解决问题方法有两种，一种是升级Navicat驱动（太麻烦），一种是把mysql用户登录密码加密规则还原成mysql_native_password

```shell
# 修改加密规则 
ALTER USER 'root'@'localhost' IDENTIFIED BY 'password' PASSWORD EXPIRE NEVER;

# 更新用户密码 
ALTER USER 'root'@'localhost' IDENTIFIED WITH mysql_native_password BY '密码';
ALTER USER 'lzt'@'%' IDENTIFIED WITH mysql_native_password BY '密码';
```

# 4、项目部署

上面我们已将项目部署所需要的东西都准备好，现在我们只需要将我们的项目进行打包，由于我的项目使用了Maven，我将项目打包成war文件，然后将文件使用XFtp软件放到Linux中/usr/local/tomcat/webapps中，即可通过 **公网IP/端口(默认8080)/资源名** 进行访问：

![](https://img2022.cnblogs.com/blog/2180460/202202/2180460-20220213054318365-741781070.png)


**到此项目部署完成**（不同类型的项目进行部署会有差别，此项目部署较适用于JavaWeb项目、Maven项目）
