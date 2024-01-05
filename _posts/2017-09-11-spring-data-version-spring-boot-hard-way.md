---
id: 2508
title: 'Spring Data Version From Spring Boot : The Hard Way'
date: '2017-09-11T20:12:00+00:00'
author: 'Martin Farrell'
layout: post
guid: 'https://www.javabullets.com/?p=2508'
permalink: /spring-data-version-spring-boot-hard-way/
thrive_post_fonts:
    - '[]'
image: /wp-content/uploads/2017/09/stefan-kunze-26932.jpg
categories:
    - 'Spring Boot'
    - 'Spring Data'
tags:
    - maven
    - 'Spring Boot'
    - 'Spring Data'
---

Spring Data Version From Spring Boot : The Hard Way, or how to find your Spring Data version from Spring Boot without an IDE.

I recently needed to find my Spring Data version from my Spring Boot maven pom, and used these steps –

1\. Open you Spring Boot project pom.xml –

```

<parent>
<groupId>org.springframework.boot</groupId>
<artifactId>spring-boot-starter-parent</artifactId>
<version>1.5.6.RELEASE</version>
<relativePath/>
</parent>
```

2\. Next stop is the [spring-boot-dependencies](https://github.com/spring-projects/spring-boot/blob/v1.5.6.RELEASE/spring-boot-dependencies/pom.xml) for version 1.5.6.RELEASE

3\. Find spring-data-releasetrain.version in pom.xml –

```

<spring-data-releasetrain.version>Ingalls-SR6</spring-data-releasetrain.version>
```

4\. Track this version against the spring-data-commons [wiki]("https://github.com/spring-projects/spring-data-commons/wiki/Release-Train-Ingalls), and we can confirm we are using – Spring Data JPA – 1.11

![](https://i0.wp.com/www.javabullets.com/wp-content/uploads/2017/09/2017-09-11-21_04_09-Release-Train-Ingalls-·-spring-projects_spring-data-commons-Wiki-·-GitHub.png?resize=300%2C179&ssl=1)

An alternative is to search on Ingalls-SR6 on [mvnrepository](https://mvnrepository.com/artifact/org.springframework.data/spring-data-releasetrain/Ingalls-SR6) which confirms the version as 1.11.6.RELEASE.

![](https://i0.wp.com/www.javabullets.com/wp-content/uploads/2017/09/2017-09-11-21_01_45-Maven-Repository_-org.springframework.data-»-spring-data-releasetrain-»-Ingalls-.png?resize=300%2C179&ssl=1)

Or the easy way (thanks Oliver) –

> Hm, why not simply call mvn dependency:list -Dsort?
> 
> — Oliver Gierke &amp;‍ (@olivergierke) [12 September 2017](https://twitter.com/olivergierke/status/907495693048336385)

<script async="" charset="utf-8" src="//platform.twitter.com/widgets.js"></script>