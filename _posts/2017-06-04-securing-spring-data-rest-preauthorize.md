---
id: 2230
title: 'Securing Spring Data REST with PreAuthorize'
date: '2017-06-04T20:18:03+00:00'
author: 'Martin Farrell'
layout: post
guid: 'https://www.javabullets.com/?p=2230'
permalink: /securing-spring-data-rest-preauthorize/
thrive_post_fonts:
    - '[]'
categories:
    - 'Spring Boot'
    - 'Spring Data'
    - spring-security
tags:
    - REST
    - 'Spring Boot'
    - 'spring data REST'
    - 'Spring Security'
---

Securing Spring Data REST with PreAuthorize is an alternative method to securing Spring Data REST API’s, building on the previous apporach covered in [Spring Security and Spring Data REST](https://www.javabullets.com/spring-security-spring-data-rest/).

# Securing Spring Data REST with Preauthorize

Securing Spring Data REST with PreAuthorize code is available on github –

<https://github.com/farrelmr/introtospringdatarest/tree/3.0.0>

<https://github.com/farrelmr/introtospringdatarest/releases/tag/3.0.0>

Run the code by typing –

```
mvnw spring-boot:run
```

# Maven

In order to use the preAuthorise attributes you need to import spring-boot-security –

```
<dependency>
<groupId>org.springframework.boot</groupId>
<artifactId>spring-boot-starter-security</artifactId>
</dependency>
```

# SecurityConfig

The key points are –

- Maintain security for roles “ADMIN” and “USER” on “/rest/\*\*” requests
- @EnableGlobalMethodSecurity(prePostEnabled = true) – this enables the annotations on the JPA model

```
<pre class="prettyprint">package com.javabullets.springdata.jparest;

import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.security.config.annotation.authentication.builders.AuthenticationManagerBuilder;
import org.springframework.security.config.annotation.web.builders.HttpSecurity;
import org.springframework.security.config.annotation.web.configuration.EnableWebSecurity;
import org.springframework.security.config.annotation.web.configuration.WebSecurityConfigurerAdapter;
import org.springframework.security.config.annotation.method.configuration.EnableGlobalMethodSecurity;

import org.springframework.http.HttpMethod;

@EnableWebSecurity
@EnableGlobalMethodSecurity(prePostEnabled = true)
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
            .antMatchers("/rest/**").hasAnyRole("ADMIN","USER").and()
          .httpBasic()
            .and()
		   .csrf().disable();		   
    }
}
```

## ParkrunCourseRepository

The key changes are the use of PreAuthorize on the ParkrunCourseRepository –

```
<pre class="prettyprint">package com.javabullets.springdata.jparest;

import org.springframework.data.repository.CrudRepository;
import org.springframework.security.access.prepost.PreAuthorize;

@PreAuthorize("hasRole('ROLE_USER')")
public interface ParkrunCourseRepository extends CrudRepository<ParkrunCourse, Long> {
	@Override
	@PreAuthorize("hasRole('ROLE_ADMIN')")
	ParkrunCourse save(ParkrunCourse parkrunCourse);
}
```

The key points are –

- Use of hasRole – note you need to append ROLE\_ to your roles
- We could also add PreAuthorize security to custom methods defined in the repository

## Putting It Together

```
mvnw spring-boot:run
```

## POST methods – Access Forbidden

```
curl -u user:user -X POST -H "Content-Type:application/json" -d "{  \"courseName\" : \"adminOnly\",  \"url\" : \"url\",  \"averageTime\" : \"10000\" }" http://localhost:8080/rest/parkrunCourses

{"timestamp":1496134011261,"status":403,"error":"Forbidden","message":"Access is denied","path":"/rest/parkrunCourses"}
```

## POST methods – Access Allowed

```
curl -u admin:admin -X POST -H "Content-Type:application/json" -d "{  \"courseName\" : \"adminOnly\",  \"url\" : \"url\",  \"averageTime\" : \"10000\" }" http://localhost:8080/rest/parkrunCourses

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

# Discussion

This post shows a different approach to role based security of Spring Data REST with PreAuthorize. It is not a question of which method is better, but which is practical. It may be that you cannot use preAuthorize in your codebase, but can use role based authentication as outlined in [Spring Security and Spring Data REST](https://www.javabullets.com/spring-security-spring-data-rest/)

From a design point of view you may want to define a custom repository with your core PreAuthorize rules, allowing your model to inherit security.

# Conclusions

This post shows how Spring Security and Spring Data REST can be combined to secure REST API URL’s and HTTP methods. It used a basic form of Spring authentication, combining a MemoryRealm with the security configuration. We have also demonstrated how to restrict access to REST methods based on user group.

My next post will look at how Spring Data REST can restrict access by deciding what methods it exposes, and what fields are exposed