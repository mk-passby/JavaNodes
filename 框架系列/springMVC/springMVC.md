---
title: springmvc分析
date: 2020-03-27 16:40:16
tags: springcloud
---

# 官网文档

https://docs.spring.io/spring/docs/5.0.0.RELEASE/spring-framework-reference/web.html

spring所有官方文档均可以通过此方式找到

# 架构图

![1587994841538](E:\developProject\hexo-aircloud-blog\source\_posts\java框架\springcloud\springMVC\springMVC\springMVC架构图1)



- 由图中可以看到，DispatcherServlet中又分了Servlet WebApplicationContext和Root WebApplicationContext，且Servlet WebApplicationContext属于上层，if no bean found，才去找root。

  - **Root WebApplicationContext**对应ContextLoaderListener(extends ServletContextListener)创建，对应**spring应用**

  **源码ContextLoaderListener.java**

  ```java
  
  public void contextInitialized(ServletContextEvent event) {
          this.initWebApplicationContext(event.getServletContext());
      }
  ```

  ```java
   public WebApplicationContext initWebApplicationContext(ServletContext servletContext) {
          if (servletContext.getAttribute(WebApplicationContext.ROOT_WEB_APPLICATION_CONTEXT_ATTRIBUTE) != null) {
              throw new IllegalStateException("Cannot initialize context because there is already a root application context present - check whether you have multiple ContextLoader* definitions in your web.xml!");
          } else {
              .....
          }
      }
  ```

  

  - **Servlet WebApplicationContext**对应为DispatcherServlet，对应**springmvc应用**

    源码如下RequestContextUtils.java

    ```java
    public static WebApplicationContext findWebApplicationContext(
    			HttpServletRequest request, @Nullable ServletContext servletContext) {
    
    		WebApplicationContext webApplicationContext = (WebApplicationContext) request.getAttribute(
    				DispatcherServlet.WEB_APPLICATION_CONTEXT_ATTRIBUTE);
    		if (webApplicationContext == null) {
    			if (servletContext != null) {
    				webApplicationContext = WebApplicationContextUtils.getWebApplicationContext(servletContext);
    			}
    			if (webApplicationContext == null) {
    				webApplicationContext = ContextLoader.getCurrentWebApplicationContext();
    			}
    		}
    		return webApplicationContext;
    	}
    ```

    



![](springMVC\springMVC架构图)



## 关键类

org.springframework.web.servlet.DispatcherServlet类关系图

![sss](springMVC\DispatcherServlet.png)



org.springframework.boot.autoconfigure.web.servlet.DispatcherServletAutoConfiguration自动装配

org.springframework.boot.autoconfigure.web.servlet.WebMvcProperties配置类(springboot中可以通过这些参数设置springpropoties文件中)





### 基于javaconfig

Below is an example of the Java configuration that registers and initializes the `DispatcherServlet`. This class is auto-detected by the Servlet container (see [Code-based, Servlet container initialization](https://docs.spring.io/spring/docs/5.0.0.RELEASE/spring-framework-reference/web.html#mvc-container-config)):

```
public class MyWebApplicationInitializer implements WebApplicationInitializer {

  @Override
  public void onStartup(ServletContext servletCxt) {

    // Load Spring web application configuration
    AnnotationConfigWebApplicationContext cxt = new AnnotationConfigWebApplicationContext();
    cxt.register(AppConfig.class);
    cxt.refresh();

    // Create DispatcherServlet
    DispatcherServlet servlet = new DispatcherServlet(cxt);

    // Register and map the Servlet
    ServletRegistration.Dynamic registration = servletCxt.addServlet("app", servlet);
    registration.setLoadOnStartup(1);
    registration.addMapping("/app/*");
  }

}
```

### 基于web.xml

Below is an example of `web.xml` configuration to register and initialize the `DispatcherServlet`:

```
<web-app>

  <listener>
    <listener-class>org.springframework.web.context.ContextLoaderListener</listener-class>
  </listener>

  <context-param>
    <param-name>contextConfigLocation</param-name>
    <param-value>/WEB-INF/app-context.xml</param-value>
  </context-param>

  <servlet>
    <servlet-name>app</servlet-name>
    <servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class>
    <init-param>
      <param-name>contextConfigLocation</param-name>
      <param-value></param-value>
    </init-param>
    <load-on-startup>1</load-on-startup>
  </servlet>

  <servlet-mapping>
    <servlet-name>app</servlet-name>
    <url-pattern>/app/*</url-pattern>
  </servlet-mapping>

</web-app>
```





## 整体流程

request--》Handler--》结果--》返回--》文本

根据请求的 URL，寻找匹配的Handler，处理数据，返回结果

RequestUrl=servletContext+requestMapping

#### 请求处理映射

RequestMappingHandlerMapping.java类处理

```java
Map<RequestMappingInfo, HandlerMethod> handlerMethods =requestMappingHandlerMapping.getHandlerMethods();
```

这个方法可以获取所有类中被@RequestMapping标注过的方法的对象

#### 拦截器

- 实现HandlerInterceptor接口

- 继承`HandlerInterceptorAdapter` 

spring官网如下解释，更推荐使用`HandlerInterceptorAdapter` 

```
As you can see, the Spring adapter class HandlerInterceptorAdapter makes it easier to extend the HandlerInterceptor interface.
```



 This is shown in the example below:

```xml
<beans>
        <bean id="handlerMapping"
                        class="org.springframework.web.servlet.mvc.method.annotation.RequestMappingHandlerMapping">
                <property name="interceptors">
                        <list>
                                <ref bean="officeHoursInterceptor"/>
                        </list>
                </property>
        </bean>

        <bean id="officeHoursInterceptor"
                        class="samples.TimeBasedAccessInterceptor">
                <property name="openingTime" value="9"/>
                <property name="closingTime" value="18"/>
        </bean>
</beans>
```





```java
package samples;

public class TimeBasedAccessInterceptor extends HandlerInterceptorAdapter {

        private int openingTime;
        private int closingTime;

        public void setOpeningTime(int openingTime) {
                this.openingTime = openingTime;
        }

        public void setClosingTime(int closingTime) {
                this.closingTime = closingTime;
        }

        public boolean preHandle(HttpServletRequest request, HttpServletResponse response,
                        Object handler) throws Exception {
                Calendar cal = Calendar.getInstance();
                int hour = cal.get(HOUR_OF_DAY);
                if (openingTime <= hour && hour < closingTime) {
                        return true;
                }
                response.sendRedirect("http://host.com/outsideOfficeHours.html");
                return false;
        }
}
```





