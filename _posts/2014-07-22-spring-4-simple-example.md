---
id: 137
title: 'Spring 4 Simple Example'
date: '2014-07-22T20:51:36+00:00'
author: 'Martin Farrell'
layout: post
guid: 'http://glenware.wordpress.com/?p=137'
permalink: /spring-4-simple-example/
geo_public:
    - '0'
thrive_post_fonts:
    - '[]'
categories:
    - Spring
tags:
    - Spring3
    - Spring4
    - Tutorial
---

The tutorial provides a simple Spring 4 application. The application demonstrates how Spring can be used to “inject” the Bicycle choice to our Cyclist object through dependency injection.

The code can be downloaded from – [www.github.com/farrelmr/spring4DI](www.github.com/farrelmr/spring4DI "www.github.com/farrelmr/spring4DI")

An installation guide for this code is available at [Spring 4 Simple Example Installation](http://www.javabullets.com/2014/07/22/spring-4-simple-example-installation/)

The project layout is –

![Spring4 DI Directory Structure](http://glenware.files.wordpress.com/2014/07/directorylayout.png)

To run the code run the RunMe application

You can rebuild this code for Spring 3 by amending the pom.xml file –

\[sourcecode lang=”xml”\] &lt;spring.version&gt;4.0.6.RELEASE&lt;/spring.version&gt;  
\[/sourcecode\] **Model**

The Model objects in our Spring 4 universe are –

- Cyclist
- RacingBike implements Bicycle
- MountainBike implements Bicycle

**Without Dependency Injection(DI)**

If we were to implement this code without DI it might look like this –

\[sourcecode lang=”java”\] package com.glenware.spring4; import org.springframework.context.ApplicationContext;  
import org.springframework.context.support.ClassPathXmlApplicationContext;

public class RunMe {  
 public static void main(String\[\] args) {  
 Cyclist roadCyclist = new Cyclist();  
 roadCyclist.setBicycle(new RoadBicycle());  
 roadCyclist.setCyclingSong("I want to ride my bicycle, I want to ride my bike");  
 System.out.println(roadCyclist.getCyclingSong());  
 roadCyclist.getBicycle().wotYouRiding();  
 }  
}  
\[/sourcecode\]

Giving an output –

*I want to ride my bicycle, I want to ride my bike*  
*Im riding my road bike*

**With Dependency Injection(DI)**

On a small scale this code is fine – but as the code base grows having the setters repeatedly add bloat to the code, something which DI removes.

The model does not have to change for this example, instead we configure the Cyclist and Bicycle’s through the Spring 4 xml configuration –

\[sourcecode lang=”xml”\] &lt;beans xmlns="http://www.springframework.org/schema/beans"  
 xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"  
 xsi:schemaLocation="http://www.springframework.org/schema/beans  
 http://www.springframework.org/schema/beans/spring-beans-4.0.xsd"&gt;  &lt;bean id="roadBike" class="com.glenware.spring4.RoadBicycle" /&gt;  
 &lt;bean id="roadCyclist" class="com.glenware.spring4.Cyclist"&gt;  
 &lt;property name="cyclingSong" value="I want to ride my bicycle, I want to ride my bike" /&gt;  
 &lt;property name="bicycle" ref="roadBike" /&gt;  
 &lt;/bean&gt;

 &lt;bean id="mountainBike" class="com.glenware.spring4.MountainBicycle" /&gt;  
 &lt;bean id="offroadCyclist" class="com.glenware.spring4.Cyclist"&gt;  
 &lt;property name="cyclingSong" value="I run around town, around round the round with the pedal to the met…. The pedal to whatever" /&gt;  
 &lt;property name="bicycle" ref="mountainBike" /&gt;  
 &lt;/bean&gt;

&lt;/beans&gt;  
\[/sourcecode\]

The key construct here is the allocation of properties, where instead of using the setters in the code, we use the property fields. Note there are two types of property – value which refers to a value object(eg. String), or ref which refers to a complex object.

The code to execute –

\[sourcecode lang=”java”\] package com.glenware.spring4; import org.springframework.context.ApplicationContext;  
import org.springframework.context.support.ClassPathXmlApplicationContext;

public class RunMe {  
 public static void main(String\[\] args) {  
 @SuppressWarnings("resource")  
 ApplicationContext context = new ClassPathXmlApplicationContext("springCyclingBeans.xml");

 Cyclist roadCyclist = (Cyclist) context.getBean("roadCyclist");  
 System.out.println(roadCyclist.getCyclingSong());  
 roadCyclist.getBicycle().wotYouRiding();

 Cyclist offroadCyclist = (Cyclist) context.getBean("offroadCyclist");  
 System.out.println(offroadCyclist.getCyclingSong());  
 offroadCyclist.getBicycle().wotYouRiding();  
 }  
}  
\[/sourcecode\]

**Output**

*I want to ride my bicycle, I want to ride my bike*  
*Im riding my road bike*  
*I run around town, around round the round with the pedal to the met…. The pedal to whatever*  
*Im riding my mountain bike*

The new code calls the ApplicationContext. This loads handles our object creation, and lifecycle.

To call the specific implementation we simply request the name we gave the bean in the spring configuration file –

\[sourcecode lang=”java”\] Cyclist offroadCyclist = (Cyclist) context.getBean("offroadCyclist");  
\[/sourcecode\] 