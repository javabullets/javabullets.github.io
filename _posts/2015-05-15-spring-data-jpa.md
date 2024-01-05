---
id: 435
title: 'Spring Data JPA'
date: '2015-05-15T14:22:04+00:00'
author: 'Martin Farrell'
layout: post
guid: 'https://glenware.wordpress.com/?p=435'
permalink: /spring-data-jpa/
thrive_post_fonts:
    - '[]'
categories:
    - Uncategorized
---

The DAO pattern is one of the most common patterns I see in JPA. It’s a good pattern, and is generally implemented in the form set out in this article dzone. It’s a good pattern, but the issue I have in development is that I end up writing boilerplate code to provide the same features.

This is where Spring Data JPA steps in and frees you up from writing DAO implementations.

**Configuration**

Consider the following JPA entity –

\[sourcecode lang=”java”\] package com.glenware.entity; @Entity  
@Table(name = "PARKRUN\_COURSE")  
public class ParkrunCourse implements Serializable {  
 private static final long serialVersionUID = 1L;

 @Id  
 @NotNull  
 @Column(name = "PARKRUN\_COURSE\_ID")  
 private Long parkrunCourseId;

 @Size(max = 200)  
 @Column(name = "NAME")  
 private String name;

 @Size(max = 200)  
 @Column(name = "DESCRIPTION")  
 private String description;

 // Getters And Setters Not shown

}  
\[/sourcecode\]

Steps –

1\. Model pom.xml –

\[sourcecode lang=”xml”\] &lt;dependency&gt;  
 &lt;groupId&gt;org.springframework.data&lt;/groupId&gt;  
 &lt;artifactId&gt;spring-data-jpa&lt;/artifactId&gt;  
 &lt;version&gt;1.2.0.RELEASE&lt;/version&gt;  
 &lt;/dependency&gt;  
\[/sourcecode\] 2\. Create a implementation of the repository interface for –

\[sourcecode lang=”java”\] package com.glenware.repository; import org.springframework.data.repository.CrudRepository;

public interface ParkrunCourseRepository extends CrudRepository&lt;ParkrunCourse, Long&gt; {  
}  
\[/sourcecode\]

Amend your Spring file –

\[sourcecode lang=”xml”\] &lt;?xml version="1.0" encoding="UTF-8" standalone="no"?&gt;  
&lt;beans xmlns="http://www.springframework.org/schema/beans"  
 xmlns:context="http://www.springframework.org/schema/context"  
 xmlns:tx="http://www.springframework.org/schema/tx"  
 xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"  
 xmlns:p="http://www.springframework.org/schema/p"  
 xmlns:jpa="http://www.springframework.org/schema/data/jpa"  
 xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans-3.2.xsd  
 http://www.springframework.org/schema/tx http://www.springframework.org/schema/tx/spring-tx-3.2.xsd  
 http://www.springframework.org/schema/context http://www.springframework.org/schema/context/spring-context-3.2.xsd  
 http://www.springframework.org/schema/data/jpa  
 http://www.springframework.org/schema/data/jpa/spring-jpa.xsd"&gt; &lt;jpa:repositories base-package=" com.glenware.repository" /&gt;

&lt;/beans&gt;  
\[/sourcecode\]

3\. Service – Create method and implementation to access this repository from the Service layer

\[sourcecode lang=”java”\] package com.glenware.service; public interface ParkrunCourseService {  
 long getTotalParkrunCourses();  
}  
\[/sourcecode\]

Create Implementation –

\[sourcecode lang=”java”\] package com.glenware.service; @Service  
public class ParkrunCourseServiceImpl implements ParkrunCourseService {

 @Autowired  
 private ParkrunCourseRepository parkrunCourseRepository;

 @Override  
 public long getTotalParkrunCourses () {  
 return parkrunCourseRepository.count();  
 }

}  
\[/sourcecode\]

[CrudRepository](http://docs.spring.io/spring-data/commons/docs/current/api/org/springframework/data/repository/CrudRepository.html "CrudRepository") defines methods for delete, find, and saving JPA objects. There is also a specialist interface for [PagingAndSortingRepository](http://docs.spring.io/spring-data/commons/docs/current/api/org/springframework/data/repository/PagingAndSortingRepository.html)

And its even easier to implement using Spring Boot!