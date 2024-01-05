---
id: 2109
title: 'parkrunPB &#8211; Spring 3 and Spring MVC &#8211; Part 1'
date: '2014-08-14T20:30:10+00:00'
author: 'Martin Farrell'
layout: post
guid: 'http://glenware.wordpress.com/?p=183'
permalink: /parkrunpb-spring-3-and-spring-mvc-part-1/
geo_public:
    - '0'
thrive_post_fonts:
    - '[]'
categories:
    - Java
tags:
    - parkrunPB
    - Spring
    - 'spring MVC'
    - Spring3
    - Spring4
---

Model-View-Controller(MVC) is a software pattern for user interfaces. It encourages clean design by separating out the three components of the Graphical User Interface (GUI) into three components – Model,  
View and Controller

[![springmvc](http://glenware.files.wordpress.com/2014/08/springmvc.png?w=300&resize=596%2C390)](https://glenware.files.wordpress.com/2014/08/springmvc.png)

It consists of 3 components –

- Controller – This is normally Front Controller, which delegates to specific controls
- Model – The controller updates the model
- View – The controller then decides which view to return

The flow is sometimes easier to see in a sequence diagram –

![mvcssd](http://glenware.files.wordpress.com/2014/08/mvcssd.jpg?w=300&resize=592%2C292)

**Configuration**

**Front Controller – web.xml**

This is the starting point for configuration –

```
  <display-name>parkrunPB</display-name>
  <servlet>
    <servlet-name>spring</servlet-name>
    <servlet-class>
       org.springframework.web.servlet.DispatcherServlet
    </servlet-class>
    <load-on-startup>1</load-on-startup>
  </servlet>
  <servlet-mapping>
    <servlet-name>spring</servlet-name>
    <url-pattern>/</url-pattern>
  </servlet-mapping>
```

Here the DispatcherServlet is the Front Controller

**Spring Configuration – spring-servlet.xml**

This configuration is using the default name for the spring configuration file which matches the servlet name with a -servlet.xml appended – *spring-servlet.xml*

The key areas of this configuration are –

```
   <context:component-scan base-package="com.glenware.parkrunpb" />
   <context:annotation-config />
   <mvc:annotation-driven/>

   <bean id="jspViewResolver"
         class="org.springframework.web.servlet.view.InternalResourceViewResolver">
      <property name="viewClass" value="org.springframework.web.servlet.view.JstlView" />
      <property name="prefix" value="/WEB-INF/jsp/" />
      <property name="suffix" value=".jsp" />
   </bean>

   <bean id="messageSource"
         class="org.springframework.context.support.ReloadableResourceBundleMessageSource">
      <property name="basename" value="classpath:messages" />
      <property name="defaultEncoding" value="UTF-8" />
   </bean>
```

In spring terms we’re basically –

- Setting up annotation driven configuration on package com.glenware.parkrunpb
- ViewResolver – check /WEB-INF/jsp for files ending in .jsp
- Messages – check classpath for messages.properties

   
 **AboutController – Simple Example**

The simplest controller in parkrunPB is the AboutController –

```
@Controller
public class AboutController {

   @RequestMapping("/about")
   public String about() {
      return "about";
   }

}
```

 The key attributes of this controller are –

- @Controller – Marks the class as a Controller
- @RequestMapping(“/about”) – Method is called when /parkrunPB/about is called
- return “about” – This is where the view resolver kicks in an searches for the corresponding view – /WEB-INF/jsp/about.jsp

**References**

Images courtesy of (any issues and they will be removed/replaced) –

http://docs.spring.io/spring-framework/docs/3.0.x/reference/mvc.html

http://java4developers.com/2011/spring-mvc-basic-example-with-maven/

http://www.theserverside.com/news/1364355/Ajax-and-the-Spring-Framework-with-TIBCO-General-Interface