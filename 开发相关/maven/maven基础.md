---
title: maven基础
date: 2019-05-19 22:29:22
tags: maven
---


## maven基础
#### 优势
- 相比传统项目，减少了手动导入工序，更加简便
- 插件丰富
- 构建简单


#### 安装
1. [maven下载地址](https://maven.apache.org/download.cgi，"点击跳转")
2. 配置MVN_HOME，如JAVA_HOME类似
3. 配置完成后，cmd输入`mvn-version` 检测是否配置成功
4. 配置setting.xml，路径在maven安装目录conf目录下，打开setting.xml配置一个aliyun的仓库(默认仓库下载速度，以前也可以配置oschina打仓库，现在关闭了)


```xml
      <mirrors>
      <!--aliyun仓库-->
    	<mirror>
    	  <id>alimaven</id>
    	  <name>aliyun maven</name>
    	  <url>http://maven.aliyun.com/nexus/content/groups/public/</url>
    	  <mirrorOf>central</mirrorOf>
    	</mirror>
    	<!-- 中央仓库1 -->
        <mirror>
            <id>repo1</id>
            <mirrorOf>central</mirrorOf>
            <name>Human Readable Name for this Mirror.</name>
            <url>http://repo1.maven.org/maven2/</url>
        </mirror>
    
        <!-- 中央仓库2 -->
        <mirror>
            <id>repo2</id>
            <mirrorOf>central</mirrorOf>
            <name>Human Readable Name for this Mirror.</name>
            <url>http://repo2.maven.org/maven2/</url>
        </mirror>
        <!-- mirror
         | Specifies a repository mirror site to use instead of a given repository. The repository that
         | this mirror serves has an ID that matches the mirrorOf element of this mirror. IDs are used
         | for inheritance and direct lookup purposes, and must be unique across the set of mirrors.
         |
        <mirror>
          <id>mirrorId</id>
          <mirrorOf>repositoryId</mirrorOf>
          <name>Human Readable Name for this Mirror.</name>
          <url>http://my.repository.com/repo/path</url>
        </mirror>
         -->
      </mirrors>
```
#### maven加载顺序
首先是到用户目录下打.m2目录下setting中去找jar包，再去conf下的setting中找的
maven->/.m2/setting.xml->conf/settiong.xml


#### pom.xml文件说明
新建maven此处不做说明
- groupid：公司名(网址）
- artifactId：模块功能名(common，web，model，dao)
- version：版本号
- pachaging：打包方式，默认jar
- dependencyManagemanet：依赖管理，统一版本号
- Dependency：依赖
 * Type：默认jar
 * scope：
   * complile：编译
   * test：测试
   * provided：已经提供了，不需要打进包
   * runtime：运行时需要
   * system：本地依赖的jar包 
 * exclusions：剔除不需要的jar包，多用于jar包冲突


#### 基本的命令
- compile:编译(mvn clean:compile)
- test：运行test
- package：打包
- install：install到本地仓库
- deploy：发布到远程仓库

