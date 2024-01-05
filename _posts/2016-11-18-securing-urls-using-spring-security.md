---
id: 969
title: 'Securing URLs using Spring Security'
date: '2016-11-18T15:39:14+00:00'
author: 'Martin Farrell'
layout: post
guid: 'https://glenware.wordpress.com/?p=969'
permalink: /securing-urls-using-spring-security/
thrive_post_fonts:
    - '[]'
categories:
    - Java
---

Typically when securing a URL you are looking to do one of the following –

- Allow access to everyone to a given URL
- Secure URL based on roles
- Secure URL based on multiple roles
- Secure URL based on IP Address

This post shows how to do this using spring security

## Specifying URL’s

The most common approach to specifying a URL is through [antMatcher’s](http://docs.spring.io/spring-security/site/docs/current/apidocs/org/springframework/security/config/annotation/web/builders/HttpSecurity.html#antMatcher-java.lang.String-)

So if we want to secure –

| http://www.example.com/static | Open to everyone – css, javascript |
|---|---|
| http://www.example.com/register | Open to everyone |
| http://www.example.com/login | Open to everyone |
| http://www.example.com/user/ | ROLE\_USER or ROLE\_ADMIN – User Area |
| http://www.example.com/admin/ | ROLE\_ADMIN only and restrict on IPADDRESS – Admin Area |

We would simply use –

```
.antMatchers("/register")
```

Or with multiple –

```
.antMatchers("/register","/login","/user","/admin")
```

We also specify individual pages or directories –

```
.antMatchers("register.html"); // Individual
.antMatchers("/admin/**"); // Directory
```

Depending on the complexity of pattern you are securing you can also consider –

- [mvcMatcher](http://docs.spring.io/spring-security/site/docs/current/apidocs/org/springframework/security/config/annotation/web/builders/HttpSecurity.html#mvcMatcher-java.lang.String-)
- [requestMatcher](http://docs.spring.io/spring-security/site/docs/current/apidocs/org/springframework/security/config/annotation/web/builders/HttpSecurity.html#requestMatcher-org.springframework.security.web.util.matcher.RequestMatcher-)
- [regexMatcher](http://docs.spring.io/spring-security/site/docs/current/apidocs/org/springframework/security/config/annotation/web/builders/HttpSecurity.html#regexMatcher-java.lang.String-)

## Securing the URL’s

The methods to secure URL’s are defined in [AuthorizedUrl](http://docs.spring.io/spring-security/site/docs/current/apidocs/org/springframework/security/config/annotation/web/configurers/ExpressionUrlAuthorizationConfigurer.AuthorizedUrl.html)

The most common methods are –

- authenticated() – This is the URL you want to protect, and requires the user to login
- permitAll() – This is used for URL’s with no security applied for example css, javascript
- hasRole(String role) – Restrict to single role. Note that the role will have “ROLE\_” appended. So role=”ADMIN” has a comparison against “ROLE\_ADMIN”. An alternatve is hasAuthority(String authority)
- hasAnyRole(String… roles) – Allows multiple roles. An alternative is hasAnyAuthority(String… authorities)

Other useful methods are –

- access(String attribute) – This method takes SPEL, so you can create more complex restrictions. For those who are interested a lot of the methods in [ExpressionUrlAuthorizationConfigurer.AuthorizedUrl](http://docs.spring.io/spring-security/site/docs/current/apidocs/org/springframework/security/config/annotation/web/configurers/ExpressionUrlAuthorizationConfigurer.AuthorizedUrl.html) ultimately call access with the required SPEL
- hasIpAddress(String ipaddressExpression) – Restrict on IP address or subnet

## Putting it all together

We can put this altogher and create a method like –

```
    @Override
    protected void configure(HttpSecurity http) throws Exception {
      http
        .authorizeRequests()
           .antMatchers("/static","/register").permitAll()
           .antMatchers("/user/**").hasRoles("USER", "ADMIN") // can pass multiple roles
           .antMatchers("/admin/**").access("hasRole('ADMIN') and hasIpAddress('123.123.123.123')") // pass SPEL using access method
           .anyRequest().authenticated()
           .and()
       .formLogin()
           .loginUrl("/login")
           .permitAll();
    }
```

The key points are –

- permitAll gives everyone access to a file or directory
- hasRoles passes multiple roles
- access for more compicated access

As a side note I am currently working on a project to automatically generate this configuration with my [spring-security-generator](http://www.glenware.com/spring-security-generator/)