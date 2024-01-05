---
id: 357
title: 'Apache CXF &#8211; Quickstart'
date: '2014-12-18T09:24:34+00:00'
author: 'Martin Farrell'
layout: post
guid: 'https://glenware.wordpress.com/?p=357'
permalink: /apache-cxf-quickstart/
thrive_post_fonts:
    - '[]'
categories:
    - 'Web Services'
tags:
    - 'Apache CXF'
---

Is an open source web services development framework using JAX-WS and JAX-RS. Its goals are to be –

- High Performance
- Extensible
- Intuitive &amp; Easy to Use

It supports a number of protocols and transport including –

- Protocols – SOAP, RESTful HTTP
- Transport – HTTP, JMS, JBI(Java Business Integration)

**Quickstart**

The easiest way to create a CXF web service is through the servicemix archetypes(https://github.com/apache/servicemix-archetypes)

\[sourcecode\] mvn archetype:generate \\  
-DarchetypeGroupId=org.apache.servicemix.tooling \\  
-DarchetypeArtifactId=servicemix-cxf-code-first-osgi-bundle \\  
-DarchetypeVersion=2013.01 \\  
-DgroupId=com.glenware.cxf \\  
-DartifactId=myfirstcxf \\  
-Dversion=1.0-SNAPSHOT  
\[/sourcecode\] Compile and install –

\[sourcecode\] mvn clean install  
\[/sourcecode\] Then its a matter of installing the package in Fuse or ServiceMix –

\[sourcecode\] JBossFuse:karaf@root&gt; install mvn:com.glenware.cxf/myfirstcxf/1.0-SNAPSHOT  
JBossFuse:karaf@root&gt; list  
JBossFuse:karaf@root&gt; start &lt;number&gt;  
\[/sourcecode\] Test the service is installed –

*http://localhost:8181/cxf?wsdl*

Or –

*cd get-started/myfirstcxf*   
\[sourcecode\] mvn -Pclient  
\[/sourcecode\]  **References**

[https://github.com/apache/servicemix-archetypes](https://github.com/apache/servicemix-archetypes "https://github.com/apache/servicemix-archetypes")

[https://github.com/apache/servicemix-archetypes/tree/trunk/servicemix-cxf-code-first-osgi-bundle](https://github.com/apache/servicemix-archetypes/tree/trunk/servicemix-cxf-code-first-osgi-bundle "https://github.com/apache/servicemix-archetypes/tree/trunk/servicemix-cxf-code-first-osgi-bundle")

[https://access.redhat.com/documentation/en-US/Red\_Hat\_JBoss\_Fuse/6.0/html/Deploying\_into\_the\_Container/files/DeployCxf-Running.html](https://access.redhat.com/documentation/en-US/Red_Hat_JBoss_Fuse/6.0/html/Deploying_into_the_Container/files/DeployCxf-Running.html "https://access.redhat.com/documentation/en-US/Red_Hat_JBoss_Fuse/6.0/html/Deploying_into_the_Container/files/DeployCxf-Running.html")