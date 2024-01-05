---
id: 2115
title: 'Auto-generating Spring Security Tutorial – Custom JDBC Realms'
date: '2016-11-11T09:20:09+00:00'
author: 'Martin Farrell'
layout: post
guid: 'https://glenware.wordpress.com/?p=923'
permalink: /auto-generating-spring-security-tutorial-custom-jdbc-realms/
thrive_post_fonts:
    - '[]'
categories:
    - Java
    - Spring
    - spring-security
---

This post builds on the set of spring-security posts I have done, and particularly my last post on [Default JDBC Realms](https://www.javabullets.com/2016/11/07/auto-generating-spring-security-tutorial-default-jdbc-realms/). The code is available on [github](https://github.com/farrelmr/parkrunpbreboot), and [spring-security-generator](http://www.glenware.com/spring-security-generator/) and the instructions to run the application are contained in the previous tutorial.

We also have a requirement to use a custom JDBC realm with the structure –

| USER | PASSWORD | ROLES |
|---|---|---|
| customadmin | customadmin | ROLE\_CUSTOM\_ADMIN |

## Custom JDBC Tables

```
The tables Ive used in this example are renamed versions of the default tables -

create table custom_users (
  username varchar(256),
  password varchar(256),
  enabled boolean
);
create table custom_authorities (
  username varchar(256),
  authority varchar(256)
);
```

```
With inserts - 

insert into custom_users (username, password, enabled) values ('customadmin', 'customadmin', true);
insert into custom_authorities (username, authority) values ('customadmin', 'ROLE_CUSTOM_ADMIN');
```

## Spring-Security-Generator

Using [spring-security-generator](http://www.glenware.com/spring-security-generator/) we now select “JDBC Realm (Custom)” and supply the queries –

```
select username, password, enabled from custom_users where username = ?
select username, authority from custom_authorities where username = ?
```

The screen configuration is then –

![screen-shot-2016-11-07-at-21-00-50](https://glenware.files.wordpress.com/2016/11/screen-shot-2016-11-07-at-21-00-50.png?resize=1262%2C2445)

We then generate the code –

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
                   .usersByUsernameQuery(
                   "select username, password, enabled from custom_users where username = ?")
                   .authoritiesByUsernameQuery(
                   "select username, authority from custom_authorities where username = ?");
    }

    @Override
    protected void configure(HttpSecurity http) throws Exception {
        http
            .authorizeRequests()
                .antMatchers("/webjars/**","/about.html","/rest/**").permitAll()
                .antMatchers("/admin/**").hasAnyRole("CUSTOM_ADMIN")
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

The key difference between this tutorial and the previous is that we supply the SQL directly to the usersByUsernameQuery and authoritiesByUsernameQuery

We can now copy this code to the parkrunpb applicaiton, and login using customadmin/customadmin