---
id: 738
title: 'Solving Spring NoClassDefFoundError, ClassNotFoundException and MethodNotFoundExceptions Errors'
date: '2016-09-09T15:04:01+00:00'
author: 'Martin Farrell'
layout: post
guid: 'https://glenware.wordpress.com/?p=738'
permalink: /solving-spring-noclassdeffounderror-classnotfoundexception-and-methodnotfoundexceptions-errors/
thrive_post_fonts:
    - '[]'
categories:
    - Uncategorized
---

I see a lot of Spring questions on Stackoverflow about NoClassDefFoundError, ClassNotFoundException and MethodNotFoundExceptions, especially with Spring Boot. The cause is nearly always a change to a build dependency in gradle or maven has resulted in the mixing of dependencies. This post considers how approach these issues, providing a starting point and possibly resolving these issues.

# Steps

- Do a clean build – the conflict is being caused by out of date libraries in the maven repository
- If problems still exist then check Spring’s bill-of-materials to see what versions are recommended, and if they are in conflict with your own pom file versions. The purpose of the bom is discussed in my previous post(<https://www.javabullets.com/2016/09/07/maven-bill-of-materialsbom/>)
- Check which spring-framework-bom you are using – the import will be of the form –

```
<dependencyManagement>
    <dependencies>
        <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-framework-bom</artifactId>
            <version>4.3.2.RELEASE</version>
            <type>pom</type>
            <scope>import</scope>
        </dependency>
    </dependencies>
</dependencyManagement>
```

In this case we are using 4.3.2.RELEASE. You can then check what versions of other packages spring is using using mvnrepository(<https://mvnrepository.com/artifact/org.springframework/spring-framework-bom/4.3.2.RELEASE>).

- For spring boot you can further check dependencies using the spring-boot-dependencies pom –

<https://github.com/spring-projects/spring-boot/tree/master/spring-boot-dependencies>

# Conflicts

If the above highlights a conflict then your options are –

- Use the Spring Dependencies – Can you use the dependency version from the spring-bom or dependencies?
- Dependency Management – can you manage the issue through dependency management?
- Code Change – It may be that you are upgrading your spring boot or spring version to the latest version. It might be the API’s have changed, so you have to change your code. Time to get the manuals out