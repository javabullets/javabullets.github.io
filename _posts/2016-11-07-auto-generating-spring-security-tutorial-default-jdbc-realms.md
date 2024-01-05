---
id: 887
title: 'Auto-generating Spring Security Tutorial – Default JDBC Realms'
date: '2016-11-07T21:26:17+00:00'
author: 'Martin Farrell'
layout: post
guid: 'https://glenware.wordpress.com/?p=887'
permalink: /auto-generating-spring-security-tutorial-default-jdbc-realms/
thrive_post_fonts:
    - '[]'
categories:
    - Java
    - Spring
    - spring-security
---

The previous [tutorial](https://www.javabullets.com/2016/10/09/auto-generating-spring-security-tutorial-memory-realms/) showed how we can auto-generate of spring security using a memory realm. This tutorial expands on this to cover Default JDBC Realms using the source code from the [parkrunPB](https://github.com/farrelmr/parkrunpbreboot) application

## Security Requirements

The site has the following links and security requirements –

| http://localhost:8080/ | Accessible to all |
|---|---|
| http://localhost:8080/webjars | Static Resources – Accessible to all |
| http://localhost:8080/about.html | Static page – Accessible to all |
| http://localhost:8080/login.html | Accessible to all |
| http://localhost:8080/admin/ | Admin User |
| http://localhost:8080/rest | Accessible to all |

We also have a requirement to use a users and roles with the structure –

| USER | PASSWORD | ROLES |
|---|---|---|
| admin | admin | ADMIN |

## Getting Started

The first thing we need to do is uncomment spring security in the maven pom –

```
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-security</artifactId>
        </dependency>
```

We can then compile and run the code –

```
mvn spring-boot:run
```

The whole application is now locked down

Luckily we can login using the default username (user), and the password from the logs. Im my case –

```
2016-11-06 21:16:56.877 INFO 8088 --- [main] b.a.s.AuthenticationManagerConfiguration :
 Using default security password: e1c87658-8b7e-4b1e-88da-902b5356ef66
```

## Default JDBC Tables

We can now begin to create our SecurityConfiguration using [Spring Security Generator](http://www.glenware.com/spring-security-generator)

![screen-shot-2016-11-06-at-21-33-53](https://glenware.files.wordpress.com/2016/11/screen-shot-2016-11-06-at-21-33-53.png?resize=1262%2C2461)

We then get the generated source code –

```
package com.glenware.springboot;

import javax.sql.DataSource;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.security.config.annotation.authentication.builders.AuthenticationManagerBuilder;
import org.springframework.security.config.annotation.web.builders.HttpSecurity;
import org.springframework.security.config.annotation.web.configuration.EnableWebSecurity;
import org.springframework.security.config.annotation.web.configuration.WebSecurityConfigurerAdapter;

@EnableWebSecurity
public class SecurityConfig extends WebSecurityConfigurerAdapter {

	@Autowired
	private DataSource dataSource;

	@Autowired
	public void configureGlobal(AuthenticationManagerBuilder auth)
			throws Exception {
         auth
             .jdbcAuthentication()
                 .dataSource(dataSource)
                     .withDefaultSchema()
				.withUser("admin").password("admin").roles("ADMIN");
	}

    @Override
    protected void configure(HttpSecurity http) throws Exception {
        http
            .authorizeRequests()
                .antMatchers("/webjars/*","/about.html","/rest/**").permitAll()
                .antMatchers("/admin/**").hasAnyRole("ADMIN")
                .anyRequest().authenticated()
            .and()
                .formLogin()
                    .loginPage("/login")
                    .defaultSuccessUrl("/admin/admin.html")
                    .failureUrl("/login")
                    .permitAll()
             .and()
                .logout()
                    .logoutSuccessUrl("/")
                    .permitAll()
                    ;
	}

}
```

## Key Points

- Using JDBC Realm(Default) – The default realm means Spring Security will use its default [users.ddl](https://github.com/spring-projects/spring-security/blob/master/core/src/main/resources/org/springframework/security/core/userdetails/jdbc/users.ddl)
- Same configuration as before

We can now access the site the same as the memory realm, but with user details stored in the database. The next post will look at using a custom JDBC table