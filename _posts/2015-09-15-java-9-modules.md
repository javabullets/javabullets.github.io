---
id: 456
title: 'Java 9 Modules'
date: '2015-09-15T08:36:57+00:00'
author: 'Martin Farrell'
layout: post
guid: 'https://glenware.wordpress.com/?p=456'
permalink: /java-9-modules/
thrive_post_fonts:
    - '[]'
categories:
    - Uncategorized
---

Java already has some components that hint towards modularity – but doesnt offer true modularity. JSR-376 formally introduces modules into Java.

How we do it currently

Basically we use two approaches –

&gt; Packages – this allows us to group functionality into similar groups  
&gt; Jar files – We place all the packages associated with a given component into a jar file

The problem is once we have this structure we’re basically sticking the jar into the classpath and hope for the best.

OK its not that bad – we prop up this mechanism with build tools like maven, and frameworks like Spring and OSGI – but the potential for disaster is still there  
Modules

Modules address the shortcomings by ensuring the modules –

&gt; Encapsulate implementation details and expose whats needed  
&gt; Explicitly describe what they offer &amp; dependencies – verified and resolved automatically during development

A Module graph can be constructed from this

Ive not seen many examples of Java9 modules yet – but will update this post when I have some examples.

References

https://dzone.com/articles/the-java-module-system-first-look  
http://www.javaworld.com/article/2878952/java-platform/modularity-in-java-9.html