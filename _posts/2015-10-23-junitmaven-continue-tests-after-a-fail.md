---
id: 463
title: 'Junit/maven &#8211; Continue Tests after a fail'
date: '2015-10-23T08:07:04+00:00'
author: 'Martin Farrell'
layout: post
guid: 'https://glenware.wordpress.com/?p=463'
permalink: /junitmaven-continue-tests-after-a-fail/
thrive_post_fonts:
    - '[]'
categories:
    - Uncategorized
---

This is a useful snippet to run all your tests even where one fails. This is useful where a test is dependent on an external system that is not available – obviously you will want to ensure the tests all pass at some point!

\[sourcecode language=”lang='xml”\] &lt;plugin&gt;  
 &lt;groupId&gt;org.apache.maven.plugins&lt;/groupId&gt;  
 &lt;artifactId&gt;maven-surefire-plugin&lt;/artifactId&gt;  
 &lt;version&gt;2.18.1&lt;/version&gt;  
 &lt;configuration&gt;  
 &lt;testFailureIgnore&gt;true&lt;/testFailureIgnore&gt;  
 &lt;/configuration&gt;  
 &lt;/plugin&gt;  
\[/sourcecode\] 