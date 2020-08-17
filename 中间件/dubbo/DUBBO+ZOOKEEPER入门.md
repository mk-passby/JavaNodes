---
title: dubbo+zookeeper 入门
date: 2018-03-07 18:26:00
tags: dubbo
---

采用spring3.2+dubbo2.5.3+zookeeper3.3.6

所需架包如图所示   

![](DUBBO+ZOOKEEPER入门\1.png)

# 1.安装zookeeper

本文采用zookeeper-3.3.6，可自行查找下载。下载后进入conf目录下，修改zoo_sample.cfg为zoo.cfg。该文件为zookeeper的配置文件

```bash
# The number of milliseconds of each tick
tickTime=2000
# The number of ticks that the initial 
# synchronization phase can take
initLimit=10
# The number of ticks that can pass between 
# sending a request and getting an acknowledgement
syncLimit=5
# the directory where the snapshot is stored.
dataDir=/tmp/zookeeper
# the port at which the clients will connect
clientPort=2181
```

tickTime：基本事件单元，以毫秒为单位。**它用来控制心跳和超时，默认情况下最小的会话超时时间为两倍的 tickTime。**

dataDir：存放内存数据的地方

clientPort：用户于zookeeper相连的端口

initLimit：Leader允许F在 **initLimit** 时间内完成这个工作，请求和响应时间长度，最长不能超过多少个 tickTime 的时间长度，总的时间长度就是 10*2000=20 秒

syncLimit：检测机器的存活状态，请求和响应时间长度，最长不能超过多少个 tickTime 的时间长度，总的时间长度就是 5*2000=10 秒

此处需注意这种文件

```bash
tickTime=2000
dataDir=/usr/zdatadir
dataLogDir=/usr/zlogdir
clientPort=2181
initLimit=5
syncLimit=2
server.1=cloud:2888:3888
server.2=cloud02:2888:3888
server.3=cloud03:2888:3888
server.4=cloud04:2888:3888
server.5=cloud05:2888:3888
```

**server.A=B：C：D：**其中 A 是一个数字，表示这个是第几号服务器；**B 是这个服务器的 ip 地址；C 表示的是这个服务器与集群中的 Leader 服务器交换信息的端口；D 表示的是万一集群中的 Leader 服务器挂了，需要一个端口来重新进行选举，选出一个新的 Leader，而这个端口就是用来执行选举时服务器相互通信的端口。**如果是伪集群的配置方式，由于 B 都是一样，所以不同的 Zookeeper 实例通信端口号不能一样，所以要给它们分配不同的端口号

除了修改 zoo.cfg 配置文件，集群模式下还要配置一个文件 myid，这个文件在 dataDir 目录下，这个文件里面就有一个数据就是 A 的值，**Zookeeper 启动时会读取这个文件，拿到里面的数据与 zoo.cfg 里面的配置信息比较从而判断到底是那个 server。**

![](DUBBO+ZOOKEEPER入门\2.png)

# 2.启动zookeeper

此处我用的为第一个配置文件，并没有配置集群。直接打开zookeeper-3.3.6\\bin\\zkServer.cmd，这里我是window环境，linux环境运行.sh文件

# 3.dubbo

通过将服务统一管理起来，可以有效地优化内部应用对服务发布/使用的流程和管理。服务注册中心可以通过特定协议来完成服务对外的统一。
    
dubbo就是一个服务框架，可以实现调用远程接口像调用本地接口一样方便。
    
这里注册中心我们选择zookeeper

## 3.1安装Dubbo-admin，实现监控

这里我安装的是dubbo-admin-2.5.3.war，部署到tomcat的webapps下，修改webapps\\dubbo-admin-2.5.3\\WEB-INF\\dubbo.properties文件，内容如下

```ini
dubbo.registry.address=zookeeper://127.0.0.1:2181
dubbo.admin.root.password=root
dubbo.admin.guest.password=guest
```

其中root和guest都为密码。

启动tomcat，效果如下(如提示输出账号信息，输入root，root)

![](DUBBO+ZOOKEEPER入门\3.png)

## 3.2dubbo流程

provider注册----生产---->zookeeper<----消费--------consumer
    
生产者将接口信息注册到zookeper，消费者通过zookeeper进行消费

## 3.3生产者注册

Provider.java文件如下

```java
package com.dubbotest.provider;
 
import org.springframework.context.support.ClassPathXmlApplicationContext;
 
public class Provider {
 
    public static void main(String[] args) throws Exception {
        ClassPathXmlApplicationContext context = new ClassPathXmlApplicationContext("applicationContext-service.xml");
        context.start();
        System.out.println("-----------");
        System.in.read(); // 为保证服务一直开着，利用输入流的阻塞来模拟,任意键退出
    }
}
```

applicationContext-service.xml如下

```xml
<beans xmlns="http://www.springframework.org/schema/beans"
    xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns:dubbo="http://code.alibabatech.com/schema/dubbo"
    xsi:schemaLocation="http://www.springframework.org/schema/beans  
        http://www.springframework.org/schema/beans/spring-beans-3.2.xsd  
        http://code.alibabatech.com/schema/dubbo  
        http://code.alibabatech.com/schema/dubbo/dubbo.xsd  
        ">
 
    <bean id="user" class="com.model.User">
        <property name="name" value="person" />
    </bean>
 
    <!-- 具体的实现bean -->
    <bean id="demoService" class="com.dubbotest.provider.DemoServiceImpl" />
 
    <!-- 提供方应用信息，用于计算依赖关系 -->
    <dubbo:application name="xixi_provider" />
 
    <!-- 使用multicast广播注册中心暴露服务地址 <dubbo:registry address="multicast://224.5.6.7:1234" 
        /> -->
 
    <!-- 使用zookeeper注册中心暴露服务地址 -->
    <dubbo:registry address="zookeeper://127.0.0.1:2181" />
 
    <!-- 用dubbo协议在20880端口暴露服务 -->
    <dubbo:protocol name="dubbo" port="20880" />
 
    <!-- 声明需要暴露的服务接口 -->
    <!-- <dubbo:service interface="com.dubbotest.provider.DemoService" ref="demoService" 
        /> -->
    <dubbo:service interface="com.dubbotest.provider.DemoService"
        ref="demoService" />
 
</beans>
```

运行了provider中的main函数后可以在dubbo-admin管理中看见接口信息。

![](DUBBO+ZOOKEEPER入门\4.png)

## 3.4consumer消费

Consumer.java如下

```java
package com.dubbotest.consumer;
 
import java.util.List;
 
import org.springframework.context.support.ClassPathXmlApplicationContext;
 
import com.dubbotest.provider.DemoService;
import com.model.User;
 
public class Consumer {
 
    public static void main(String[] args) throws Exception {  
        ClassPathXmlApplicationContext context = new ClassPathXmlApplicationContext(  
                new String[] { "applicationContext-dubbo.xml" });  
        context.start();  
        DemoService demoService = (DemoService) context.getBean("demoService"); 
        String hello = demoService.sayHello("tom"); 
        System.out.println(hello); 
        List<User> list = demoService.getUsers();  
        if (list != null && list.size() > 0) {  
            for (int i = 0; i < list.size(); i++) {  
                System.out.println(list.get(i).toString());  
            }  
        }  
        System.in.read();  
    }  
}
```

applicationContext-dubbo.xml如下

```xml
<?xml version="1.0" encoding="UTF-8"?>  
<beans xmlns="http://www.springframework.org/schema/beans"  
        xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns:dubbo="http://code.alibabatech.com/schema/dubbo"  
        xsi:schemaLocation="http://www.springframework.org/schema/beans  
            http://www.springframework.org/schema/beans/spring-beans.xsd  
            http://code.alibabatech.com/schema/dubbo  
            http://code.alibabatech.com/schema/dubbo/dubbo.xsd  
            ">  
        
        <!-- 消费方应用名，用于计算依赖关系，不是匹配条件，不要与提供方一样 -->  
        <dubbo:application name="hehe_consumer" />  
       
        <!-- 使用zookeeper注册中心暴露服务地址 -->  
        <!-- <dubbo:registry address="multicast://224.5.6.7:1234" /> -->  
        <dubbo:registry address="zookeeper://127.0.0.1:2181" />  
       
        <!-- 生成远程服务代理，可以像使用本地bean一样使用demoService -->  
        <dubbo:reference id="demoService"  
            interface="com.dubbotest.provider.DemoService" />  
       
</beans>
```

运行后结果如图 ：

![](DUBBO+ZOOKEEPER入门\5.png)

## 3.5其他文件：

DemoService.java

```java
package com.dubbotest.provider;
 
import java.util.List;
 
public interface DemoService {
    String sayHello(String name);  
       
    public List getUsers();  
}
```

DemoServiceImpl.java

```java
package com.dubbotest.provider;
 
import java.util.ArrayList;
import java.util.List;
 
import org.springframework.stereotype.Component;
 
import com.alibaba.dubbo.config.annotation.Service;
import com.model.User;
 
@Service(version="1.0")//此处Component是Spring bean注解，Service是dubbo的注解
public class DemoServiceImpl implements DemoService{
 
    @Override
    public String sayHello(String name) {
        // TODO Auto-generated method stub
         return "Hello " + name;  
    }
 
    @Override
    public List getUsers() {
          List list = new ArrayList();  
             User u1 = new User();  
             u1.setName("jack");  
             u1.setAge(20);  
             u1.setSex("男");  
                
             User u2 = new User();  
             u2.setName("tom");  
             u2.setAge(21);  
             u2.setSex("女");  
                
             User u3 = new User();  
             u3.setName("rose");  
             u3.setAge(19);  
             u3.setSex("女");  
                
             list.add(u1);  
             list.add(u2);  
             list.add(u3);  
             return list;  
    }
 
}
```

User.java

```java
package com.model;
 
import java.io.Serializable;
 
public class User implements Serializable{
     
    private String name;
    private String sex;
    private int age;
 
    @Override
    public String toString() {
        return "User [name=" + name + ", sex=" + sex + ", age=" + age + "]";
    }
 
    public String getName() {
        return name;
    }
 
    public void setName(String name) {
        this.name = name;
    }
 
    public String getSex() {
        return sex;
    }
 
    public void setSex(String sex) {
        this.sex = sex;
    }
 
    public int getAge() {
        return age;
    }
 
    public void setAge(int age) {
        this.age = age;
    }
 
}
```