---
title: nginx+tomcat入门配置
date: 2017-04-21 21:44:00
tags: nginx
---

    此文仅作入门学习，以及记录下配置中遇到的坑。首先nginx+tomcat主要为了实现负载均衡 (分发请求)。为了解释清楚负载均衡，这里假设www.test.com采用这种配置，当我们去访问www.test.com这个网址的时候，请求是传到了nginx服务器，然后由nginx分发到tomcat，假设我们启动了10个tomcat，nginx根据我们的配置分发请求给指定的tomcat，减轻服务器压力。

# 1.工具

nginx-1.8.0

apache-tomcat-6.0.20

apache-tomcat-8.0.30

# 2.Nginx+tomcat配置

解压后在nginx-1.8.0\\conf\\中找到Nginx配置文件nginx.conf进行配置。

```xml
server {
        listen       8080;
        server_name  localhost;
		
		#location / {
        #    proxy_pass http://backend_tomcat;
        #}
        }
```

先找到配置文件中server{}，这里listen 8080监听8080端口

如果启动了nginx，输入http://localhost:8080会看见![](https://static.oschina.net/uploads/space/2017/0421/103109_IUKW_3429289.png)

接下来取消注解location / {  
            proxy\_pass http://backend\_tomcat;  
        }这里标示将请求转发给backend_tomcat（名字随意），然后在http里配置backend_tomcat，与server同一级。

```xml
http{
	upstream backend_tomcat{
		
		server 127.0.0.1:80 weight=1;
		server 127.0.0.1:8888 weight=1;
	}
	
    server {
    }
}
```

这里配置两个server对应两个tomcat，一个80端口的tomcat6，一个8888端口的tomcat8，weight表示访问的权重。

配置好后，启动tomcat6和tomcat8，启动nginx，访问http://localhost:8080一直刷新，会看见在tomcat6和tomcat8界面切换。实现分发请求

# 3.nginx实现静态分离

这里主要说在使用nginx时，由于我们的请求是转发的，所以对于静态的文件无法直接加载，这里需要配置静态分离，即将js，css，image等静态资源放在nginx服务器，jsp，do，action等去分发请求。

操作如下

```xml
server {
        listen       8080;
        server_name  localhost;
		
	
		location ~ .*\.(css|js|gif|jpg|png|bmp|swf)$   #由nginx处理静态页面
        {
			
            expires 10d;   #使用expires缓存模块，缓存到客户端30天
			root   html;
        }
		
		location ~ \.(jsp|action)$ {
			
			proxy_pass http://backend_tomcat;
		}
}
```

这里贴出代码注意，此时需要将第二点处的location /(如下图) 改为location ~ \\.(jsp|action)$(如上图)，第一个表示所有请求，第二个表示拦截以.jsp或者.action结尾的请求

root html表示拦截到的静态文件去html文件夹找，这里的html文件夹表示安装目录\\nginx-1.8.0\\html文件夹，可以在html里放一个test文件夹，放一个图片test.png，启动nginx，输入http://localhost:8080/test/test.png，此时可以访问

```xml
server {
        listen       8080;
        server_name  localhost;
		
		location / {
            proxy_pass http://backend_tomcat;
        }
        }
```

# 4.nignx配置导向项目

        如果按上述配置将请求转发到项目位置，并将项目静态文件放到html文件夹下，此时发现报错，查看文件时发现在服务器的jsp页面中的basePath会被解析为backend_tomcat加上端口+项目名，系统找不到backend\_tomcat，此时需要在添加proxy\_set\_header Host， proxy\_set_header，这里作用及时重写请求头，防止后端服务器处理时认为所有请求都来自反向代理服务器。

```xml
server {
        listen       8080;
        server_name  localhost;
		
		#location / {
        #    proxy_pass http://backend_tomcat;
        #}
		location ~ .*\.(css|js|gif|jpg|png|bmp|swf)$   #由nginx处理静态页面
        {
			proxy_set_header Host  $host; 
			proxy_set_header X-Real-IP $remote_addr; 
			proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for; 
            expires 10d;   #使用expires缓存模块，缓存到客户端30天
			root   filetest;
        }
		
		location ~ \.(jsp|action)$ {
			proxy_set_header Host  $host; 
			proxy_set_header X-Real-IP $remote_addr; 
			proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for; 
			proxy_pass http://backend_tomcat;
		}
```

如图，配置后，重跑ngnix，发现请求地址对了，但是对于nginx中的静态文件未显示端口，加载不出静态文件。此时再加上host中的端口，如下图

```xml
location ~ .*\.(css|js|gif|jpg|png|bmp|swf)$   #由nginx处理静态页面
        {
			proxy_set_header Host  $host:8080; 
			proxy_set_header X-Real-IP $remote_addr; 
			proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for; 
            expires 10d;   #使用expires缓存模块，缓存到客户端30天
			root   filetest;
        }
		
		location ~ \.(jsp|action)$ {
			proxy_set_header Host  $host; 
			proxy_set_header X-Real-IP $remote_addr; 
			proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for; 
			proxy_pass http://backend_tomcat;
```

重新启动nginx，配置完成，正常访问