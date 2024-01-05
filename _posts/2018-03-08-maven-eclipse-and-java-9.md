---
id: 2777
title: 'Maven, Eclipse and Java 9'
date: '2018-03-08T12:12:39+00:00'
author: 'Martin Farrell'
layout: post
guid: 'https://www.javabullets.com/?p=2777'
permalink: /maven-eclipse-and-java-9/
thrive_post_fonts:
    - '[]'
image: /wp-content/uploads/2017/09/stefan-kunze-26932.jpg
categories:
    - 'Java EE 8'
    - Java9
tags:
    - eclipse
    - 'Java 9'
    - maven
---

Anyone that uses the M2E(maven-to-eclipse) plugin in eclipse knows the issue where you have run a build, then update maven on your project only to have it reset the JRE and throw up a wall of project errors! I noticed the issue in the post I made on running [Java EE 8 with Java 9 using Open Liberty](https://www.javabullets.com/java-9-on-java-ee-8-using-eclipse-and-open-liberty/)

The solution is to ensure the compiler is running the correct version you require, so –

```
<pre class=""><properties>
   <maven.compiler.source>1.8</maven.compiler.source>
   <maven.compiler.target>1.8</maven.compiler.target>
   <failOnMissingWebXml>false</failOnMissingWebXml>
</properties>
```

Or –

```
<pre class=""> <build>
  <plugins>
   <plugin>
    <groupId>org.apache.maven.plugins</groupId>
    <artifactId>maven-compiler-plugin</artifactId>
    <version>3.7.0</version>
    <configuration>
     <source>1.8</source>
     <target>1.8</target>
    </configuration>
   </plugin>
  </plugins>
 </build>
```

There is a further point worth noting if you are using Java 9 or later, as the version number format is different. So while Java 8 has a version of 1.8, Java 9 has a version of 9 –

<div class="wp-caption aligncenter" id="attachment_2779" style="width: 310px">![](https://i0.wp.com/www.javabullets.com/wp-content/uploads/2018/03/j9j8cmd.png?resize=300%2C158&ssl=1)Java 9 vs Java 8 versions

</div>This means you need to state the version in your pom as –

```
<pre class=""> <properties>
   <maven.compiler.source>9</maven.compiler.source>
   <maven.compiler.target>9</maven.compiler.target>
   <failOnMissingWebXml>false</failOnMissingWebXml>
 </properties>
```

Or using the full version in my JavaEE8 project(https://github.com/farrelmr/javaee8/blob/master/pom.xml) this would be stated as –

```
<pre class=""><project 
 xmlns="http://maven.apache.org/POM/4.0.0" 
 xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
 xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
 <modelVersion>4.0.0</modelVersion>
 <groupId>com.javabullets.javaee8</groupId>
 <artifactId>javaee8</artifactId>
 <version>1.0-SNAPSHOT</version>
 <packaging>war</packaging>
 <dependencies>
  <dependency>
   <groupId>javax</groupId>
   <artifactId>javaee-api</artifactId>
   <version>8.0</version>
   <scope>provided</scope>
  </dependency>
 </dependencies>
 <build>
  <finalName>javaee8</finalName>
 </build>
 <properties>
  <maven.compiler.source>9</maven.compiler.source>
  <maven.compiler.target>9</maven.compiler.target>
  <failOnMissingWebXml>false</failOnMissingWebXml>
 </properties>
</project>
```

This will prevent the problem in eclipse

This post may seem obvious – but it is worth stating that the change in version number for Java 9 means you must check you are using the correct version number in maven