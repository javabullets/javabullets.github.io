---
id: 2203
title: 'Introduction to Spring Data REST'
date: '2017-05-24T21:15:20+00:00'
author: 'Martin Farrell'
layout: post
guid: 'https://www.javabullets.com/?p=2203'
permalink: /spring-data-rest-introduction/
thrive_post_fonts:
    - '[]'
thrive_share_count:
    - '{"facebook":0,"twitter":0,"plusone":2,"pinterest":1,"linkedin":0,"total":3,"last_fetch":1496162485,"url":"https://www.javabullets.com/spring-data-rest-introduction/"}'
categories:
    - JPA
    - REST
    - 'Spring Boot'
    - 'Spring Data'
tags:
    - JPA
    - REST
    - 'Spring Boot'
    - 'Spring Data'
---

This tutorial on Spring Data REST shows how Spring Data repositories can be exposed as a REST API. Its a really interesting idea, and can save you a lot of boilerplate code building microservices.

# Introduction to Spring Data REST

[Spring Boot](https://projects.spring.io/spring-boot/) makes it easy to create a Spring Data REST starter project using [Spring Initializr](https://start.spring.io/) –

<div class="wp-caption aligncenter" id="attachment_2204" style="width: 1034px">![spring initializr](https://i0.wp.com/www.javabullets.com/wp-content/uploads/2017/05/springInitializrdatajpa.png?resize=1024%2C611&ssl=1)Spring Initializr – Spring Boot JPA Microservice

</div>Im using Spring Boot 2.0.0, and the HAL Browser to quickly demonstrate REST API.

Download, expand and import into your IDE –

<div class="wp-caption aligncenter" id="attachment_2205" style="width: 310px">![Expanded Spring Boot Project](https://i0.wp.com/www.javabullets.com/wp-content/uploads/2017/05/springdataresttree.png?resize=300%2C294&ssl=1)Expanded Spring Boot Project

</div># Code

For a quickstart the code is available on github at – <https://github.com/farrelmr/introtospringdatarest/tree/1.0.0>

Let’s keep this simple and use the classes from my parkrunpb project.

The starting point is the pom.xml –

```
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
    xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>

    <groupId>com.javabullets.springdata</groupId>
    <artifactId>jparest</artifactId>
    <version>0.0.1-SNAPSHOT</version>
    <packaging>jar</packaging>

    <name>jparest</name>
    <description>Spring Data Rest Example</description>

    <parent>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-parent</artifactId>
        <version>2.0.0.BUILD-SNAPSHOT</version>
        <relativePath/> <!-- lookup parent from repository -->
    </parent>

    <properties>
        <project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
        <project.reporting.outputEncoding>UTF-8</project.reporting.outputEncoding>
        <java.version>1.8</java.version>
    </properties>

    <dependencies>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-data-jpa</artifactId>
        </dependency>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-data-rest</artifactId>
        </dependency>
        <dependency>
            <groupId>org.springframework.data</groupId>
            <artifactId>spring-data-rest-hal-browser</artifactId>
        </dependency>
        <dependency>
            <groupId>com.h2database</groupId>
            <artifactId>h2</artifactId>
            <scope>runtime</scope>
        </dependency>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-test</artifactId>
            <scope>test</scope>
        </dependency>
    </dependencies>

    <build>
        <plugins>
            <plugin>
                <groupId>org.springframework.boot</groupId>
                <artifactId>spring-boot-maven-plugin</artifactId>
            </plugin>
        </plugins>
    </build>

    <repositories>
        <repository>
            <id>spring-snapshots</id>
            <name>Spring Snapshots</name>
            <url>https://repo.spring.io/snapshot</url>
            <snapshots>
                <enabled>true</enabled>
            </snapshots>
        </repository>
        <repository>
            <id>spring-milestones</id>
            <name>Spring Milestones</name>
            <url>https://repo.spring.io/milestone</url>
            <snapshots>
                <enabled>false</enabled>
            </snapshots>
        </repository>
    </repositories>

    <pluginRepositories>
        <pluginRepository>
            <id>spring-snapshots</id>
            <name>Spring Snapshots</name>
            <url>https://repo.spring.io/snapshot</url>
            <snapshots>
                <enabled>true</enabled>
            </snapshots>
        </pluginRepository>
        <pluginRepository>
            <id>spring-milestones</id>
            <name>Spring Milestones</name>
            <url>https://repo.spring.io/milestone</url>
            <snapshots>
                <enabled>false</enabled>
            </snapshots>
        </pluginRepository>
    </pluginRepositories>
</project>
```

So we have our JPA object –

```
<pre class="prettyprint">package com.javabullets.springdata.jparest;

import javax.persistence.Entity;
import javax.persistence.GeneratedValue;
import javax.persistence.Id;

@Entity
public class ParkrunCourse {
@Id
@GeneratedValue(strategy=GenerationType.IDENTITY)
private Long id;

private String courseName;
private String url;
private Long averageTime;

public String getCourseName() {
return courseName;
}
public void setCourseName(String courseName) {
this.courseName = courseName;
}
public String getUrl() {
return url;
}
public void setUrl(String url) {
this.url = url;
}
public Long getAverageTime() {
return averageTime;
}
public void setAverageTime(Long averageTime) {
this.averageTime = averageTime;
}
}
```

Im using JPARepository –

```
<pre class="prettyprint">package com.javabullets.springdata.jparest;

import org.springframework.data.jpa.repository.JpaRepository;
import org.springframework.data.repository.query.Param;
import org.springframework.data.rest.core.annotation.RepositoryRestResource;

public interface ParkrunCourseRepository extends JpaRepository<ParkrunCourse, Long> {
}
```

And a script to ensure the data is preloaded – stored in (src\\main\\resources\\import.sql) –

```
<pre class="prettyprint">INSERT INTO PARKRUN_COURSE(ID, COURSE_NAME, URL, AVERAGE_TIME) VALUES (1, 'Inverness', 'http://www.parkrun.org.uk/inverness/', 1582);
INSERT INTO PARKRUN_COURSE(ID, COURSE_NAME, URL, AVERAGE_TIME) VALUES (2, 'Aberdeen',    'http://www.parkrun.org.uk/aberdeen/', 1586);
INSERT INTO PARKRUN_COURSE(ID, COURSE_NAME, URL, AVERAGE_TIME) VALUES (3, 'Dundee(Camperdown)', 'http://www.parkrun.org.uk/camperdown/', 1752);
INSERT INTO PARKRUN_COURSE(ID, COURSE_NAME, URL, AVERAGE_TIME) VALUES (4, 'St Andrews', 'http://www.parkrun.org.uk/standrews/', 1669);
INSERT INTO PARKRUN_COURSE(ID, COURSE_NAME, URL, AVERAGE_TIME) VALUES (5, 'Perth', 'http://www.parkrun.org.uk/perth/', 1620);
INSERT INTO PARKRUN_COURSE(ID, COURSE_NAME, URL, AVERAGE_TIME) VALUES (6, 'Edinburgh', 'http://www.parkrun.org.uk/edinburgh/', 1523);
```

# Running the Code

Run the code using the maven wrapper – mvnw –

```
mvnw spring-boot:run
```

We can then access the HALBrowser on –

<http://localhost:8080/browser/index.html#http://localhost:8080/parkrunCourses>

<div class="wp-caption aligncenter" id="attachment_2207" style="width: 1034px">![Spring Data REST - HALBrowser](https://i0.wp.com/www.javabullets.com/wp-content/uploads/2017/05/halbrowser2.png?resize=1024%2C611&ssl=1)Spring Data REST – HALBrowser

</div>The response body returns all the parkrunCourse. You can also do GET, POST, PUT, PATCH and DELETE.

A good trick is to use the browser integrated SQL editor for the H2 database. I covered this in my [spring boot security tutorial](https://www.javabullets.com/auto-generating-spring-security-accessing-the-in-memory-database/).

# <span class="col-11 text-gray-dark mr-2">hal+json media type </span>

Spring Data JPA allows you to make calls as json or hal+json. The purpose of [hal+json](http://stateless.co/hal_specification.html) makes it easier to navigate API’s following links. The HALBrowser is great for quickly checking your API.

# Conclusions

This example shows how a spring data repository can easily be exposed as a REST API. My next post will look at the practicality of this approach, and what restrictions you might want to apply to your API.