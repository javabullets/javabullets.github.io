---
id: 725
title: 'Maven bill of materials (bom)'
date: '2016-09-07T10:30:08+00:00'
author: 'Martin Farrell'
layout: post
guid: 'https://glenware.wordpress.com/?p=725'
permalink: /maven-bill-of-materialsbom/
thrive_post_fonts:
    - '[]'
categories:
    - BuildTools
tags:
    - bom
    - maven
---

The purpose of a bill of materials(bom) is to centrally manage Maven dependencies and reduce the chances of version mismatches. The concept is orignally from supply chain management where it is used to track parts.

The key points of boms are –

- Special maven project to centrally manage dependencies
- Managed in dependencyManagement section of type – &lt;type&gt;pom&lt;/type&gt;
- Allow dependencies to cascade down through projects using &lt;dependencyManagement&gt;
- The bill of materials pom.xml will only list supported versions
- Simplify dependency import in child projects because you do not need to specify dependency versions

# Example

One advantage of bill-of-materials is that you can set out the recommended version dependencies, and use these through your application framework-

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

You can then import dependencies in sub-project pom’s without version numbers –

```

<dependency>
<groupId>org.springframework</groupId>
<artifactId>spring-core</artifactId>
</dependency>
```

# Reference

<http://docs.spring.io/spring/docs/current/spring-framework-reference/html/overview.html>