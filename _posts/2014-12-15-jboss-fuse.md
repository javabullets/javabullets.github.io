---
id: 340
title: 'JBoss Fuse'
date: '2014-12-15T08:38:18+00:00'
author: 'Martin Farrell'
layout: post
guid: 'http://glenware.wordpress.com/?p=340'
permalink: /jboss-fuse/
thrive_post_fonts:
    - '[]'
categories:
    - JEE
tags:
    - 'Dependency Injection'
    - Jboss
    - 'Jboss Fuse'
---

JBoss Fuse is an open source Enterprise Service Bus(ESB) combine a number of technologies –

- Apache Camel – Implementation of Hohpe and Woolf Enterprise Integration Patterns (EIPs)
- Apache CXF – Faster development for SOAP, XML/HTTP and RESTful Web Services
- Apache ActiveMQ – FUSE ESB messaging platform
- Apache Karaf – Lightweight OSGI-based runtime container for managing applications
- Fabric8 – Manage/Deploy applications from single location

Fuse Fabric technology layer supports the scalable deployment of JBoss Fuse containers across a network. It enables a variety of advanced features, such as remote installation and provisioning of containers; phased rollout of new versions of libraries and applications; load-balancing and failover of deployed endpoints.

**Supported File Types**

- jar(Default) – Used for Fuse Application Bundles (FABs)
- bundle – OSGi bundles. To use this packaging type, you must also configure the maven-bundle-plugin in the POM file.
- war – For WAR files. To use this packaging type, you must also configure the maven-war-plugin in the POM file.
- pom – When you build with this packaging type, the POM file itself gets installed into the local Maven repository. Used for Parent POM’s

**Fuse Folder Structure**

*pom.xml*  
*src/*  
*src/main/*  
*src/java/*  
*src/resources/*  
*META-INF/spring/\*.xml*  
*OSGI-INF/blueprint/\*.xml*  
*test/*  
*test/java/*  
*test/resources/*  
*target/*

**Deployment Metadata**

***META-INF/MANIFEST.MF***  
Can provide deployment metadata either for an OSGi bundle (in bundle headers) or for a FAB.

***META-INF/maven/groupId/artifactId/pom.xml***  
POM file—which is normally embedded in any Maven-built JAR file—is the main source of deployment metadata for a FAB.

***WEB-INF/web.xml***  
web.xml file is the standard descriptor for an application packaged as a Web ARchive(WAR).

**Dependency Injection(DI)**

Fuse supports two forms of dependency injection – spring and OSGI blueprint.xml

The approach to dependency injection is similar – but the key difference is that blueprint is tailored for OSGI, and can add dependencies through XML schema namespaces. While spring requires these to be added through maven-bundle-plugin.

The advice for using FUSE is to stick with blueprint as it is more closely bound to the OSGI container

*Example Spring – resources/META-INF/spring/\*.xml*

```
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
>

<camelContext xmlns="http://camel.apache.org/schema/spring">
<route>
<from uri="timer://myTimer?fixedRate=true&amp;period=2000"/>
<to uri="log:ExampleRouter"/>
</route>
</camelContext>

</beans>
```

*Example Blueprint – OSGI-INF/blueprint/\*.xml*

```
<?xml version="1.0" encoding="UTF-8"?>
<blueprint xmlns="http://www.osgi.org/xmlns/blueprint/v1.0.0"
xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
>

<camelContext xmlns="http://camel.apache.org/schema/blueprint">
<route>
<from uri="timer://myTimer?fixedRate=true&amp;period=2000"/>
<to uri="log:ExampleRouter"/>
</route>
</camelContext>

</blueprint>
```

**Fuse Fabric**

Fuse Fabric is the framework that supports the scalable deployment within Fuse Containers in a network environment. It includes services like remove installation, provisioning, load balancing and phased rollouts. At its core is the fabric registry(fabric database) which stores the information needed to manage/provision containers. A fabric allows a distributed network of containers to be managed across multiple hosts

**Fabric server**

Registry Service – replicable database &amp; child containers

*InstallDir/etc/system.properties* – Java system properties that affect installed bundles  
*InstallDir/etc/config.properties* – Java system properties that affect the Apache Karaf container

**Reference**

[http://www.jboss.org/products/fuse/overview/](http://www.jboss.org/products/fuse/overview/ "http://www.jboss.org/products/fuse/overview")  
[http://www.jboss.org/products/fuse/resources/#demos](http://www.jboss.org/products/fuse/resources/#demos "http://www.jboss.org/products/fuse/resources/#demos")  
[https://access.redhat.com/documentation/en-US/Red\_Hat\_JBoss\_Fuse/6.1/html/Fabric\_Guide/PartBasic.html](https://access.redhat.com/documentation/en-US/Red_Hat_JBoss_Fuse/6.1/html/Fabric_Guide/PartBasic.html "https://access.redhat.com/documentation/en-US/Red_Hat_JBoss_Fuse/6.1/html/Fabric_Guide/PartBasic.html")  
[http://docs.spring.io/osgi/docs/2.0.0.M1/reference/html/blueprint.html](http://docs.spring.io/osgi/docs/2.0.0.M1/reference/html/blueprint.html "http://docs.spring.io/osgi/docs/2.0.0.M1/reference/html/blueprint.html")  
[http://www.ibm.com/developerworks/opensource/library/os-osgiblueprint](http://www.ibm.com/developerworks/opensource/library/os-osgiblueprint "http://www.ibm.com/developerworks/opensource/library/os-osgiblueprint")