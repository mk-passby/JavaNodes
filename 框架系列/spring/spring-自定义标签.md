---
title: Spring源码学习——自定义标签
date: 2017-11-28 11:44:00
tags: spring
---

# 1.自定义标签步骤

1.  创建一个需要扩展的组件
2.  定义xsd文件描述组件内容
3.  创建一个文件，实现BeanDefinitionParser接口，解析xsd文件中的定义和组件定义
4.  创建handler文件，扩展NamespaceHandlerSupport，注册组件到spring容器
5.  编写spring.handlers和spring.schemas文件

# 2.代码如下

## 1.编写pojo

```java
public class User {
	
	private String name;
	private String sex;
	private int age;
//省略getter、setter

}
```

## 2.xsd文件描述组件内容

```xml
<?xml version="1.0"?>
<schema xmlns="http://www.w3.org/2001/XMLSchema" targetNamespace="http://www.springtest.com/schema/user"
	xmlns:tns="http://www.springtest.com/schema/user" elementFormDefault="qualified">
	<!-- 表示数据类型等定义来自w3 -->
	<!--表示文档中要定义的元素来自什么命名空间 -->
	<!--表示此文档的默认命名空间是什么 -->
	<!--表示要求xml文档的每一个元素都要有命名空间指定 -->

	<!-- ……定义主体部分…… -->
	<element name="user">
		<complexType>
			<attribute name="id" type="string"></attribute>
			<attribute name="name" type="string"></attribute>
			<attribute name="sex" type="string"></attribute>
			<attribute name="age" type="int"></attribute>
		</complexType>
	</element>

</schema>
```

描述了一个新的targetNamespace，并定义了一个name是user的element，有id，name，sex，age属性

## 3.创建类，实现BeanDefinitionParser接口

```java
package test.customtag;

import org.springframework.beans.factory.support.BeanDefinitionBuilder;
import org.springframework.beans.factory.xml.AbstractSingleBeanDefinitionParser;
import org.springframework.util.StringUtils;
import org.w3c.dom.Element;

import com.model.User;

public class UserBeanDefinitionParser extends AbstractSingleBeanDefinitionParser {
	// Element对应的类
	protected Class getBeanClass(Element element) {
		return User.class;
	}

	// 从element中解析并提取对应的元素
	protected void doParse(Element element, BeanDefinitionBuilder bean) {
		String name = element.getAttribute("name");
		String sex = element.getAttribute("sex");
		String age = element.getAttribute("age");
		// 将提取的数据放入到BeanDefinitionBuilder中，将所有beanbeanFactory中
		if (StringUtils.hasText(name)) {
			bean.addPropertyValue("name", name);
		}
		if (StringUtils.hasText(sex)) {
			bean.addPropertyValue("sex", sex);
		}
		if (StringUtils.hasText(age)) {
			bean.addPropertyValue("age", Integer.parseInt(age));
		}

	}

}
```

## 4.创建handler文件，注册spring容器

```java
package test.customtag;

import org.springframework.beans.factory.xml.NamespaceHandlerSupport;

/******创建handler文件，组件注册到spring容器***/
public class MyNamespaceHandler extends NamespaceHandlerSupport{

	@Override
	public void init() {
		// TODO Auto-generated method stub
		registerBeanDefinitionParser("user", new UserBeanDefinitionParser());
	}

}
```

## 5.编写spring.handlers和spring.schemas文件，默认在工程的/META-INF/文件下

spring.handlers

```xml
http\://www.springtest.com/schema/user=test.customtag.MyNamespaceHandler
```

spring.schemas

```xml
http\://www.springtest.com/schema/user.xsd=META-INF/Spring-test.xsd
```

此处注意：

这里因为我创建的是java项目，直接在项目下建造META-INF会提示找不到对应的文件，所以这里是将文件打包成jar包导入到项目中。如下图所示

![](https://static.oschina.net/uploads/space/2017/1128/113739_vNLU_3429289.png)![](https://static.oschina.net/uploads/space/2017/1128/113750_HHla_3429289.png)

## 6.测试

导入自定义标签

```xml
<beans xmlns="http://www.springframework.org/schema/beans"
	xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" 
	xmlns:myname="http://www.springtest.com/schema/user"
	xsi:schemaLocation="http://www.springframework.org/schema/beans  
        http://www.springframework.org/schema/beans/spring-beans-3.2.xsd  
        http://www.springtest.com/schema/user
       	http://www.springtest.com/schema/user.xsd
        ">
	<myname:user id="testBean" name="aaaaaa" sex="dsaf" age="12"></myname:user>
</beans>
```

测试代码

```java

public class Test {
	/****测试输出*/
	@org.junit.Test
	public void test1(){
		System.out.println("--------");
		ApplicationContext act=new ClassPathXmlApplicationContext("applicationContext-service.xml");
		User u=(User) act.getBean("testBean");
		System.out.println("--------------"+u.toString());
	}
}
```

输出结果

![](https://static.oschina.net/uploads/space/2017/1128/113515_sKwH_3429289.png)

# 3.整个项目结构

![](https://static.oschina.net/uploads/space/2017/1128/113903_VcT2_3429289.png)

参考自：spring源码深度解析