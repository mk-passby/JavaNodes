---
title: Shiro简介
date: 2017-09-02 16:44:00
tags: shiro
---

# 1.Shiro简介

Shiro 可以帮助我们完成：认证、授权、加密、会话管理、与Web 集成、缓存等

![](https://static.oschina.net/uploads/space/2017/0902/160620_MqP6_3429289.png)

其中工作原理主要如图，进入后创建一个Subject（即为当前用户），然后SecurityManager管理所有Subject，这里可以理解为于SpringMVC的DispatcherServlet，最后我们Realm相当于是一个数据源，管理用户身份是否合法。

# 2.入门示例

```java
@Test
public void testHelloworld() {
//1、获取SecurityManager工厂，此处使用Ini配置文件初始化SecurityManager
Factory<org.apache.shiro.mgt.SecurityManager> factory =
new IniSecurityManagerFactory("classpath:shiro.ini");
//2、得到SecurityManager实例并绑定给SecurityUtils
org.apache.shiro.mgt.SecurityManager securityManager = factory.getInstance();
SecurityUtils.setSecurityManager(securityManager);
//3、得到Subject及创建用户名/密码身份验证Token（即用户身份/凭证）
Subject subject = SecurityUtils.getSubject();
UsernamePasswordToken token = new UsernamePasswordToken("zhang", "123");
try {
//4、登录，即身份验证
subject.login(token);
} catch (AuthenticationException e) {
//5、身份验证失败
}
Assert.assertEquals(true, subject.isAuthenticated()); //断言用户已经登录
//6、退出
subject.logout();
}
```

2.1、首先通过new IniSecurityManagerFactory 并指定一个ini 配置文件来创建一个SecurityManager工厂；

2.2、接着获取SecurityManager并绑定到SecurityUtils，这是一个全局设置，设置一次即可；  
2.3、通过SecurityUtils得到Subject，其会自动绑定到当前线程；如果在web环境在请求结  
束时需要解除绑定；然后获取身份验证的Token，如用户名/密码；  
2.4、调用subject.login 方法进行登录，其会自动委托给SecurityManager.login方法进行登录；  
2.5、如果身份验证失败请捕获AuthenticationException 或其子类;

2.6、最后可以调用subject.logout退出

# 3.与web集成

```xml
<dependency>
<groupId>org.apache.shiro</groupId>
<artifactId>shiro-web</artifactId>
<version>1.2.2</version>
</dependency>
<dependency>
<groupId>junit</groupId>
<artifactId>junit</artifactId>
<version>4.9</version>
</dependency>
<dependency>
<groupId>commons-logging</groupId>
<artifactId>commons-logging</artifactId>
<version>1.1.3</version>
</dependency>
<dependency>
<groupId>org.apache.shiro</groupId>
<artifactId>shiro-core</artifactId>
<version>1.2.2</version>
</dependency>
```

## 必要的架包。

## web.xml如图所示

```xml
<filter>
		<filter-name>shiroFilter</filter-name>
		<filter-class>org.springframework.web.filter.DelegatingFilterProxy</filter-class>
	</filter>

	<filter-mapping>
		<filter-name>shiroFilter</filter-name>
		<url-pattern>*.shtml</url-pattern>
	</filter-mapping>
```

DelegatingFilterProxy作用是自动到spring容器查找名字为shiroFilter（filter-name）的bean并把所有Filter的操作委托给它，然后将ShiroFilter 配置到spring容器即可

```xml
<bean id="shiroFilter" class="org.apache.shiro.spring.web.ShiroFilterFactoryBean">
		<property name="securityManager" ref="securityManager" />
</bean>
```

## ini配置部分说明

```xml
[main]
#默认是/login.jsp
authc.loginUrl=/login
roles.unauthorizedUrl=/unauthorized
perms.unauthorizedUrl=/unauthorized
[users]
zhang=123,admin
wang=123
[roles]
admin=user:*,menu:*
[urls]
/login=anon
/unauthorized=anon
/static/**=anon
/authenticated=authc
/role=authc,roles[admin]
/permission=authc,perms["user:create"]
```

其中最重要的就是\[urls\]部分的配置，其格式是： “url=拦截器\[参数\]，拦截器\[参数\]”；  
即如果当前请求的url匹配\[urls\]部分的某个url模式，将会执行其配置的拦截器。比如anon  
拦截器表示匿名访问（即不需要登录即可访问）；authc拦截器表示需要身份认证通过后才  
能访问；roles\[admin\]拦截器表示需要有admin 角色授权才能访问；而perms\["user:create"\]  
拦截器表示需要有“user:create”权限才能访问

暂时就看到这里，作一个记录，下次继续更新