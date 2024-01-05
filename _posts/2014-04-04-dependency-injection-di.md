---
id: 2107
title: 'Dependency Injection (DI)'
date: '2014-04-04T11:45:48+00:00'
author: 'Martin Farrell'
layout: post
guid: 'http://glenware.wordpress.com/?p=86'
permalink: /dependency-injection-di/
geo_public:
    - '0'
thrive_post_fonts:
    - '[]'
categories:
    - CoreJava
tags:
    - 'Dependency Injection'
    - Spring
---

Dependency Injection(DI) is part of larger concept of Inversion of Control(IoC). It means the object is not responsible for creating its own dependencies, and dependencies will be injected when it needs to know them

**Em, What?**

Consider the following Cyclist class. It creates and manages its own dependencies in its constructor –

\[sourcecode lang=”java”\] public class Cyclist  
{  
 private RacingBike racingBike;  public Cyclist()  
 {  
 // Establish own dependency  
 racingBike = new RacingBike();  
 }  
}  
\[/sourcecode\]

In dependency injection using Spring this class would be rewritten as –

\[sourcecode lang=”java”\] public class Cyclist {  
 private Bike bike;  public void setBike(Bike bike){  
 this.bike = bike;  
 }

 public void getBike(){  
 return bike;  
 }  
}  
\[/sourcecode\]

Client code –

\[sourcecode lang=”java”\] public class App {  
 public static void main(String\[\] args) {  
 ApplicationContext context = new ClassPathXmlApplicationContext("beans.xml");  
 Cyclist cyclist = (Cyclist) context.getBean("cyclist");  
 cyclist.getBike();  
 }  
}  
\[/sourcecode\] The DI framework would then “inject” the bike dependency through a config file –

\[sourcecode lang=”xml”\] &lt;?xml version="1.0" encoding="UTF-8"?&gt; &lt;beans xmlns="http://www.springframework.org/schema/beans"  
 xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"  
 xsi:schemaLocation="http://www.springframework.org/schema/beans  
 http://www.springframework.org/schema/beans/spring-beans-3.0.xsd"&gt;

 &lt;bean id="racingBike" class="RacingBike"&gt;  
 &lt;bean id="cyclist" class="Cyclist"&gt;  
 &lt;property name="bike" value="racingBike"/&gt;  
 &lt;/bean&gt;

&lt;/beans&gt;  
\[/sourcecode\]

The advantages of DI are –

- Testable – Main advantage, and this helps improve code quality
- Loose coupling – Code to interfaces
- Reuseable – Can replace implementation classes, so you could swap JDBC for say hibernate
- Single responsibilty principle – Classes can focus on their own tasks

Types of injection –

- Constructor
- Setter
- Interface – not in Spring