---
title: maven插件
date: 2019-06-15 12:29:22
tags: maven
---

## maven插件

#### 插件网址

 * [maven官网](https://maven.apache.org/plugins/,"MAVEN-PLUGIN")
 * [MojoHaus官网](http://www.mojohaus.org/plugins.html,"MojoHous-Plugin")

#### 常用插件
 * [tomcat](https://tomcat.apache.org/maven-plugin-trunk/)


```xml
        <pluginManagement>
          <plugins>
            <plugin>
              <groupId>org.apache.tomcat.maven</groupId>
              <artifactId>tomcat6-maven-plugin</artifactId>
              <version>2.3-SNAPSHOT</version>
            </plugin>
            <plugin>
              <groupId>org.apache.tomcat.maven</groupId>
              <artifactId>tomcat7-maven-plugin</artifactId>
              <version>2.3-SNAPSHOT</version>
            </plugin>
          </plugins>
        </pluginManagement>
```
* [assembly](https://maven.apache.org/plugins/maven-assembly-plugin/index.html)（打包）
 * zip
 * tar
 * tar.gz (or tgz)
 * tar.bz2 (or tbz2)
 * tar.snappy
 * tar.xz (or txz)
 * jar
 * dir
 * war
* versions 统一升级版本号
`mvn versions:set -DnewVersion=1.1`

#### 自定义插件
* [自定义Mojo](https://maven.apache.org/guides/plugin/guide-java-plugin-development.html)


1.extends AbstractMojo
```java
    package sample.plugin;
     
    import org.apache.maven.plugin.AbstractMojo;
    import org.apache.maven.plugin.MojoExecutionException;
    import org.apache.maven.plugins.annotations.Mojo;
     
    /**
     * Says "Hi" to the user.
     *
     */
    @Mojo( name = "sayhi")
    public class GreetingMojo extends AbstractMojo
    {
        public void execute() throws MojoExecutionException
        {
            getLog().info( "Hello, world." );
        }
    }
```
2.pom参数

```xml
    <project>
      <modelVersion>4.0.0</modelVersion>
     
      <groupId>sample.plugin</groupId>
      <artifactId>hello-maven-plugin</artifactId>
      <version>1.0-SNAPSHOT</version>
      <packaging>maven-plugin</packaging>
     
      <name>Sample Parameter-less Maven Plugin</name>
     
      <dependencies>
        <dependency>
          <groupId>org.apache.maven</groupId>
          <artifactId>maven-plugin-api</artifactId>
          <version>3.0</version>
        </dependency>
     
        <!-- dependencies to annotations -->
        <dependency>
          <groupId>org.apache.maven.plugin-tools</groupId>
          <artifactId>maven-plugin-annotations</artifactId>
          <version>3.4</version>
          <scope>provided</scope>
        </dependency>
      </dependencies>
    </project>
```
3.引用插件

```xml
    ...
      <build>
        <plugins>
          <plugin>
            <groupId>sample.plugin</groupId>
            <artifactId>hello-maven-plugin</artifactId>
            <version>1.0-SNAPSHOT</version>
          </plugin>
        </plugins>
      </build>
    ...
```
