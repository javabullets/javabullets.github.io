---
id: 2612
title: 'My Top Java 9 Features'
date: '2017-09-27T20:39:07+00:00'
author: 'Martin Farrell'
layout: post
guid: 'https://www.javabullets.com/?p=2612'
permalink: /top-java-9-features/
thrive_post_fonts:
    - '[]'
image: /wp-content/uploads/2017/09/tom-mussak-108425.jpg
categories:
    - Java
    - Java9
tags:
    - 'Java 9'
    - java9
---

I was busy last week and didnt have time to write a post on Java9, so have decided to do one now. The big story with Java 9 is the Jigsaw Java Platform Modularity System(JPMS) platform. It delayed the release of Java9, but this was a good thing as a poor implementation of Jigsaw would set Java back in the long run.

Java 9 is now released and this post takes a look at my top Java 9 features

# New Features

A full list of Java 9 features can be found [here](https://docs.oracle.com/javase/9/whatsnew/toc.htm), but the features I am most interested in are –

- Java Platform Modularity System(JPMS) – Prior to Java 9 you could pretty much access any public class on your classpath. This made it difficult to truly encapsulate API’s. Java 9’s JPMS seeks to resolve this through a module descriptor – module-info.java – which defines a modules requirements and what it exports
- [JShell](https://www.javabullets.com/jshell-in-five-minutes/) – REPL(Read-Eval-Print-Loop), or Shells, have been a common feature of other languages for a while, and Java has finally introduced a Java Shells. This provides a great environment for experimenting with API’s
- [HTTP/2 Client](https://www.javabullets.com/experimenting-with-java9-http-client-and-process-api-in-jshell/) – This client provides HTTP 2.0 support in the Java platform, and will replace the existing HttpURLConnection API. The key benefit of this is will result in improved performance over the HTTP1.1 implementation
- Streams API – There are some updates to the Streams API, including takeWhile, dropWhile which take/drop elements while a condition remains true
- [Process API](https://www.javabullets.com/experimenting-with-java9-http-client-and-process-api-in-jshell/) – The process API changes allows Java to manage OS processes without resorting to parsing the output of a call to System.exec. The simplest example is accessing the process Id, where we can now call –

```
<pre class="">int processId = ProcessHandle.current().pid();
```

- Private Methods In Interfaces – Java8 introduced [default and static methods in interfaces](https://www.javabullets.com/java8-methods-in-interfaces/) allowing us to add behaviour to interfaces. Private methods build on this by allowing default methods to call private methods implementing shared behaviour.

# Download

You can download Java 9 at – [download](http://www.oracle.com/technetwork/java/javase/downloads/jdk9-downloads-3848520.html)