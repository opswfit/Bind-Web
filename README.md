

<h1 align = "center">Bind-DLZ + Flask  + Mysql  DNS管理平台 </h1>

系统环境:CentOS 7.4 X64

软件版本: 

      bind-9.9.5.tar.gz  
      mysql-5.6.16.tar.gz
描述： 
数据库安装就不在絮叨，了解运维的同学都应该知道

<h2 align = "center">一．源码安装配置Bind: </h2>

1.yum安装
yum  install  -y bind  mysql-server    mysql
 

	 
![](https://github.com/1032231418/doc/blob/master/images/1.png?raw=true)



4.配置Bind
vim /etc/named.conf 

options {
        listen-on port 53 { 192.168.10.1; };
        directory       "/etc/named";            #zone文件路径
        allow-query     { any; };
        recursion yes;
        dnssec-enable yes;

};

zone  "cdd.group" {
    type  master;
    file  "cdd.group.zone";
            notify yes;
        allow-transfer { 192.168.10.2; };
        also-notify { 192.168.10.2; };
        #allow-update { none; };
};


保存退出


5.配置数据库，导入sql 文件

	mysql -p   #登录数据库
	mysql> CREATE DATABASE  named   CHARACTER SET utf8 COLLATE utf8_general_ci; 
	mysql> source named.sql;             #注意路径，这里我放在当前目录
	就两张表，一个dns用到的表，一个用户管理表

![](https://github.com/1032231418/doc/blob/master/images/2.png?raw=true)


监控系统日志：

	 tail -f /var/log/messages
	 
如下，说明服务启动正常

![](https://github.com/1032231418/doc/blob/master/images/3.png?raw=true)

	测试bind连接数据库是否正常:

![](https://github.com/1032231418/doc/blob/master/images/4.png?raw=true)

查看启动状态
 tail -f /var/log/messages

![](https://github.com/1032231418/doc/blob/master/images/5.png?raw=true)

<h2 align = "center">二．配置Bind-Web 管理平台 </h2>

上传 Bind-web-1.0.tar.gz 管理平台

	(demo) -bash-4.1# git  clone  https://github.com/1032231418/Bind-Web.git  #git  克隆下来
	(demo) -bash-4.1# cd Bind-Web
	(demo) -bash-4.1# python  run.py     

运行软件程序使用flask框架写的，要用pip安装该框架

http://ip/5000   访问WEB 界面 登录账户 eagle 密码 123456

功能有，用户管理，域名管理

![](https://github.com/1032231418/doc/blob/master/images/6.png?raw=true)


![](https://github.com/1032231418/doc/blob/master/images/7.png?raw=true)

![](https://github.com/1032231418/doc/blob/master/images/8.png?raw=true)				

![](https://github.com/1032231418/doc/blob/master/images/jiexi.png?raw=true)

