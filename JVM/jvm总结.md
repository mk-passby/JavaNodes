---
title: jvm总结
date: 2020-02-13 16:40:16
tags: jvm
---



#### JVM运行时数据区

![img](jvm总结\1.png)

JVM运行时数据区由以下组成：

程序计数器：指向当前线程正在执行的字节码指令的地址行号等。（记录程序运行到哪里了）

虚拟机栈：当前线程运行方法所需要的数据指令，返回地址

本地方法栈：native方法使用的栈帧

方法区：类信息，常量，静态变量等

Heap：堆内存（存放对象实例）

#### java内存模型

![img](jvm总结\2.png)

由于对象的生命周期不一样，出现了分代，98%的对象在minorGC的时候会被回收掉

结合图一可以看到，新生代和老年代是分配在堆内存中，而1.8以前的永久代是分配在方法区，1.8以后的元空间(meta space)是一个独立的内存。

补充：32位系统最大支持2^32即4GB的内存，64位系统支持2^(64-16)，并非2^64次方，有16位的保留位

注意 ，元空间相当于是动态扩容，只要内存够用

#### 虚拟机栈

![](jvm总结\虚拟机栈.png)



虚拟机栈主要由局部变量表、操作数栈、动态链接、方法出口几个部分组成，默认大小为1024。

[字节码指令表](https://www.jianshu.com/p/7d623f441bed)

- 局部变量表（Local Variable Table）
  - 是一组变量值存储空间，用于存放方法参数和方法内部定义的局部变量
  - 局部变量表的基本单位为变量槽（slot）；局部变量表存放的是方法参数和局部变量；虚拟机是通过索引定位的方式使用局部变量表。
  - 局部变量表中第0位索引的 Slot 默认是用于传递方法所属对象实例的引用（reference），即 “this” 关键字指向的对象，分配完方法参数后，便会依次分配方法内部定义的局部变量
  - 局部变量表存放着编译期可知的基本数据类型（boolean，byte、char、short、int、float、long、double）、对象引用和returnAddress（指向一条字节码指令地址）。其中long和double占两个局部变量空间（Slot），其余数据类型只占一
  - 局部变量表是在编译时期就确定了的

- 操作数栈
  
  - 即为操作栈，同局部变量表一样，
  
- 动态链接
  
  - 指向运行时常量池(方法区)中该栈帧所属性方法的引用，此引用可能是直接地址，也可能是句柄(在windows中指向的是句柄)
  
- 方法返回地址
  - 执行引擎遇到一个方法返回的字节码指令
  - 防止执行过程中遇到了异常
  
##### 导致虚拟机栈溢出的原因有哪些？

栈大小：通过java -XX:+PrintFlagsFinal -version | grep ThreadStack可以看到，默认的栈大小为1024，即1M。

同时需要注意：-Xss65K以下设置会报错如下，(提示最小值设置为108K，但是实际测试最小值为65k)

```java
"C:\Program Files\Java\jdk1.8.0_131\bin\java.exe" -Xss10k -Didea.test.cyclic.buffer.size=1048576 "-javaagent:D:\software\develop\idea\IntelliJ IDEA 2018.3.5\lib\idea_rt.jar=6085:D:\software\develop\idea\IntelliJ IDEA 2018.3.5\bin" -Dfile.encoding=UTF-8 -classpath "D:\software\develop\idea\IntelliJ IDEA 2018.3.5\lib\idea_rt.jar;D:\software\develop\idea\IntelliJ IDEA 2018.3.5\plugins\junit\lib\junit-rt.jar;D:\software\develop\idea\IntelliJ IDEA 2018.3.5\plugins\junit\lib\junit5-rt.jar;C:\Program Files\Java\jdk1.8.0_131\jre\lib\charsets.jar;C:\Program Files\Java\jdk1.8.0_131\jre\lib\deploy.jar;C:\Program Files\Java\jdk1.8.0_131\jre\lib\ext\access-bridge-64.jar;C:\Program Files\Java\jdk1.8.0_131\jre\lib\ext\cldrdata.jar;C:\Program Files\Java\jdk1.8.0_131\jre\lib\ext\dnsns.jar;C:\Program Files\Java\jdk1.8.0_131\jre\lib\ext\jaccess.jar;C:\Program Files\Java\jdk1.8.0_131\jre\lib\ext\jfxrt.jar;C:\Program Files\Java\jdk1.8.0_131\jre\lib\ext\localedata.jar;C:\Program Files\Java\jdk1.8.0_131\jre\lib\ext\nashorn.jar;C:\Program Files\Java\jdk1.8.0_131\jre\lib\ext\sunec.jar;C:\Program Files\Java\jdk1.8.0_131\jre\lib\ext\sunjce_provider.jar;C:\Program Files\Java\jdk1.8.0_131\jre\lib\ext\sunmscapi.jar;C:\Program Files\Java\jdk1.8.0_131\jre\lib\ext\sunpkcs11.jar;C:\Program Files\Java\jdk1.8.0_131\jre\lib\ext\zipfs.jar;C:\Program Files\Java\jdk1.8.0_131\jre\lib\javaws.jar;C:\Program Files\Java\jdk1.8.0_131\jre\lib\jce.jar;C:\Program Files\Java\jdk1.8.0_131\jre\lib\jfr.jar;C:\Program Files\Java\jdk1.8.0_131\jre\lib\jfxswt.jar;C:\Program Files\Java\jdk1.8.0_131\jre\lib\jsse.jar;C:\Program Files\Java\jdk1.8.0_131\jre\lib\management-agent.jar;C:\Program Files\Java\jdk1.8.0_131\jre\lib\plugin.jar;C:\Program Files\Java\jdk1.8.0_131\jre\lib\resources.jar;C:\Program Files\Java\jdk1.8.0_131\jre\lib\rt.jar;E:\developProject\learndemo\learning-demo\jvm-learn\target\test-classes;E:\developProject\learndemo\learning-demo\jvm-learn\target\classes;E:\saveDatas\mavenRepository\junit\junit\4.11\junit-4.11.jar;E:\saveDatas\mavenRepository\org\hamcrest\hamcrest-core\1.3\hamcrest-core-1.3.jar" com.intellij.rt.execution.junit.JUnitStarter -ideVersion5 -junit4 com.mk.learn.StackOverFlowTestTest,test

Error: Could not create the Java Virtual Machine.
The stack size specified is too small, Specify at least 108k
Error: A fatal exception has occurred. Program will exit.
```



- 调用链太长

- 死循环

- 无限递归

  

##### 代码演示栈溢出

- 如上



##### 如何避免

##### 如何调优



#### 对象

##### 怎么计算对象大小

一个空对象未开启指针压缩为：8B(markword)+8B(kclass point)+0B(数组)+ 0B(对齐填充)=16B

开启指针压缩为：8B(markword)+4B(kclass point)+0B(数组)+ 4B(对齐填充)=16B



![1589121656223](jvm总结\对象组成.png)



##### 指针压缩

​	在堆中，32位的对象引用（指针）32/8Bits占4个字节，而64位的对象引用64/8bits占8个字节。也就是说，64位的对象引用大小是32位的2倍。64位JVM在支持更大堆的同时，由于对象引用变大却带来了性能问题。为了能够保持32位的性能，oop必须保留32位。由于java中采用8字节对齐的，利用这个特性，开启指针压缩后，在压缩过程中，将数据右移3位，保存到32位地址，在解压再把32位左移3位放大8倍。所以oop能表示的范围为4(字节)\*8(位)+3(擦除的后三位)，即35位，所以我们要求使用UseCompressedOops条件是堆内存在4GB\*8=32GB以内。

- java中使用 +XX:+UseCompressedOops开启指针压缩，
- 开启指针压缩占用4字节，不开启占用8字节。UseCompressedOops其原理就是利用java中8字节对齐。
- 如果GC堆大小在4G以下，直接砍掉高32位，避免了编码解码过程；
- 如果GC堆大小在4G以上32G以下，则启用UseCompressedOop；
- 如果GC堆大小大于32G，压指失效，使用原来的64位



#### jvm参数配置

##### VM参数

http://www.oracle.com/technetwork/java/javase/tech/vmoptions-jsp-140102.html



-Xms20M  starting	起始堆内存大小

-Xmx     max	最大堆内存大小

-Xmn     new	新生代大小



对象分配eden

-XX:SurvivorRatio=8

8:1:1



TLAB  Thread Local Allaction Buffer  栈上分配



对象很大

​	-XX:PretenureSizeThreshold=3145728   3M

长期存活的对象 

​	-XX:MaxTenuringThreshold=15

动态对象年龄判定

​	相同年龄所有对象的大小总和 > Survivor空间的一半

​	

分配担保

 Minor GC 之前检查 

 老年代最大可用连续空间是否>新生代所有对象总空间

​	

Minor GC		新生代垃圾回收

Major GC		old区垃圾回收

Full  GC		minor+major

JVM调优最终目的减小垃圾回收

**3.垃圾回收算法**

引用

​	强  Object object = new Object();

​	软  SoftReference引用的对象

​	弱  WeakReference引用的对象

​	虚  PhantomReference引用的对象



​	

回收

​	方法论

​		标记-清除算法

​		复制回收算法

​		标记-整理算法

![img](jvm总结\3.png)

​	垃圾收集器

​		STW  Stop The World

​		Serial

​		ParNew 

​			-XX:ParallelGCThreads

​		Parallel Scavenge （全局）

​			吞吐量 = 运行用户代码时间 / （运行用户代码时间  + 垃圾收集时间）

​			-XX:MaxGCPauseMillis=n

​			-XX:GCTimeRatio=n

​			-XX:UseAdaptiveSizePolicy   GC  Ergonomics

​		Serial Old

​			CMS备用预案  Concurrent Mode Failusre时使用

​			标记-整理算法

​		Parallel Old

​			标记-整理算法

​		CMS

​			标记-清除算法

​			减少回收停顿时间

​			碎片 -XX:CMSInitiatingOccupancyFraction  

​			Concurrent Mode Failure 启用Serial Old

​			

​			-XX:+UseCMSCompactAtFullCollection

​			-XX:CMSFullGCsBeforeCompaction 执行多少次不压缩FullGC后 来一次带压缩的 0 表示每次都压

​			-XX:+UseConcMarkSweep

​		G1



![img](jvm总结\4.png)

​		

​	注：新生代均用复制回收算法





**4.GC定位**

如何查看当前的垃圾回收器

​	 

​	-XX:+PrintCommandLineFlags

![img](jvm总结\5.jpg)

​	server client

​	MBean

GC日志

​	1.输出日志

​	-XX:+PrintGCTimeStamps 

​	-XX:+PrintGCDetails 

​	-Xloggc:/home/administrator/james/gc.log

​	-XX:+PrintHeapAtGC

​	2.日志文件控制

​	-XX:-UseGCLogFileRotation

​	-XX:GCLogFileSize=8K

​	3.怎么看

​	

JDK自带的 监控工具

https://docs.oracle.com/javase/8/docs/technotes/tools/windows/toc.html

​	jmap -heap pid 堆使用情况

​	jstat  -gcutil pid 1000

​	jstack  线程dump 

​	jvisualvm

​	jconsole

​	

MAT

​	http://help.eclipse.org/oxygen/index.jsp?topic=/org.eclipse.mat.ui.help/welcome.html

​	-XX:+HeapDumpOnOutOfMemoryError 

​	-XX:HeapDumpPath=/home/administrator/james/error.hprof



怀疑：

​	1.看GC日志  126719K->126719K(126720K)

​	2.dump

​	3.MAT

​		1.占用Retained Heap

​		2.看有没有GC Root指向

​	

什么条件触发STW的Full GC呢？

Perm空间不足；

CMS GC时出现promotion failed和concurrent mode failure（concurrent mode failure发生的原因一般是CMS正在进行，但是由于老年代空间不足，需要尽快回收老年代里面的不再被使用的对象，这时停止所有的线程，同时终止CMS，直接进行Serial Old GC）；

（promontion faild产生的原因是EDEN空间不足的情况下将EDEN与From survivor中的存活对象存入To survivor区时,To survivor区的空间不足，再次晋升到old gen区，而old gen区内存也不够的情况下产生了promontion faild从而导致full gc	）



统计得到的Young GC晋升到老年代的平均大小大于老年代的剩余空间；



主动触发Full GC（执行jmap -histo:live [pid]）来避免碎片问题。

​	

​	

java -Xms8m -Xmx64m -verbose:gc -Xloggc:/home/administrator/james/gc.log  -XX:+PrintHeapAtGC -XX:+PrintGCApplicationStoppedTime -XX:+PrintGCTimeStamps -XX:+PrintCommandLineFlags -XX:+PrintFlagsFinal -XX:+PrintGCDetails -XX:+UseConcMarkSweepGC -Dcom.sun.management.jmxremote.authenticate=false -Dcom.sun.management.jmxremote.ssl=false -Dcom.sun.management.jmxremote.port=9004 -Djava.rmi.server.hostname=177.1.1.122 -jar jvm-demo1-0.0.1-SNAPSHOT.jar  > catalina.out  2>&1 &



java -Xms128m -Xmx128m -verbose:gc -Xloggc:/home/administrator/james/gc.log  -XX:+HeapDumpOnOutOfMemoryError -XX:HeapDumpPath=/home/administrator/james/error.hprof -XX:+PrintGCApplicationStoppedTime -XX:+PrintGCTimeStamps -XX:+PrintCommandLineFlags -XX:+PrintFlagsFinal -XX:+PrintGCDetails -XX:+UseConcMarkSweepGC  -XX:+UseCMSCompactAtFullCollection -Dcom.sun.management.jmxremote.authenticate=false -Dcom.sun.management.jmxremote.ssl=false -Dcom.sun.management.jmxremote.port=9004 -Djava.rmi.server.hostname=177.1.1.122 -jar jvm-demo1-0.0.1-SNAPSHOT.jar  > catalina.out  2>&1 &

 	

java -Xms128m -Xmx128m -verbose:gc -Xloggc:/home/administrator/james/gc.log  -XX:+HeapDumpOnOutOfMemoryError -XX:+PrintHeapAtGC -XX:HeapDumpPath=/home/administrator/james/error.hprof -XX:+PrintGCApplicationStoppedTime -XX:+PrintGCTimeStamps -XX:+PrintCommandLineFlags -XX:+PrintFlagsFinal -XX:+PrintGCDetails -XX:+UseCMSCompactAtFullCollection -Dcom.sun.management.jmxremote.authenticate=false -Dcom.sun.management.jmxremote.ssl=false -Dcom.sun.management.jmxremote.port=9004 -Djava.rmi.server.hostname=177.1.1.122 -jar jvm-demo1-0.0.1-SNAPSHOT.jar  > catalina.out  2>&1 &

 	



-XX:+CMSScavengeBeforeRemark

​	

​	







​		

![img](jvm总结\6.jpg)



![img](jvm总结\7.jpg)

#### JVM问题

##### 1.默认内存大小？如何证明？

​	最小物理内存的1/64，最大物理内存的1/4，

​	通过指令查看：

- Windows：java -XX:+PrintFlagsFinal -version | findstr  HeapSize
- Linux：java -XX:+PrintFlagsFinal -version | grep HeapSize

##### 2.堆为什么分新生代/老年代，为什么新生代1/3，老年代2/3？

- 1.方便垃圾回收，新生代会频繁GC。
- 2.老年代肯定比新生代大，Eden区90%的对象在第一次GC就被

##### 3.为什么新生代老年代比例是1：2

##### 4.为什么新生代要分三个区，且Eden，From，To为什么是8:1:1？

​	由于对象的生命周期不一样，出现了分代

​	基于一个经验值，根据统计调查发现Eden区90%~95%的对象在第一次GC就被回收掉，如果eden设置过低，会导致内存浪费

##### 5.新生代的三个区是如何协同工作的？新生代是如何转移到老年代的

- 1.对象首先在Eden中分配，如果eden中没有足够的空间会触发一次MinorGC。当Minor GC结束后，Eden区会被清空，存活的对象放入survivor中，且对象年龄计算器(Age)+1，当survivor中放不下，分配担保进入老年代中。

##### 6.什么样的对象会进入老年代

- 1.大对象，虚拟机提供了一个-XX:PretenureSizeThreshold参数(字节大小可以设分配到新生代对象的大小限制)，大于这个值的参数会直接在老年代分配。-XX:PretenureSizeThreshold默认值是0，即对象的大小超过Eden区的对象，直接进入老年代
- 2.GC年龄超过15次还存活的对象
- 3.空间分配担保的对象

