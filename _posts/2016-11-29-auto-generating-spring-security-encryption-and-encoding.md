---
id: 1067
title: 'Auto-Generating Spring Security : Encryption and Encoding'
date: '2016-11-29T21:44:52+00:00'
author: 'Martin Farrell'
layout: post
guid: 'https://glenware.wordpress.com/?p=1067'
permalink: /auto-generating-spring-security-encryption-and-encoding/
thrive_post_fonts:
    - '[]'
categories:
    - Java
---

It is simple to add security to Spring Security using [spring-security-generator](http://www.glenware.com/spring-security-generator). You simply select an option from the Encryption and Encoding section.

![screenshot4](https://glenware.files.wordpress.com/2016/11/screenshot4.png?resize=1262%2C2672)

The default is No Encryption or Encoding, which is what the previous tutorials have been using.

The other options are –

- None – This is the default
- [BCryptPasswordEncoder ](https://docs.spring.io/spring-security/site/docs/current/apidocs/org/springframework/security/crypto/bcrypt/BCryptPasswordEncoder.html)– BCrypt – The recommended encryption model for Spring Security
- [StandardPasswordEncoder](https://docs.spring.io/spring-security/site/docs/current/apidocs/org/springframework/security/crypto/password/StandardPasswordEncoder.html) – SHA-256
- [NoOpPasswordEncoder](https://docs.spring.io/spring-security/site/docs/current/apidocs/org/springframework/security/crypto/password/NoOpPasswordEncoder.html) – No Encoding or Encryption

You dont want to look beyond BCryptPasswordEncoder, but I’ve included the other options if you want to experiment.

```
The final step is to add some users with the appropriate encryption and encoding to import.xml - 


--
insert into custom_users (username, password, enabled) values ('customadmin', 'customadmin', true);
insert into custom_authorities (username, authority) values ('customadmin', 'ROLE_CUSTOM_ADMIN');
-- NoOpPasswordEncoder - pw - NoOpPasswordEncoder
insert into custom_users (username, password, enabled) values ('NoOpPasswordEncoder', 'NoOpPasswordEncoder', true);
insert into custom_authorities (username, authority) values ('NoOpPasswordEncoder', 'ROLE_CUSTOM_ADMIN');
-- StandardPasswordEncoder - pw - StandardPasswordEncoder
insert into custom_users (username, password, enabled) values ('StandardPasswordEncoder', 'c2ee3d2c461b7d9ea9df6e26f831eccef9418bcc5940132c019b24a484600ada6131eb688a3bc9b8', true);
insert into custom_authorities (username, authority) values ('StandardPasswordEncoder', 'ROLE_CUSTOM_ADMIN');
-- BCryptPasswordEncoder - pw - BCryptPasswordEncoder
insert into custom_users (username, password, enabled) values ('BCryptPasswordEncoder', '$2a$10$RwGDv6rq.T9DE64xH6LSgOx4RV1Xn9lf6ocafwaL6AWrza40PHaMK', true);
insert into custom_authorities (username, authority) values ('BCryptPasswordEncoder', 'ROLE_CUSTOM_ADMIN');
--
```

The username and passwords are the same in all cases, so BCryptPasswordEncoder has a password BCryptPasswordEncoder.

You can now test the Encryption and Encoding options by generating the code. Remember that you can enable the database console using the [previous tutorial](https://www.javabullets.com/2016/11/23/auto-generating-spring-security-accessing-the-in-memory-database/)

```
package com.glenware.springboot;

import javax.sql.DataSource;
import org.springframework.context.annotation.Bean;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.security.config.annotation.authentication.builders.AuthenticationManagerBuilder;
import org.springframework.security.config.annotation.web.builders.HttpSecurity;
import org.springframework.security.config.annotation.web.configuration.EnableWebSecurity;
import org.springframework.security.config.annotation.web.configuration.WebSecurityConfigurerAdapter;

import org.springframework.security.crypto.bcrypt.BCryptPasswordEncoder;
import org.springframework.security.crypto.password.PasswordEncoder;
import org.springframework.security.crypto.password.NoOpPasswordEncoder;
import org.springframework.security.crypto.password.StandardPasswordEncoder;

@EnableWebSecurity
public class SecurityConfig extends WebSecurityConfigurerAdapter {

    @Autowired
    private DataSource dataSource;

    @Bean
    public PasswordEncoder passwordEncoder() {
        return new BCryptPasswordEncoder();
    }
    @Autowired
    public void configureGlobal(AuthenticationManagerBuilder auth)
            throws Exception {
         auth
             .jdbcAuthentication()
                 .passwordEncoder(passwordEncoder())
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