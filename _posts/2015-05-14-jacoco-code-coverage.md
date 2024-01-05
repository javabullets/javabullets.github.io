---
id: 431
title: 'Jacoco &#8211; Code Coverage'
date: '2015-05-14T08:50:03+00:00'
author: 'Martin Farrell'
layout: post
guid: 'https://glenware.wordpress.com/?p=431'
permalink: /jacoco-code-coverage/
thrive_post_fonts:
    - '[]'
categories:
    - Uncategorized
---

[Jacoco](http://Jacoco "Jacoco") is a free code coverage library for Java. It provides coverage([Counters](http://www.eclemma.org/jacoco/trunk/doc/counters.html "Counters")) for –

- Instructions
- Branches
- Cyclomatic Complexity
- Lines
- Methods

POM Configuration –

\[sourcecode language=”xml”\] &lt;plugins&gt;

&lt;plugin&gt;  
&lt;groupId&gt;org.jacoco&lt;/groupId&gt;  
&lt;artifactId&gt;jacoco-maven-plugin&lt;/artifactId&gt;  
&lt;version&gt;0.7.4.201502262128&lt;/version&gt;  
&lt;configuration&gt;  
&lt;excludes&gt;  
&lt;exclude&gt;\*\*/Util\*&lt;/exclude&gt;  
&lt;/excludes&gt;  
&lt;/configuration&gt;  
&lt;executions&gt;  
&lt;execution&gt;  
&lt;goals&gt;  
&lt;goal&gt;prepare-agent&lt;/goal&gt;  
&lt;/goals&gt;  
&lt;/execution&gt;  
&lt;execution&gt;  
&lt;id&gt;report&lt;/id&gt;  
&lt;phase&gt;prepare-package&lt;/phase&gt;  
&lt;goals&gt;  
&lt;goal&gt;report&lt;/goal&gt;  
&lt;/goals&gt;  
&lt;/execution&gt;  
&lt;/executions&gt;  
&lt;/plugin&gt;  
&lt;/plugins&gt;

\[/sourcecode\] This can then be called by Jenkins using –

> mvn clean test sonar:sonar

The above configuration excludes files ending in Util. Theres great debate over whether excluding fields is valid, but I think it is. Ideally I’d be able to exclude code that I consider ‘noise’, and there are plans to add support for this. I particularly like the idea of [FilteringOptions](https://github.com/jacoco/jacoco/wiki/FilteringOptions "FilteringOptions"), and like idea of excluding the following –

- Private, empty default constructors – assuming no calls to it
- Plain getters and setters