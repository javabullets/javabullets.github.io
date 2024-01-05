---
id: 773
title: 'Auto-generating Spring Security Tutorial &#8211; Memory Realms'
date: '2016-10-09T21:41:18+00:00'
author: 'Martin Farrell'
layout: post
guid: 'https://glenware.wordpress.com/?p=773'
permalink: /auto-generating-spring-security-tutorial-memory-realms/
thrive_post_fonts:
    - '[]'
categories:
    - Spring
    - 'Spring Security'
tags:
    - Spring
    - 'Spring Security'
---

Auto-generating Spring Security Tutorial – Memory Realms demonstrates how a simple open source project can generate Spring Security configuration using memory realms

I created a demo spring boot application under [github.com/farrelmr/parkrunpbreboot](https://github.com/farrelmr/parkrunpbreboot)

![parkrunpbreboot1](https://glenware.files.wordpress.com/2016/10/parkrunpbreboot1.png?resize=1280%2C564)

The application is simple and allows your to predict your 5km running time based on previous [parkrun](http://www.parkrun.org.uk/) performances. For those who dont know what a parkrun is its a free 5km timed run held weekly in an increasing number of places.

## Security Requirements

The site has the following links and security requirements –

| http://localhost:8080/ | Accessible to all |
|---|---|
| http://localhost:8080/webjars | Static Resources – Accessible to all |
| http://localhost:8080/about.html | Static page – Accessible to all |
| http://localhost:8080/login.html | Accessible to all |
| http://localhost:8080/admin/ | Admin User |
| http://localhost:8080/rest | Accessible to all |

We also have a requirement to use a memory realm with the structure –

| USER | PASSWORD | ROLES |
|---|---|---|
| admin | admin | admin |

## Getting Started

The first thing we need to do is uncomment spring security in the maven pom –

```
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-security</artifactId>
        </dependency>
```

We can now begin to create our SecurityConfiguration using –

<http://www.glenware.com/spring-security-generator>

![springsecuritygenerator](https://glenware.files.wordpress.com/2016/10/springsecuritygenerator.png?resize=1263%2C1131)

## Memory Realm with Basic Authentication

The first step is to configure the memory realm. The other security options are Default JDBC, and LDAP, and will be covered in later tutorials

![basicauthenticationspringsec](https://glenware.files.wordpress.com/2016/10/basicauthenticationspringsec.png?resize=1263%2C1150)

The code is available on [gist](https://gist.github.com/anonymous/ac4dce4a9fcfbf9f17bbaea61cf1b0ba)

We can then copy the generated code to com.glenware.springboot.SecurityConfig, and test the application. The whole application is secured, with the password admin/admin.

We now get the default login page –

![login](https://glenware.files.wordpress.com/2016/10/login.png?resize=1280%2C564)

## Fine Tuning

We can now fine tune the requirements –

![screen-shot-2016-10-09-at-21-17-08](https://glenware.files.wordpress.com/2016/10/screen-shot-2016-10-09-at-21-17-08.png?resize=1263%2C2142)

Again the code is available on [gist](https://gist.github.com/anonymous/d1a8927733bd7d9c6c73224710deb803)

This allows free access to the site, except for the admin sections as required. We also now have a formatted login page.

## Conclusions

The above tutorial shows how a menu driven application can automatically and simply provide your spring security. The next areas of work are to improve JDBC and REST functionality