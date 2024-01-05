---
id: 2222
title: 'Spring Security and Spring Data REST'
date: '2017-05-29T21:05:40+00:00'
author: 'Martin Farrell'
layout: post
guid: 'https://www.javabullets.com/?p=2222'
permalink: /spring-security-spring-data-rest/
thrive_post_fonts:
    - '[]'
thrive_share_count:
    - '{"facebook":0,"twitter":0,"plusone":1,"pinterest":0,"linkedin":0,"total":1,"last_fetch":1496554071,"url":"https://www.javabullets.com/spring-security-spring-data-rest/"}'
categories:
    - 'Spring Boot'
    - 'Spring Data'
    - spring-security
tags:
    - 'Spring Boot'
    - 'Spring Data'
    - 'Spring Data JPA'
    - 'Spring REST'
    - 'Spring Security'
---

Spring Security and Spring Data REST are discussed in this post. It builds on the previous post [“Introduction to Spring Data REST”](https://www.javabullets.com/spring-data-rest-introduction/) and my series of posts on Spring Security –

- [Can Spring Security be auto-generated?](https://www.javabullets.com/can-we-auto-generate-spring-security/)
- [Auto-Generating Spring Security: Accessing the In-memory Database](https://www.javabullets.com/auto-generating-spring-security-accessing-the-in-memory-database/)

# Spring Security and Spring Data REST

Spring Security is split into two components –

- Authentication – Defined by AuthenticationManager, or the source of the authentication credentials
- Authorisation – what we want to protect – URL’s, Roles, method

This tutorial is only considering BasicAuthentication with an memory realm. I will probably return to this subject to show how to properly harden a RESTful API.

# Source Code

Code available on github –

<https://github.com/farrelmr/introtospringdatarest/tree/2.0.0>

<https://github.com/farrelmr/introtospringdatarest/releases/tag/2.0.0>

Run the code by typing –

```
mvnw spring-boot:run
```

# Maven

You can secure spring boot by simply including this dependency –

```
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-security</artifactId>
        </dependency>
```

You can then get the password when you startup spring boot, but that is not very practical for most usages.

## SecurityConfig

Ive moved the Spring Data REST API URL to /rest in application.properties –

```
spring.data.rest.basePath=/rest
```

SecurityConfig has –

- Two users – user(Role – USER), admin(Role – admin)
- Restrictions – 
    - Only “ADMIN” or “USER” roles can access “/rest”,
    - Only “ADMIN” users can POST to the web service –

```
<pre class="prettyprint">package com.javabullets.springdata.jparest;

import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.security.config.annotation.authentication.builders.AuthenticationManagerBuilder;
import org.springframework.security.config.annotation.web.builders.HttpSecurity;
import org.springframework.security.config.annotation.web.configuration.EnableWebSecurity;
import org.springframework.security.config.annotation.web.configuration.WebSecurityConfigurerAdapter;
import org.springframework.http.HttpMethod;

@EnableWebSecurity
public class SecurityConfig extends WebSecurityConfigurerAdapter {
    @Autowired
    public void configureGlobal(AuthenticationManagerBuilder auth)
            throws Exception {
        auth.
            inMemoryAuthentication()
                .withUser("user").password("user").roles("USER").and()
                .withUser("admin").password("admin").roles("USER","ADMIN");
    }

    @Override
    protected void configure(HttpSecurity http) throws Exception {
        http
          .authorizeRequests()
            .antMatchers(HttpMethod.POST, "/rest/parkrunCourses/**").hasRole("ADMIN")
            .antMatchers("/rest/**").hasAnyRole("ADMIN","USER").and()
          .httpBasic()
            .and()
           .csrf().disable();
    }
}
```

## Putting It Together

```
mvnw spring-boot:run
```

Ive switched to [curl](https://curl.haxx.se/) for calling the API’s as its clearer for tutorials –

### No credentials – Doesnt Authenticate

```
curl  -X GET -H "Content-Type:application/json" http://localhost:8080/rest/parkrunCourses/1
{"timestamp":1496069649024,"status":401,"error":"Unauthorized","message":"Full authentication is required to access this resource","path":"/rest/parkrunCourses/1"}
```

### user/user credentials – Authenticates

```
curl -u user:user -X GET -H "Content-Type:application/json" http://localhost:8080/rest/parkrunCourses/1
{
  "courseName" : "Inverness",
  "url" : "http://www.parkrun.org.uk/inverness/",
  "averageTime" : 1582,
  "_links" : {
    "self" : {
      "href" : "http://localhost:8080/rest/parkrunCourses/1"
    },
    "parkrunCourse" : {
      "href" : "http://localhost:8080/rest/parkrunCourses/1"
    }
  }
}
```

### POST methods – Access Forbidden

```
curl -u user:user -X POST -H "Content-Type:application/json" -d "{  \"courseName\" : \"adminOnly\",  \"url\" : \"url\",  \"averageTime\" : \"10000\" }" http://localhost:8080/rest/parkrunCourses
{"timestamp":1496069812087,"status":403,"error":"Forbidden","message":"Access is denied","path":"/rest/parkrunCourses"}
```

### POST methods – Access Allowed

```
curl -u admin:admin -X POST -H "Content-Type:application/json" -d "{  \"courseName\" : \"adminOnly\",  \"url\" : \"url\",  \"averageTime\" : \"10000\" }" http://localhost:8080/rest/parkrunCourses
{
  "courseName" : "adminOnly",
  "url" : "url",
  "averageTime" : 10000,
  "_links" : {
    "self" : {
      "href" : "http://localhost:8080/rest/parkrunCourses/13"
    },
    "parkrunCourse" : {
      "href" : "http://localhost:8080/rest/parkrunCourses/13"
    }
  }
}
```

## Conclusions

This post shows how Spring Security and Spring Data REST can be combined to secure REST API URL’s and HTTP methods. It used a basic form of Spring authentication, combining a MemoryRealm with the security configuration. We have also demonstrated how to restrict access to REST methods based on user group.