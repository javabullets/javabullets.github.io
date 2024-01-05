---
id: 446
title: 'Quick Logging on Log4j'
date: '2015-06-10T13:57:51+00:00'
author: 'Martin Farrell'
layout: post
guid: 'https://glenware.wordpress.com/?p=446'
permalink: /quick-logging-on-log4j/
thrive_post_fonts:
    - '[]'
categories:
    - Uncategorized
---

Ok occasionally I need to log from a class that I wouldn’t normally log from so wouldnt have configuration setup. An example being a test class

In the class below you simply add the config into a static method –

\[sourcecode lang=”java”\] public class MyTest {  static  
 {  
 Logger rootLogger = Logger.getRootLogger();  
 rootLogger.setLevel(Level.DEBUG);  
 rootLogger.addAppender(new ConsoleAppender(new PatternLayout("%-6r \[%p\] %c – %m%n")));  
 }

 private static final Logger LOGGER = Logger.getLogger(MyTest.class);

 public void MyMethod() {  
 LOGGER.debug("in MyMethod");  
 }

}  
\[/sourcecode\]

Its a hack – but its useful in a hurry