[TOC]

## 一. 官网教程

[官网的教程](https://wiki.duraspace.org/display/ISLANDORA/milestone+1+-+Installing+Fedora),里面都是有,纯粹加上了翻译

## 二. 安装java

### 命令行安装

因为是ubuntu的可以直接安装openjdk8,只需要命令：

```
sudo apt install openjdk-8-jre-headless
sudo apt install openjdk-8-jdk-headless
```

> 两条命令即可,第一条是安装jre的就是java的运行环境
>
> ,第二条是安装jdk就是java的编译和打包环境

### 运行命令检验安装成功

![](https://ws1.sinaimg.cn/large/891f7782gy1fy9llkvq6xj20e706jmxe.jpg)

![1545018599660](C:\Users\bruce\AppData\Roaming\Typora\typora-user-images\1545018599660.png)

### 安装完成之后文件的位置

> 这一步会影响到我们下面fedora中安装环境变量的设定

![](https://ws1.sinaimg.cn/large/891f7782gy1fy9l0evzhpj205s01tjr6.jpg)

这里显示java和javac的位置都在/usr/bin/的目录中,但是我们继续看进去

![](https://ws1.sinaimg.cn/large/891f7782gy1fy9l2env0xj20so04i74u.jpg)

从这里的话我们就可以确定,其实我们java和javac的位置是安装在别的目录,而/usr/bin/中的程序只是一个链接(类似我们所熟悉的快捷方式)

我们进去这个目录看看

![](https://ws1.sinaimg.cn/large/891f7782gy1fy9l5b9w0aj20lt021mx5.jpg)

这个目录里面的内容其实就是我们在官网里面下载的jdk压缩包解压出来的内容.

## 三. 安装fedora的步骤

### 安装前提

1. jdk1.8
2. mysql(这个得显安装好)

### 设置环境变量

我是安装官网上面的直接复制的,因为我们那些文件放置的目录刚好就和他的相同,比如说java的位置(如上)

环境变量的设置可以添加在/etc/profile文件末尾,也可以添加到~/.bashrc的文件末尾

```
PATH=/opt/java/bin:$PATH:$HOME/bin # 其中这一句的话,其实是有些问题的,因为并没有/opt/java这个目录
export FEDORA_HOME=/usr/local/fedora
export CATALINA_HOME=/usr/local/fedora/tomcat
LD_LIBRARY_PATH=$LD_LIBRARY_PATH:$CATALINA_HOME/lib
export LD_LIBRARY_PATH
export JAVA_OPTS="-Xms1024m -Xmx1024m -Djavax.net.ssl.trustStore=/usr/local/fedora/server/truststore -Djavax.net.ssl.trustStorePassword=tomcat"
export JAVA_HOME=/usr/lib/jvm/java-8-openjdk-amd64
export JRE_HOME=/usr/lib/jvm/java-8-openjdk-amd64/jre
export J2SDKDIR=/usr/lib/jvm/java-8-openjdk-amd64
export J2REDIR=/usr/lib/jvm/java-8-openjdk-amd64/jre
export KAKADU_LIBRARY_PATH=/usr/local/djatoka/lib/Linux-x86-64
```

> 官网里面有解析各个环境变量的意思:https://wiki.duraspace.org/display/FEDORA38/Installation+and+Configuration

### 设置mysql

[**官网教程]**(https://wiki.duraspace.org/display/ISLANDORA/Setting+up+a+MySQL+Database+for+Fedora)

Please note that the MySQL JDBC driver provided by the installer requires MySQL v3.23.x or higher.

在终端输入命令,然后输入密码进入mysql

```
mysql -u root -p
```

创建一个fedora3的数据库

```
CREATE DATABASE fedora3;
```

设置用户名,密码和权限.比如把fedora3的所有权限都设置给fedoraAdmin用户,密码也设置为fedoraAdmin

```
GRANT ALL ON fedora3.* TO fedoraAdmin@localhost IDENTIFIED BY 'fedoraAdmin';
GRANT ALL ON fedora3.* TO fedoraAdmin@'%' IDENTIFIED BY 'fedoraAdmin';
```

设置fedora3数据库的默认字符

```
ALTER DATABASE fedora3 DEFAULT CHARACTER SET utf8;
ALTER DATABASE fedora3 DEFAULT COLLATE utf8_bin;
```

### 运行安装包

#### 下载安装包

[官网可以下载fedora3.8.1的安装包](https://wiki.duraspace.org/display/FEDORA38/Installation+and+Configuration)

### 进入到下载的目录中运行安装包

```
java -jar ./fcrepo-installer-3.8.1.jar
# 切记要选择custom安装
```

**接着显示安装的一系列提示(切记要选择custom安装),可以按照以下的选项来选择**

```
Installation type - custom
home directory - /usr/local/fedora (default)
Password - [fedora_password]
server host - localhost (default) [could be a domain name etc depending on your environment]
app server context - (default)
API-A - false (default)
ssl avail - true
ssl required for api-a - false (default)
ssl required for api-m - false
servlet included - included (default)
tomcat home -(default)
tomcat http port - 8080 (default)
tomcat shutdown - 8005 (default)
tomcat ssl - 8443 (default)
keystore file - included
databse - mysql
MySQL JDBC driver - (default)
database username - fedoraAdmin
database password - [password]
jdbc url - (default)
JDBC DriverClass - (default)
Use upstream HTTP authentication - false
Enable FESL authz - false
policy enforcement - true
low level storage - akubra-fs (default)
Enable Resource Index - true
Enable Messaging - true
Messaging Provider URI - (default)
deploy local services - true
```

各个选项的解释[官网](https://wiki.duraspace.org/display/ISLANDORA/milestone+1+-+Installing+Fedora)上面也是有的,可自行查看

![](https://ws1.sinaimg.cn/large/891f7782gy1fy9lujfxpgj20sw0dbdhx.jpg)

> 如果不想手动设置安装的话,可以建立脚本自动安装,效果也是一样的,详情看官网的如下章节
>
> ![](https://ws1.sinaimg.cn/large/891f7782gy1fy9lwtwrsfj20tx09swff.jpg)

到这里的话,安装fedora就完成了,我们可以进行测试:

> 1. 先运行tomcat
>
> ```
> # $FEDORA_HOME/tomcat/bin/startup.sh
>  
> Using CATALINA_BASE:   /usr/local/fedora/tomcat
> Using CATALINA_HOME:   /usr/local/fedora/tomcat
> Using CATALINA_TMPDIR: /usr/local/fedora/tomcat/temp
> Using JRE_HOME:        /usr
> Using CLASSPATH:       /usr/local/fedora/tomcat/bin/bootstrap.jar
> ```
>
> 2. 查看$FEDORA_HOME/tomcat/logs/catalina.out这个文件中是否有错误的信息
>
>    ![](https://ws1.sinaimg.cn/large/891f7782gy1fy9m0do101j20ih05yq3k.jpg)
>
>    可看出并没有类似ERROR的错误信息
>
> 3. 接着用浏览器登录http://localhost:8080/fedora/或者 https://[yourdomain]:8443/fedora/就可以显示如下的页面
>
>    因为我用的是阿里云的服务器,所以用的是第二个地址,效果是一样的,如果是本机上面登录的话就用第一个
>
>    ![](https://ws1.sinaimg.cn/large/891f7782gy1fy9m3oj8vgj20mp0gntab.jpg)
>
> 4. 访问 <http://localhost:8080/fedora/admin> 可新增和删除对象,如下图
>
>    ![](https://ws1.sinaimg.cn/large/891f7782gy1fy9m5ol1tqj210a0dhdh0.jpg)
>
>    这样就说明了fedora已经安装完成了

## 四. 设置XACML策略

> 1. 关闭Tomcat
>
>    ```
>    $FEDORA_HOME/tomcat/bin/shutdown.sh
>    ```
>
> 2. 删除deny-purge策略
>
>    ```
>    rm -v /usr/local/fedora/data/fedora-xacml-policies/repository-policies/default/deny-purge-*
>    ```
>
> 3. 删除anonymous-user策略
>
>    ```
>    rm -v /usr/local/fedora/data/fedora-xacml-policies/repository-policies/islandora/permit-apim-to-anonymous-user.xml
>    rm -v /usr/local/fedora/data/fedora-xacml-policies/repository-policies/islandora/permit-upload-to-anonymous-user.xml
>    ```
>
> 4. 进入islandora策略的目录
>
>    ```
>    cd /usr/local/fedora/data/fedora-xacml-policies/repository-policies/
>    ```
>
> 5. 从github中下载islandora的策略
>
>    ```
>    git clone https://github.com/Islandora/islandora-xacml-policies.git islandora
>    ```
>
> 6. 最后策略目录的文件如下
>
>    ![](https://ws1.sinaimg.cn/large/891f7782gy1fy9mbg7uf0j20ke0c3wfc.jpg)

