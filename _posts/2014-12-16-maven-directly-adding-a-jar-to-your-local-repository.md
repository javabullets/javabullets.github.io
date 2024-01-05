---
id: 346
title: 'Maven &#8211; Directly Adding a Jar To Your Local Repository'
date: '2014-12-16T13:15:25+00:00'
author: 'Martin Farrell'
layout: post
guid: 'http://glenware.wordpress.com/?p=346'
permalink: /maven-directly-adding-a-jar-to-your-local-repository/
thrive_post_fonts:
    - '[]'
categories:
    - BuildTools
---

I recently had a need to add fakereplace to my local repository as a test –

\[sourcecode language=”lang='xml”\] &lt;dependency&gt;  
 &lt;groupId&gt;org.fakereplace&lt;/groupId&gt;  
 &lt;artifactId&gt;fakereplace&lt;/artifactId&gt;  
 &lt;version&gt;1.0.0.Alpha2&lt;/version&gt;  
&lt;/dependency&gt;  
\[/sourcecode\] The problem was that I didnt want to use a remote repository, so used the following command to add it direct to the local repository –

mvn install:install-file \\  
 -DgroupId=org.fakereplace \\  
 -DartifactId=fakereplace \\  
 -Dpackaging=jar \\  
 -Dversion=1.0.0.Alpha2 \\  
 -Dfile=fakereplace-1.0.0.Alpha2.jar \\  
 -DgeneratePom=true

Navigating to \\org\\fakereplace\\fakereplace confirmed its installation