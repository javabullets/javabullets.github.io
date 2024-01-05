---
id: 441
title: 'Spring BOM &#8211; Bill Of Materials'
date: '2015-06-05T11:38:32+00:00'
author: 'Martin Farrell'
layout: post
guid: 'https://glenware.wordpress.com/?p=441'
permalink: /spring-bom-bill-of-materials/
thrive_post_fonts:
    - '[]'
categories:
    - Uncategorized
---

Spring BOM is a fairly new addition to Spring. It stands for Build-Of-Materials, and gives you pivotal’s best and tested snapshot for a given version of Spring. This is great for software development as it frees you from mixing and matching versions of jars, and setting exclusions to get your build to work.

**Configuration**

Configuration is simply a matter of setting a build manager in your pom –

\[sourcecode lang=”xml”\] &lt;dependencyManagement&gt;  
 &lt;dependencies&gt;  
 &lt;dependency&gt;  
 &lt;groupId&gt;io.spring.platform&lt;/groupId&gt;  
 &lt;artifactId&gt;platform-bom&lt;/artifactId&gt;  
 &lt;version&gt;1.1.2.RELEASE&lt;/version&gt;  
 &lt;type&gt;pom&lt;/type&gt;  
 &lt;scope&gt;import&lt;/scope&gt;  
 &lt;/dependency&gt;  
 &lt;/dependencies&gt;  
&lt;/dependencyManagement&gt;&lt;/code&gt;  
\[/sourcecode\] You then reference the spring libraries you want to use without stating version number –

\[sourcecode lang=”xml”\] &lt;dependency&gt;  
 &lt;groupId&gt;org.springframework&lt;/groupId&gt;  
 &lt;artifactId&gt;spring-core&lt;/artifactId&gt;  
 &lt;exclusions&gt;  
 &lt;exclusion&gt;  
 &lt;groupId&gt;commons-logging&lt;/groupId&gt;  
 &lt;artifactId&gt;commons-logging&lt;/artifactId&gt;  
 &lt;/exclusion&gt;  
 &lt;/exclusions&gt;  
&lt;/dependency&gt;  
\[/sourcecode\] **Issues?**

One issue I had was tying Spring BOM versions to Spring Core versions which my application previously used. Spring BOM is using the release version numbers for Spring Boot, and I had to wade through the pom’s to find the required versions.

| Spring Platform BOM Version | Spring Version | Spring Data |
|---|---|---|
| 1.1.2.RELEASE | 4.2.0.RC1 | Gosling-M |
| 1.1.0.RELEASE | 4.1.3.RELEASE | Evans-SR1 |
| 1.0.0.RELEASE | 4.0.5.RELEASE | Dijkstra-RELEASE |

For future reference you need to check the [spring-boot-dependencies](https://github.com/spring-projects/spring-boot/blob/master/spring-boot-dependencies/pom.xml) project, which version number reconciles to the BOM version –

[https://github.com/spring-projects/spring-boot/blob/master/spring-boot-dependencies/pom.xml](https://github.com/spring-projects/spring-boot/blob/master/spring-boot-dependencies/pom.xml "https://github.com/spring-projects/spring-boot/blob/master/spring-boot-dependencies/pom.xml")

**Reference**