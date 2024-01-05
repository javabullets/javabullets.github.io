---
id: 2671
title: 'Which IDE&#8217;s and Server&#8217;s support Java EE 8 and Java9'
date: '2017-11-17T12:20:10+00:00'
author: 'Martin Farrell'
layout: post
guid: 'https://www.javabullets.com/?p=2671'
permalink: /ides-servers-support-jee8-java9/
thrive_post_fonts:
    - '[]'
categories:
    - 'Java EE 8'
    - Java9
tags:
    - java
    - 'Java 9'
    - 'Java EE8'
    - JDK9
    - JEE8
---

Ive been wanting to experiment with Java EE 8 and Java9 as a continuation of my [posts](https://www.javabullets.com/top-java-9-features/) on Java 9. Seems simple enough, but its actually quite hard to get a combination of IDE and server to work together.

# The Easy Part – IDEs &amp; JDK9

The easy part of this problem is finding IDE’s which support Java 9. All the major IDE’s offer JDK support –

- [Eclipse Oxygen](<https://wiki.eclipse.org/Configure_Eclipse_for_Java_9) > IntelliJ IDEA 2017.3 for Java 9 (https://blog.jetbrains.com/idea/2017/09/java-9-and-intellij-idea/>)
- [IntelliJ IDEA 2017.3 for Java 9](https://blog.jetbrains.com/idea/2017/09/java-9-and-intellij-idea/)
- [Netbeans 8.2](https://netbeans.org/downloads/) – you need to run on JDK8, but can compile against JDK9 – “NetBeans 8.2 does not run on JDK9!”

I am not really bothered which IDE I use, but generally have used Eclipse and NetBeans. So the rest of the post will focus on these, but would be interested to hear some IntelliJ input?

# The Hard Part – Application Servers

This is more complicated as the only application servers that are Java EE8 compliant as I understand it –

- [Glassfish 5.0](https://javaee.github.io/glassfish/download)
- [Payara 5](http://blog.payara.fish/payara-server-5-alpha-2-release-is-here)

This is where it gets complicated!

This means –

- Eclipse Doesnt Yet support [Glassfish 5](https://github.com/javaee/glassfish/issues/22279)

Im not sure on IntelliJ, but that leaves NetBeans 8.2

As stated I must install both Java8 and Java9. This isnt an issue as all my projects are built and deployed against Java9 so I have both JDK’s

## Setting Up Netbeans

The post on Eclipse support for [Glassfish 5](https://github.com/javaee/glassfish/issues/22279) links to a github page for setting up [Netbeans 8.2 and Glassfish 5.0](https://github.com/javaee/j1-hol#initial-setup)

The focus of that page is Java8, with a note that Java 9 isnt supported by Glassfish 5.0, so you must use Java 8. A quick check shows that Glassfish 5.0.1 will be released soon. So game over!

## Actually No

I concluded that I didnt need full Java EE 8 support as I only needed specific components. My main aim is to experiment with Java 9 and –

- Servlet 4.0
- JAX-RS 2.1
- JSF2.3
- CDI 2.0
- JPA2.1

From googling I believe that Wildfly 11 will give me this support on Java 9, although I am struggling to find a page listing the version compliant? Anyone?

# Update

I noticed [Arjan Tijms](https://stackoverflow.com/users/472792/arjan-tijms) posted this solution on stackoverflow to change the MANIFEST version on glassfish-api.jar. Its extreme but worth giving a try-

https://stackoverflow.com/questions/46584107/eclipse-support-for-glassfish-5

# Conclusions

This post is not from a frustration point of view. Everything in Java 9 and Java EE 8 is new and it takes time to produce the compliant software. The purpose of this post was to set out the current status as I see it. I would be interested to hear how others are getting on.