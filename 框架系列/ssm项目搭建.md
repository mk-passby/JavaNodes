---
title: ssm项目搭建
date: 2018-03-22 16:44:00
tags:
---

# 基于xml配置

## 1.所需架包

```xml
<dependencies>
     
        <dependency>
              <groupId>javax.websocket</groupId>
              <artifactId>javax.websocket-api</artifactId>
              <version>1.1</version>
        </dependency>
        <dependency>
              <groupId>org.springframework</groupId>
              <artifactId>spring-websocket</artifactId>
              <version>4.0.5.RELEASE</version>
         </dependency>
        <dependency>
            <groupId>junit</groupId>
            <artifactId>junit</artifactId>
            <version>3.8.1</version>
            <scope>test</scope>
        </dependency>
 
        <dependency>
            <groupId>jstl</groupId>
            <artifactId>jstl</artifactId>
            <version>1.2</version>
        </dependency>
        <dependency>
            <groupId>com.fasterxml.jackson.core</groupId>
            <artifactId>jackson-databind</artifactId>
            <version>2.2.3</version>
        </dependency>
        <dependency>
            <groupId>com.google.code.gson</groupId>
            <artifactId>gson</artifactId>
            <version>2.2.4</version>
        </dependency>
        <!-- https://mvnrepository.com/artifact/net.sf.ezmorph/ezmorph -->
        <dependency>
            <groupId>net.sf.ezmorph</groupId>
            <artifactId>ezmorph</artifactId>
            <version>1.0.4</version>
        </dependency>
        <!-- https://mvnrepository.com/artifact/commons-beanutils/commons-beanutils -->
        <dependency>
            <groupId>commons-beanutils</groupId>
            <artifactId>commons-beanutils</artifactId>
            <version>1.7.0</version>
        </dependency>
 
        <dependency>
            <groupId>commons-collections</groupId>
            <artifactId>commons-collections</artifactId>
            <version>3.2.2</version>
        </dependency>
        <dependency>
            <groupId>net.sf.json-lib</groupId>
            <artifactId>json-lib-ext-spring</artifactId>
            <version>1.0.2</version>
        </dependency>
        <dependency>
            <groupId>log4j</groupId>
            <artifactId>log4j</artifactId>
            <version>1.2.17</version>
        </dependency>
        <dependency>
            <groupId>commons-lang</groupId>
            <artifactId>commons-lang</artifactId>
            <version>2.6</version>
        </dependency>
        <dependency>
            <groupId>commons-io</groupId>
            <artifactId>commons-io</artifactId>
            <version>2.0.1</version>
        </dependency>
        <dependency>
            <groupId>commons-fileupload</groupId>
            <artifactId>commons-fileupload</artifactId>
            <version>1.2.2</version>
        </dependency>
        <dependency>
            <groupId>org.mybatis</groupId>
            <artifactId>mybatis-spring</artifactId>
            <version>1.2.0</version>
        </dependency>
        <dependency>
            <groupId>commons-dbcp</groupId>
            <artifactId>commons-dbcp</artifactId>
            <version>1.4</version>
        </dependency>
        <dependency>
            <groupId>commons-pool</groupId>
            <artifactId>commons-pool</artifactId>
            <version>1.5.6</version>
        </dependency>
        <dependency>
            <groupId>org.aspectj</groupId>
            <artifactId>aspectjweaver</artifactId>
            <version>1.6.11</version>
        </dependency>
        <dependency>
            <groupId>org.mybatis</groupId>
            <artifactId>mybatis</artifactId>
            <version>3.2.2</version>
        </dependency>
        <dependency>
            <groupId>org.mybatis.generator</groupId>
            <artifactId>mybatis-generator</artifactId>
            <version>1.3.5</version>
            <type>pom</type>
        </dependency>
        <dependency>
            <groupId>org.mybatis.generator</groupId>
            <artifactId>mybatis-generator-core</artifactId>
            <version>1.3.5</version>
        </dependency>
        <dependency>
            <groupId>commons-logging</groupId>
            <artifactId>commons-logging</artifactId>
            <version>1.1.2</version>
        </dependency>
        <dependency>
            <groupId>javax.servlet.jsp</groupId>
            <artifactId>jsp-api</artifactId>
            <version>2.2</version>
        </dependency>
        <dependency>
            <groupId>javax.servlet</groupId>
            <artifactId>servlet-api</artifactId>
            <version>2.5</version>
        </dependency>
        <!-- ============================================== -->
        <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-webmvc</artifactId>
            <version>3.2.2.RELEASE</version>
        </dependency>
        <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-aop</artifactId>
            <version>3.2.2.RELEASE</version>
        </dependency>
        <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-aspects</artifactId>
            <version>3.2.2.RELEASE</version>
        </dependency>
        <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-context</artifactId>
            <version>3.2.2.RELEASE</version>
        </dependency>
        <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-context-support</artifactId>
            <version>3.2.2.RELEASE</version>
        </dependency>
        <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-core</artifactId>
            <version>3.2.2.RELEASE</version>
        </dependency>
        <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-expression</artifactId>
            <version>3.2.2.RELEASE</version>
        </dependency>
        <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-jdbc</artifactId>
            <version>3.2.2.RELEASE</version>
        </dependency>
        <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-tx</artifactId>
            <version>3.2.2.RELEASE</version>
        </dependency>
        <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-web</artifactId>
            <version>3.2.2.RELEASE</version>
        </dependency>
        <!-- ============================================== -->
        <dependency>
            <groupId>org.codehaus.jackson</groupId>
            <artifactId>jackson-core-asl</artifactId>
            <version>1.9.11</version>
        </dependency>
        <dependency>
            <groupId>org.codehaus.jackson</groupId>
            <artifactId>jackson-mapper-asl</artifactId>
            <version>1.9.11</version>
        </dependency>
        <dependency>
            <groupId>net.sf.json-lib</groupId>
            <artifactId>json-lib</artifactId>
            <version>2.3</version>
            <classifier>jdk15</classifier>
        </dependency>
        <dependency>
            <groupId>mysql</groupId>
            <artifactId>mysql-connector-java</artifactId>
            <version>5.1.25</version>
        </dependency>
        <dependency>
            <groupId>junit</groupId>
            <artifactId>junit</artifactId>
            <version>4.12</version>
            <scope>test</scope>
        </dependency>
 
        <dependency>
            <groupId>javax.mail</groupId>
            <artifactId>mail</artifactId>
            <version>1.4.3</version>
        </dependency>
 
       
 
    </dependencies>
```

## 2.框架搭建

### 2.1.html--->springMVC

        将html页面的请求转发到springMVC，即转换servlet到springMVC，在WebRoot下的WEB-INF配置web.xml

```xml
<!-- springmvc前端控制器 -->
    <servlet>
        <servlet-name>springmvc</servlet-name>
        <servlet-class>org.springframework.web.servlet.DispatcherServlet
        </servlet-class>
        <!--
            contextConfigLocation配置springmvc加载的配置文件（配置处理器映射器、适配器等等）
            如果不配置contextConfigLocation，默认加载的是/WEB-INF/servlet名称-serlvet.xml（springmvc-servlet.xml）
        -->
        <init-param>
            <param-name>contextConfigLocation</param-name>
            <param-value>classpath:spring/springmvc.xml</param-value>
        </init-param>
    </servlet>
 
    <servlet-mapping>
        <servlet-name>springmvc</servlet-name>
        <!--
            第一种：*.action，访问以.action结尾 由DispatcherServlet进行解析
            第二种：/，所以访问的地址都由DispatcherServlet进行解析，对于静态文件的解析需要配置不让DispatcherServlet进行解析
            使用此种方式可以实现 RESTful风格的url 第三种：/*，这样配置不对，使用这种配置，最终要转发到一个jsp页面时，
            仍然会由DispatcherServlet解析jsp地址，不能根据jsp页面找到handler，会报错。
        -->
        <url-pattern>*.action</url-pattern>
    </servlet-mapping>
```

处理页面乱码，过滤乱码问题

```xml
<!-- post乱码过虑器 -->
    <filter>
        <filter-name>CharacterEncodingFilter</filter-name>
        <filter-class>org.springframework.web.filter.CharacterEncodingFilter
        </filter-class>
        <init-param>
            <param-name>encoding</param-name>
            <param-value>utf-8</param-value>
        </init-param>
        <init-param>
            <param-name>forceEncoding</param-name>
            <param-value>true</param-value>
        </init-param>
    </filter>
    <filter-mapping>
        <filter-name>CharacterEncodingFilter</filter-name>
        <url-pattern>/*</url-pattern>
    </filter-mapping>
```

在web.xml中加载spring容器

```xml
<!-- 加载spring容器 -->
    <context-param>
        <param-name>contextConfigLocation</param-name>
        <param-value>/WEB-INF/classes/spring/applicationContext-*.xml
        </param-value>
    </context-param>
    <listener>
        <listener-class>org.springframework.web.context.ContextLoaderListener</listener-class>
    </listener>
```

部分errorpage的配置

```xml
<!-- 404 页面不存在错误 -->
    <error-page>
        <error-code>404</error-code>
        <location>/error404.jsp</location>
    </error-page>
 
    <!-- 500 系统错误 -->
    <error-page>
        <error-code>500</error-code>
        <location>/error500.jsp</location>
    </error-page>
 
    <!-- 405 页面不存在错误 -->
    <error-page>
        <error-code>405</error-code>
        <location>/error405.jsp</location>
    </error-page>
 
    <!-- 400 提交数据异常错误 -->
    <error-page>
        <error-code>400</error-code>
        <location>/error400.jsp</location>
    </error-page>
```

至此，web.xml的配置已经完成

### springMVC的处理：

此处转发的请求是到classpath:spring/springmvc.xml中

```xml
<beans xmlns="http://www.springframework.org/schema/beans"
    xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns:mvc="http://www.springframework.org/schema/mvc"
    xmlns:context="http://www.springframework.org/schema/context"
    xmlns:aop="http://www.springframework.org/schema/aop" xmlns:tx="http://www.springframework.org/schema/tx"
    xsi:schemaLocation="http://www.springframework.org/schema/beans 
        http://www.springframework.org/schema/beans/spring-beans-3.2.xsd 
        http://www.springframework.org/schema/mvc 
        http://www.springframework.org/schema/mvc/spring-mvc-3.2.xsd 
        http://www.springframework.org/schema/context 
        http://www.springframework.org/schema/context/spring-context-3.2.xsd 
        http://www.springframework.org/schema/aop 
        http://www.springframework.org/schema/aop/spring-aop-3.2.xsd 
        http://www.springframework.org/schema/tx 
        http://www.springframework.org/schema/tx/spring-tx-3.2.xsd ">
 
    <!-- 对于注解的Handler可以单个配置
    实际开发中建议使用组件扫描
     -->
    <!-- <bean class="cn.itcast.ssm.controller.ItemsController3" /> -->
    <!-- 可以扫描controller、service、...
    这里让扫描controller，指定controller的包
     -->
    <context:component-scan base-package="com.learn.controller"></context:component-scan>
     
     
    <!-- 处理器映射器 将bean的name作为url进行查找 ，需要在配置Handler时指定beanname（就是url） 
    所有的映射器都实现 HandlerMapping接口。
    -->
    <bean
        class="org.springframework.web.servlet.handler.BeanNameUrlHandlerMapping" />
         
    <!--注解映射器 -->
    <!--<bean class="org.springframework.web.servlet.mvc.method.annotation.RequestMappingHandlerMapping"/>-->
    <!--注解适配器 -->
    <!--<bean class="org.springframework.web.servlet.mvc.method.annotation.RequestMappingHandlerAdapter"/>-->
     
    <!-- 使用 mvc:annotation-driven代替上边注解映射器和注解适配器配置
    mvc:annotation-driven默认加载很多的参数绑定方法，
    比如json转换解析器就默认加载了，如果使用mvc:annotation-driven不用配置上边的RequestMappingHandlerMapping和RequestMappingHandlerAdapter
    实际开发时使用mvc:annotation-driven
     -->
    <mvc:annotation-driven conversion-service="conversionService"></mvc:annotation-driven>
         
    <!-- 自定义参数绑定 -->
    <bean id="conversionService"
        class="org.springframework.format.support.FormattingConversionServiceFactoryBean">
        <!-- 转换器 -->
        <property name="converters">
            <list>
                <!-- 日期类型绑定器 -->
                <bean class="com.learn.conv.DateConverter" />
            </list>
        </property>
    </bean>
 
    <!-- 视图解析器
    解析jsp解析，默认使用jstl标签，classpath下的得有jstl的包
     -->
    <bean
        class="org.springframework.web.servlet.view.InternalResourceViewResolver">
        <!-- 配置jsp路径的前缀 -->
        <property name="prefix" value="WEB-INF/jsp/"/>
        <!-- 配置jsp路径的后缀 -->
        <property name="suffix" value=".jsp"/>
    </bean>
    <!-- 自定义的全局异常处理器 
只要实现HandlerExceptionResolver接口就是全局异常处理器-->
    <bean class="com.learn.exception.CustomExceptionResolver"></bean>
     
     <!-- SpringMVC上传文件时，需要配置MultipartResolver处理器 -->  
    <bean id="multipartResolver" class="org.springframework.web.multipart.commons.CommonsMultipartResolver">  
        <!-- 指定所上传文件的总大小不能超过200KB。注意maxUploadSize属性的限制不是针对单个文件，而是所有文件的容量之和 -->  
      <!--    <property name="maxUploadSize" value="5242880"/>  -->
    </bean> 
     
    <!--拦截器 -->
<mvc:interceptors>
    <!-- 登陆认证拦截器 -->
    <mvc:interceptor>
        <mvc:mapping path="/**"/>
        <bean class="com.learn.interceptor.LoginInterceptor"></bean>
    </mvc:interceptor>
</mvc:interceptors>
</beans>
```

同时这里贴出三个类（日期邦定器、登陆拦截器、全部异常处理 ）

```java
package com.learn.exception;
 
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
 
import org.springframework.web.servlet.HandlerExceptionResolver;
import org.springframework.web.servlet.ModelAndView;
 
public class CustomExceptionResolver implements HandlerExceptionResolver{
 
    public ModelAndView resolveException(HttpServletRequest request,
            HttpServletResponse response, Object handler, Exception ex) {
        // TODO Auto-generated method stub
        CustomException customException = null;
        if(ex instanceof CustomException){
            customException = (CustomException)ex;
        }else{
            customException = new CustomException("未知错误");
        }
         
        //错误信息
        String message = customException.getMessage();
         
         
        ModelAndView modelAndView = new ModelAndView();
         
        //将错误信息传到页面
        modelAndView.addObject("message", message);
         
        //指向错误页面
        modelAndView.setViewName("error");
 
         
        return modelAndView;
    }
     
}
```

```java
package com.learn.conv;
import java.text.ParseException;
import java.text.SimpleDateFormat;
import java.util.Date;
import org.springframework.core.convert.converter.Converter;
public class DateConverter implements Converter<String, Date>{
    public Date convert(String souce) {
        System.out.println(souce.length());
        if(souce.length() == 19){
            SimpleDateFormat format = new SimpleDateFormat("yyyy-MM-dd HH:mm:ss");
            try {
                return format.parse(souce);
            } catch (ParseException e) {
                // TODO Auto-generated catch block
                e.printStackTrace();
                return null;
            }          
        }else if(souce.length() == 10){
            SimpleDateFormat format = new SimpleDateFormat("yyyy-MM-dd");
            try {
                return format.parse(souce);
            } catch (ParseException e) {
                // TODO Auto-generated catch block
                e.printStackTrace();
                return null;
            }  
        }else{
            return null;
        }
    }
}
```

```java
package com.learn.interceptor;
 
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
import javax.servlet.http.HttpSession;
 
import org.springframework.web.servlet.HandlerInterceptor;
import org.springframework.web.servlet.ModelAndView;
 
/** 
* @author  
* @version 创建时间：2016年4月26日 下午4:42:38 
*  
*/
public class LoginInterceptor implements HandlerInterceptor{
 
    public void afterCompletion(HttpServletRequest request, HttpServletResponse response, Object arg2, Exception arg3)
            throws Exception {
        // TODO Auto-generated method stub
        System.out.println("HandlerInterceptor1...afterCompletion");
         
    }
 
    public void postHandle(HttpServletRequest request, HttpServletResponse response, Object arg2, ModelAndView arg3)
            throws Exception {
        // TODO Auto-generated method stub
        System.out.println("HandlerInterceptor1...postHandle");
         
    }
 
    public boolean preHandle(HttpServletRequest request, HttpServletResponse response, Object arg2) throws Exception {
        String url=request.getRequestURI();
        //System.out.println("url---------"+url+"--"+url.indexOf("getModule.action"));
    if(url.indexOf("index.action")>=0 ||url.indexOf("toLogin.action")>=0||url.indexOf("msgDetail.action")>=0
            ||url.indexOf("findMsg.action")>=0||url.indexOf("getModule.action")>=0||url.indexOf("getModule2.action")>=0
            ||url.indexOf("getModule3.action")>=0||url.indexOf("findMsg.action")>=0||url.indexOf("indexSkip.action")>=0
            ||url.indexOf("searchType.action")>=0||url.indexOf("searchRSIndex.action")>=0||url.indexOf("getModule.action")>=0     
//             
            )
        {
 
            return true;
        }      
        //判断session
                HttpSession session  = request.getSession();
                //从session中取出用户身份信息
                String usercode = (String) session.getAttribute("userName");               
                if(usercode != null){
                    //身份存在，放行
                    return true;
                }
                 
        //执行这里表示用户身份需要认证，跳转登陆页面
        request.setAttribute("loginFailed", "用户登陆过期!"); 
        request.getRequestDispatcher("index.action").forward(request,response); 
         
        return true;
        //return true;
    }
}
```

#### applicationContext-transaction.xml

```xml
<beans xmlns="http://www.springframework.org/schema/beans"
    xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns:mvc="http://www.springframework.org/schema/mvc"
    xmlns:context="http://www.springframework.org/schema/context"
    xmlns:aop="http://www.springframework.org/schema/aop" xmlns:tx="http://www.springframework.org/schema/tx"
    xsi:schemaLocation="http://www.springframework.org/schema/beans 
        http://www.springframework.org/schema/beans/spring-beans-3.2.xsd 
        http://www.springframework.org/schema/mvc 
        http://www.springframework.org/schema/mvc/spring-mvc-3.2.xsd 
        http://www.springframework.org/schema/context 
        http://www.springframework.org/schema/context/spring-context-3.2.xsd 
        http://www.springframework.org/schema/aop 
        http://www.springframework.org/schema/aop/spring-aop-3.2.xsd 
        http://www.springframework.org/schema/tx 
        http://www.springframework.org/schema/tx/spring-tx-3.2.xsd ">
 
<!-- 事务管理器 
    对mybatis操作数据库事务控制，spring使用jdbc的事务控制类
-->
<bean id="transactionManager" class="org.springframework.jdbc.datasource.DataSourceTransactionManager">
    <!-- 数据源
    dataSource在applicationContext-dao.xml中配置了
     -->
    <property name="dataSource" ref="dataSource"/>
</bean>
 
<!-- 通知 -->
<tx:advice id="txAdvice" transaction-manager="transactionManager">
    <tx:attributes>
        <!-- 传播行为 -->
        <tx:method name="save*" propagation="REQUIRED"/>
        <tx:method name="delete*" propagation="REQUIRED"/>
        <tx:method name="insert*" propagation="REQUIRED"/>
        <tx:method name="update*" propagation="REQUIRED"/>
        <tx:method name="find*" propagation="SUPPORTS" read-only="true"/>
        <tx:method name="get*" propagation="SUPPORTS" read-only="true"/>
        <tx:method name="select*" propagation="SUPPORTS" read-only="true"/>
    </tx:attributes>
</tx:advice>
<!-- aop -->
<aop:config>
    <aop:advisor advice-ref="txAdvice" pointcut="execution(* com.learn.service.Impl.*.*(..))"/>
</aop:config>
 
</beans>
```

#### applicationContext-service.xml

```xml
<beans xmlns="http://www.springframework.org/schema/beans"
    xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns:mvc="http://www.springframework.org/schema/mvc"
    xmlns:context="http://www.springframework.org/schema/context"
    xmlns:aop="http://www.springframework.org/schema/aop" xmlns:tx="http://www.springframework.org/schema/tx"
    xsi:schemaLocation="http://www.springframework.org/schema/beans 
        http://www.springframework.org/schema/beans/spring-beans-3.2.xsd 
        http://www.springframework.org/schema/mvc 
        http://www.springframework.org/schema/mvc/spring-mvc-3.2.xsd 
        http://www.springframework.org/schema/context 
        http://www.springframework.org/schema/context/spring-context-3.2.xsd 
        http://www.springframework.org/schema/aop 
        http://www.springframework.org/schema/aop/spring-aop-3.2.xsd 
        http://www.springframework.org/schema/tx 
        http://www.springframework.org/schema/tx/spring-tx-3.2.xsd ">
         
<context:component-scan base-package="com.learn.service.Impl"></context:component-scan>
<!-- service -->
<bean id="msgService" class="com.learn.service.Impl.MsgServiceImpl"/>
 
</beans>
```

#### applicationContext-dao.xml

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
    xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns:aop="http://www.springframework.org/schema/aop"
    xmlns:context="http://www.springframework.org/schema/context" xmlns:p="http://www.springframework.org/schema/p"
    xsi:schemaLocation="http://www.springframework.org/schema/context http://www.springframework.org/schema/context/spring-context-3.0.xsd          
      http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans-3.0.xsd 
      http://www.springframework.org/schema/aop http://www.springframework.org/schema/aop/spring-aop-3.0.xsd">
     
    <!-- 加载db.properties文件的内容，db.properties文件中的key命名有一定的特殊规则 -->
    <context:property-placeholder location="classpath:db.properties"/>
    <!--配置数据源,dbcp  -->
    <bean id="dataSource" class="org.apache.commons.dbcp.BasicDataSource" destroy-method="close">
                <!-- 数据连接信息 -->
                <property name="driverClassName" value="${jdbc.driver}" />
                <property name="url" value="${jdbc.url}" />
                <property name="username" value="${jdbc.username}" />
                <property name="password" value="${jdbc.password}" />
                <property name="maxActive" value="30" />
                <property name="maxIdle" value="5" />
                 
    </bean>
    <!-- sqlsessionFactory -->
    <bean id="sqlSessionFactory" class="org.mybatis.spring.SqlSessionFactoryBean"> 
                 <property name="dataSource" ref="dataSource"></property>
                 <property name="configLocation" value="classpath:mybatis/sqlMapConfig.xml"></property>
    </bean>
   <!--  <bean id="sqlSessionFactory" class="org.mybatis.spring.SqlSessionFactoryBean"
        p:dataSource-ref="dataSource" p:configLocation="classpath:mybatis/sqlMapConfig.xml" />--> 
    <!-- mapper扫描器 -->
    <bean class="org.mybatis.spring.mapper.MapperScannerConfigurer">
                 <property name="basePackage" value="com.learn.mapper"></property>
                 <property name="SqlSessionFactoryBeanName" value="sqlSessionFactory"></property>
    </bean>
</beans>
```

#### sqlMapConfig.xml

```java
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE configuration PUBLIC "-//mybatis.org//DTD Config 3.0//EN" "http://mybatis.org/dtd/mybatis-3-config.dtd">
<configuration>
   <!-- 全局的setting配置，根据需要添加 -->
   <typeAliases>
     <!-- 批量扫描别名 -->
     <package name="com.learn.po"/>
   </typeAliases>
</configuration>
```

## 逆向生成文件

```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE generatorConfiguration
  PUBLIC "-//mybatis.org//DTD MyBatis Generator Configuration 1.0//EN"
  "http://mybatis.org/dtd/mybatis-generator-config_1_0.dtd">
 
<generatorConfiguration>
    <context id="testTables" targetRuntime="MyBatis3">
        <commentGenerator>
            <!-- 是否去除自动生成的注释 true：是 ： false:否 -->
            <property name="suppressAllComments" value="true" />
        </commentGenerator>
        <!--数据库连接的信息：驱动类、连接地址、用户名、密码 -->
        <jdbcConnection driverClass="com.mysql.jdbc.Driver"
            connectionURL="jdbc:mysql://127.0.0.1:3306/mk_notes" userId="root"
            password="root">
        </jdbcConnection>
        <!-- <jdbcConnection driverClass="oracle.jdbc.OracleDriver"
            connectionURL="jdbc:oracle:thin:@127.0.0.1:1521:yycg" 
            userId="yycg"
            password="yycg">
        </jdbcConnection> -->
 
        <!-- 默认false，把JDBC DECIMAL 和 NUMERIC 类型解析为 Integer，为 true时把JDBC DECIMAL 和 
            NUMERIC 类型解析为java.math.BigDecimal -->
        <javaTypeResolver>
            <property name="forceBigDecimals" value="false" />
        </javaTypeResolver>
 
        <!-- targetProject:生成PO类的位置 -->
        <javaModelGenerator targetPackage="com.learn.po"
            targetProject=".\src">
            <!-- enableSubPackages:是否让schema作为包的后缀 -->
            <property name="enableSubPackages" value="false" />
            <!-- 从数据库返回的值被清理前后的空格 -->
            <property name="trimStrings" value="true" />
        </javaModelGenerator>
        <!-- targetProject:mapper映射文件生成的位置 -->
        <sqlMapGenerator targetPackage="com.learn.mapper" 
            targetProject=".\src">
            <!-- enableSubPackages:是否让schema作为包的后缀 -->
            <property name="enableSubPackages" value="false" />
        </sqlMapGenerator>
        <!-- targetPackage：mapper接口生成的位置 -->
        <javaClientGenerator type="XMLMAPPER"
            targetPackage="com.learn.mapper" 
            targetProject=".\src">
            <!-- enableSubPackages:是否让schema作为包的后缀 -->
            <property name="enableSubPackages" value="false" />
        </javaClientGenerator>
        <!-- 指定数据库表 -->
        <table tableName="op_member_t"></table>
     
    </context>
</generatorConfiguration>
```

### 对应的java代码

```java
package Generator;
import java.io.File;
import java.util.ArrayList;
import java.util.List;
 
import org.mybatis.generator.api.MyBatisGenerator;
import org.mybatis.generator.config.Configuration;
import org.mybatis.generator.config.xml.ConfigurationParser;
import org.mybatis.generator.internal.DefaultShellCallback;
public class GeneratorSqlmap {
    public void generator() throws Exception{
 
        List<String> warnings = new ArrayList<String>();
        boolean overwrite = true;
        //指定 逆向工程配置文件
        File configFile = new File("config/generatorConfig.xml"); 
        ConfigurationParser cp = new ConfigurationParser(warnings);
        Configuration config = cp.parseConfiguration(configFile);
        DefaultShellCallback callback = new DefaultShellCallback(overwrite);
        MyBatisGenerator myBatisGenerator = new MyBatisGenerator(config,
                callback, warnings);
        myBatisGenerator.generate(null);
 
    } 
    public static void main(String[] args) throws Exception {
        try {
            GeneratorSqlmap generatorSqlmap = new GeneratorSqlmap();
            generatorSqlmap.generator();
        } catch (Exception e) {
            e.printStackTrace();
        }
         
    }
}
```

## 整个项目的目录结构

供参考

![](https://static.oschina.net/uploads/space/2017/0616/161712_LaAq_3429289.png)


# 基于javaconfig配置

## 1    项目结构（SSM+MAVEN）

### 1.1    目录结构

![](https://static.oschina.net/uploads/space/2018/0322/155319_tg0n_3429289.png)

### 1.2    POM文件

        由于仓库是nexus私服，如遇见无法加载的包，请无视

```xml
<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/maven-v4_0_0.xsd">
	<modelVersion>4.0.0</modelVersion>
	<groupId>com.test</groupId>
	<artifactId>ssmTest</artifactId>
	<packaging>war</packaging>
	<version>0.0.1-SNAPSHOT</version>
	<name>ssmTest Maven Webapp</name>
	<url>http://maven.apache.org</url>
	<dependencies>
		<dependency>
			<groupId>junit</groupId>
			<artifactId>junit</artifactId>
			<version>3.8.1</version>
			<scope>test</scope>
		</dependency>

		<dependency>
			<groupId>org.apache.xmlbeans</groupId>
			<artifactId>xmlbeans</artifactId>
			<version>2.6.0</version>
		</dependency>

		<dependency>
			<groupId>org.apache.poi</groupId>
			<artifactId>poi-ooxml-schemas</artifactId>
			<version>3.16</version>
		</dependency>

		<dependency>
			<groupId>org.apache.poi</groupId>
			<artifactId>poi-ooxml</artifactId>
			<version>3.16</version>
		</dependency>

		<dependency>
			<groupId>org.apache.poi</groupId>
			<artifactId>poi</artifactId>
			<version>3.16</version>
		</dependency>

		<dependency>
			<groupId>org.apache.commons</groupId>
			<artifactId>commons-collections4</artifactId>
			<version>4.1</version>
		</dependency>

		<dependency>
			<groupId>javax.websocket</groupId>
			<artifactId>javax.websocket-api</artifactId>
			<version>1.1</version>
		</dependency>

		<dependency>
			<groupId>org.springframework</groupId>
			<artifactId>spring-websocket</artifactId>
			<version>4.0.5.RELEASE</version>
		</dependency>

		<dependency>
			<groupId>jstl</groupId>
			<artifactId>jstl</artifactId>
			<version>1.2</version>
		</dependency>

		<dependency>
			<groupId>com.fasterxml.jackson.core</groupId>
			<artifactId>jackson-databind</artifactId>
			<version>2.2.3</version>
		</dependency>

		<dependency>
			<groupId>com.google.code.gson</groupId>
			<artifactId>gson</artifactId>
			<version>2.2.4</version>
		</dependency>

		<dependency>
			<groupId>net.sf.ezmorph</groupId>
			<artifactId>ezmorph</artifactId>
			<version>1.0.4</version>
		</dependency>

		<dependency>
			<groupId>jcifs</groupId>
			<artifactId>jcifs</artifactId>
			<version>1.3.17</version>
		</dependency>

		<dependency>
			<groupId>commons-beanutils</groupId>
			<artifactId>commons-beanutils</artifactId>
			<version>1.7.0</version>
		</dependency>

		<dependency>
			<groupId>commons-collections</groupId>
			<artifactId>commons-collections</artifactId>
			<version>3.2.2</version>
		</dependency>

		<dependency>
			<groupId>net.sf.json-lib</groupId>
			<artifactId>json-lib-ext-spring</artifactId>
			<version>1.0.2</version>
		</dependency>

		<dependency>
			<groupId>log4j</groupId>
			<artifactId>log4j</artifactId>
			<version>1.2.17</version>
		</dependency>

		<dependency>
			<groupId>commons-lang</groupId>
			<artifactId>commons-lang</artifactId>
			<version>2.6</version>
		</dependency>

		<dependency>
			<groupId>commons-io</groupId>
			<artifactId>commons-io</artifactId>
			<version>2.0.1</version>
		</dependency>

		<dependency>
			<groupId>commons-fileupload</groupId>
			<artifactId>commons-fileupload</artifactId>
			<version>1.2.2</version>
		</dependency>

		<dependency>
			<groupId>org.mybatis</groupId>
			<artifactId>mybatis-spring</artifactId>
			<version>1.2.0</version>
		</dependency>

		<dependency>
			<groupId>commons-dbcp</groupId>
			<artifactId>commons-dbcp</artifactId>
			<version>1.4</version>
		</dependency>

		<dependency>
			<groupId>commons-pool</groupId>
			<artifactId>commons-pool</artifactId>
			<version>1.5.6</version>
		</dependency>

		<dependency>
			<groupId>org.aspectj</groupId>
			<artifactId>aspectjweaver</artifactId>
			<version>1.6.11</version>
		</dependency>

		<dependency>
			<groupId>org.mybatis</groupId>
			<artifactId>mybatis</artifactId>
			<version>3.2.2</version>
		</dependency>

		<dependency>
			<groupId>org.mybatis.generator</groupId>
			<artifactId>mybatis-generator</artifactId>
			<version>1.3.5</version>
			<type>pom</type>
		</dependency>

		<dependency>
			<groupId>org.mybatis.generator</groupId>
			<artifactId>mybatis-generator-core</artifactId>
			<version>1.3.5</version>
		</dependency>

		<dependency>
			<groupId>commons-logging</groupId>
			<artifactId>commons-logging</artifactId>
			<version>1.1.2</version>
		</dependency>

		<dependency>
			<groupId>javax.servlet.jsp</groupId>
			<artifactId>jsp-api</artifactId>
			<version>2.2</version>
		</dependency>
		<dependency>
			<groupId>javax.servlet</groupId>
			<artifactId>servlet-api</artifactId>
			<version>2.5</version>
		</dependency>

		<dependency>
			<groupId>aopalliance</groupId>
			<artifactId>aopalliance</artifactId>
			<version>1.0</version>
		</dependency>

		<dependency>
			<groupId>org.springframework</groupId>
			<artifactId>spring-webmvc</artifactId>
			<version>4.2.5.RELEASE</version>
		</dependency>

		<dependency>
			<groupId>org.springframework</groupId>
			<artifactId>spring-aop</artifactId>
			<version>4.2.5.RELEASE</version>
		</dependency>

		<dependency>
			<groupId>org.springframework</groupId>
			<artifactId>spring-aspects</artifactId>
			<version>4.2.5.RELEASE</version>
		</dependency>

		<dependency>
			<groupId>org.springframework</groupId>
			<artifactId>spring-context</artifactId>
			<version>4.2.5.RELEASE</version>
		</dependency>

		<dependency>
			<groupId>org.springframework</groupId>
			<artifactId>spring-beans</artifactId>
			<version>4.2.5.RELEASE</version>
		</dependency>

		<dependency>
			<groupId>org.springframework</groupId>
			<artifactId>spring-context-support</artifactId>
			<version>4.2.5.RELEASE</version>
		</dependency>

		<dependency>
			<groupId>org.springframework</groupId>
			<artifactId>spring-core</artifactId>
			<version>4.2.5.RELEASE</version>
		</dependency>

		<dependency>
			<groupId>org.springframework</groupId>
			<artifactId>spring-expression</artifactId>
			<version>4.2.5.RELEASE</version>
		</dependency>

		<dependency>
			<groupId>org.springframework</groupId>
			<artifactId>spring-jdbc</artifactId>
			<version>4.2.5.RELEASE</version>
		</dependency>

		<dependency>
			<groupId>org.springframework</groupId>
			<artifactId>spring-tx</artifactId>
			<version>4.2.5.RELEASE</version>
		</dependency>
		<dependency>
			<groupId>org.springframework</groupId>
			<artifactId>spring-test</artifactId>
			<version>4.2.5.RELEASE</version>
		</dependency>
		<dependency>
			<groupId>org.springframework</groupId>
			<artifactId>spring-web</artifactId>
			<version>4.2.5.RELEASE</version>
		</dependency>

		<dependency>
			<groupId>org.apache.commons</groupId>
			<artifactId>commons-pool2</artifactId>
			<version>2.2</version>
		</dependency>

		<dependency>
			<groupId>org.codehaus.jackson</groupId>
			<artifactId>jackson-core-asl</artifactId>
			<version>1.9.11</version>
		</dependency>

		<dependency>
			<groupId>org.codehaus.jackson</groupId>
			<artifactId>jackson-mapper-asl</artifactId>
			<version>1.9.11</version>
		</dependency>

		<dependency>
			<groupId>net.sf.json-lib</groupId>
			<artifactId>json-lib</artifactId>
			<version>2.3</version>
			<classifier>jdk15</classifier>
		</dependency>

		<dependency>
			<groupId>mysql</groupId>
			<artifactId>mysql-connector-java</artifactId>
			<version>5.1.20</version>
		</dependency>

		<dependency>
			<groupId>javax.mail</groupId>
			<artifactId>mail</artifactId>
			<version>1.4.3</version>
		</dependency>

		<dependency>
			<groupId>commons-net</groupId>
			<artifactId>commons-net</artifactId>
			<version>3.3</version>
		</dependency>

		<dependency>
			<groupId>commons-codec</groupId>
			<artifactId>commons-codec</artifactId>
			<version>1.10</version>
		</dependency>
	</dependencies>
	<build>
		<finalName>ssmTest</finalName>
	</build>
</project>

```

## 2    配置SPRINGMVC

### 2.1    初始化

         首先创建一个初始化类，继承AbstractAnnotationConfigDispatcherServletInitializer类

```java
package com.ssm.config;

import org.apache.log4j.Logger;
import org.springframework.web.servlet.support.AbstractAnnotationConfigDispatcherServletInitializer;

/**
 * 
 * @author C
 *
 */
public class WebAppInitializer extends AbstractAnnotationConfigDispatcherServletInitializer {
	private final static Logger LOG = Logger.getLogger(WebAppInitializer.class);

	@Override
	protected Class<?>[] getRootConfigClasses() {
		// TODO Auto-generated method stub
		LOG.info("------root配置类初始化------");
		return new Class<?>[] { RootConfig.class };
	}

	@Override
	protected Class<?>[] getServletConfigClasses() {
		LOG.info("------web配置类初始化------");
		 return new Class<?>[] { WebConfig.class };
	}

	@Override
	protected String[] getServletMappings() {
		// TODO Auto-generated method stub
		LOG.info("------映射根路径初始化------");
		//return null;
		 return new String[] {"/"};// 请求路径映射，将路径映射到DispatcherServlet上
	}

}

```

这里继承AbstractAnnotationConfigDispatcherServletInitializer 类，就会自动个地配置Dispatcher-Servlet和Spring上下文(传统的方法是在web.xml中配置 DispatcherServlet)

### 2.2    AbstractAnnotationConfigDispatcherServletInitializer剖析

         Servlet3.0环境中，容器会在类路径去查找实现javax.servlet.ServletContainerInitializer接口的类，如果发现，就用它做servlet的容器
    
        Spring对这个接口进行了实现，为SpringServletContainerInitializer。它( SpringServletContainerInitializer )会去查找实现了WebAppInitializer的类并将配置任务交给他们来完成。Spring3.2引入了便利的 WebAppInitializer 实现，就是AbstractAnnotationConfigDispatcherServletInitializer。所以当部署到Servlet3.0容器中时，容器会自动发现它，并配置servlet上下文
    
         重新的三个方法也有对应注解，这里不再多说

### 2.3  WebConfig配置

        当DispatcherServlet启动的时候，会创建Spring应用上下文并加载配置文件或配置文件中声明的bean。
    
        当它加载上下文时，使用定义在WebConfig中的bean(基于java配置)
    
        DispatcherServlet 加载包含web组件的bean，如控制器，视图解析器等。还有一个ContextLoaderListener加载其他bean，如中间层及数据层组件等

```java
package com.ssm.config;

import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.ComponentScan;
import org.springframework.context.annotation.Configuration;
import org.springframework.web.multipart.commons.CommonsMultipartResolver;
import org.springframework.web.servlet.ViewResolver;
import org.springframework.web.servlet.config.annotation.DefaultServletHandlerConfigurer;
import org.springframework.web.servlet.config.annotation.EnableWebMvc;
import org.springframework.web.servlet.config.annotation.WebMvcConfigurerAdapter;
import org.springframework.web.servlet.view.InternalResourceViewResolver;

/****
 * 定义 DispatcherServlet 加载应用上下文的配置
 * 
 * @author C
 *
 */
@Configuration
@EnableWebMvc
@ComponentScan("com.ssm.controller") // 包扫描
public class WebConfig extends WebMvcConfigurerAdapter{
	@Bean
	public ViewResolver viewResolver() {
		InternalResourceViewResolver resolver = new InternalResourceViewResolver();
		resolver.setPrefix("/WEB-INF/jsp");
		resolver.setSuffix(".jsp");
		return resolver;
	}

	@Bean(name = "multipartResolver") // bean必须写name属性且必须为multipartResolver
	protected CommonsMultipartResolver multipartResolver() {
		CommonsMultipartResolver commonsMultipartResolver = new CommonsMultipartResolver();
		commonsMultipartResolver.setMaxUploadSize(5 * 1024 * 1024);
		commonsMultipartResolver.setMaxInMemorySize(0);
		commonsMultipartResolver.setDefaultEncoding("UTF-8");
		return commonsMultipartResolver;
	}

	// 静态资源的处理
	public void configureDefaultServletHandling(DefaultServletHandlerConfigurer configurer) {
		configurer.enable();
	}
}

```

@Configuration：配置类

@EnableWebMvc：相当于基于xml配置的<mvc:annotation-driven>启用注解驱动

@ComponentScan：包扫描

## 3    RootConfig配置

```java
package com.ssm.config;

import org.springframework.aop.framework.autoproxy.BeanNameAutoProxyCreator;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.ComponentScan;
import org.springframework.context.annotation.Configuration;
import org.springframework.context.annotation.Import;

/***
 * 主要配置持久层的一些东西，包括数据库、Mybatis框架，事务之类的
 * 
 * @author C
 *
 */
@Configuration
@ComponentScan(basePackages = { "com.ssm.config", "com.ssm.service.impl" })
@Import(DataSourceConfig.class)
public class RootConfig {
	@Bean
	public BeanNameAutoProxyCreator autoProxyCreator() {
		BeanNameAutoProxyCreator autoProxyCreator = new BeanNameAutoProxyCreator();
		autoProxyCreator.setProxyTargetClass(true);
		// 设置要创建代理的那些Bean的名字
		autoProxyCreator.setBeanNames("*Service");
		autoProxyCreator.setInterceptorNames("transactionInterceptor");
		return autoProxyCreator;
	}

}

```

这里数据库的事务配置方式有三种：

-   第一种在 RootConfig加上 @EnableTransactionManagement 注解，配置数据库， 手动加上事务使用 [@Transactional](https://my.oschina.net/u/3770144) 注解，并且指定的传播属性，缺点麻烦
-   第二种使用 BeanNameAutoProxyCreator拦截代理方式
-   第三种是采用aop切面事务

## 4    DataSourceConfig

        这里由于我用的1.7的JDK，@PropertySources会报错，作为学习，就直接将properties文件写到类里面了

```java
package com.ssm.config;

import java.io.IOException;
import java.util.Properties;

import org.apache.commons.dbcp.BasicDataSource;
import org.apache.log4j.Logger;
import org.mybatis.spring.SqlSessionFactoryBean;
import org.mybatis.spring.annotation.MapperScan;
import org.springframework.beans.factory.annotation.Value;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;
import org.springframework.context.annotation.PropertySource;
import org.springframework.context.annotation.PropertySources;
import org.springframework.core.io.support.PathMatchingResourcePatternResolver;
import org.springframework.core.io.support.ResourcePatternResolver;
import org.springframework.jdbc.datasource.DataSourceTransactionManager;
import org.springframework.transaction.interceptor.TransactionInterceptor;

@Configuration
/*@PropertySources(value = { @PropertySource("classpath:db.properties") }) */
@MapperScan("com.ssm.mapper")
public class DataSourceConfig {
	private final static Logger LOG = Logger.getLogger(DataSourceConfig.class);

	private String driver = "com.mysql.jdbc.Driver";;

	private String url = "jdbc:mysql://127.0.0.1:3306/test?characterEncoding=UTF-8&zeroDateTimeBehavior=convertToNull&transformedBitIsBoolean=true&autoReconnect=true";

	private String username = "root";

	private String password = "root";

	@Bean
	public BasicDataSource dataSource() {
		LOG.info("Initialize the BasicDataSource...");
		BasicDataSource dataSource = new BasicDataSource();
		dataSource.setDriverClassName(driver);
		dataSource.setUrl(url);
		dataSource.setUsername(username);
		dataSource.setPassword(password);
		return dataSource;
	}

	// mybatis的配置
	@Bean
	public SqlSessionFactoryBean sqlSessionFactoryBean() throws IOException {
		ResourcePatternResolver resourcePatternResolver = new PathMatchingResourcePatternResolver();
		SqlSessionFactoryBean sqlSessionFactoryBean = new SqlSessionFactoryBean();
		sqlSessionFactoryBean.setDataSource(dataSource());
		sqlSessionFactoryBean.setMapperLocations(resourcePatternResolver.getResources("classpath*:mappers/*.xml"));
		sqlSessionFactoryBean.setTypeAliasesPackage("com.ssm.mapper");// 别名，让*Mpper.xml实体类映射可以不加上具体包名
		return sqlSessionFactoryBean;
	}

	// 事务管理器 对mybatis操作数据库事务控制，spring使用jdbc的事务控制类
	@Bean(name = "transactionManager")
	public DataSourceTransactionManager dataSourceTransactionManager() {
		DataSourceTransactionManager dataSourceTransactionManager = new DataSourceTransactionManager();
		dataSourceTransactionManager.setDataSource(dataSource());
		return dataSourceTransactionManager;
	}

	@Bean(name = "transactionInterceptor")
	public TransactionInterceptor interceptor() {
		TransactionInterceptor interceptor = new TransactionInterceptor();
		interceptor.setTransactionManager(dataSourceTransactionManager());
		Properties transactionAttributes = new Properties();
		transactionAttributes.setProperty("save*", "PROPAGATION_REQUIRED");
		transactionAttributes.setProperty("del*", "PROPAGATION_REQUIRED");
		transactionAttributes.setProperty("update*", "PROPAGATION_REQUIRED");
		transactionAttributes.setProperty("get*", "PROPAGATION_REQUIRED,readOnly");
		transactionAttributes.setProperty("find*", "PROPAGATION_REQUIRED,readOnly");
		transactionAttributes.setProperty("*", "PROPAGATION_REQUIRED");
		interceptor.setTransactionAttributes(transactionAttributes);
		return interceptor;
	}
}

```

## 5    测试

```java
package com.ssm.controller;

import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.RestController;

import com.ssm.service.IndexService;
import com.sun.mail.handlers.image_gif;

@RestController
public class IndexController {
	@Autowired
	private IndexService indexService;

	@RequestMapping("index")
	public String index() {
		return "THIS IS A TEST.WELCOME";
	}

	@RequestMapping("getData")
	public String getData() {
		return indexService.findUser();
	}

}

```

### 5.1访问servlet

![](https://static.oschina.net/uploads/space/2018/0322/165245_2jbO_3429289.png)

### 5.2访问数据库

![](https://static.oschina.net/uploads/space/2018/0322/165508_siNA_3429289.png)![](https://static.oschina.net/uploads/space/2018/0322/165413_GwmS_3429289.png)