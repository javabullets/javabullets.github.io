---
id: 2714
title: 'Java 9 on Java EE 8 Using Eclipse and Open Liberty'
date: '2017-12-12T21:22:14+00:00'
author: 'Martin Farrell'
layout: post
guid: 'https://www.javabullets.com/?p=2714'
permalink: /java-9-on-java-ee-8-using-eclipse-and-open-liberty/
thrive_post_fonts:
    - '[]'
thrive_share_count:
    - '{"facebook":2,"twitter":0,"plusone":0,"pinterest":0,"linkedin":2,"total":4,"last_fetch":1513189455,"url":"https://www.javabullets.com/java-9-on-java-ee-8-using-eclipse-and-open-liberty/"}'
categories:
    - 'Java EE 8'
    - Java9
tags:
    - eclipse
    - 'Java 9'
    - 'Java EE8'
    - 'open liberty'
---

I wrote a post a few weeks ago titled [Which IDE’s and Server’s support Java EE 8 and Java9](https://www.javabullets.com/ides-servers-support-jee8-java9/) which looked at the current state of play between Java 9 and Java EE 8. As you would expect things have moved quickly and we now have some alpha and development builds supporting Java 9 and Java EE 8. These are –

- Payara 5 – For payaradomain
- [Open Liberty](https://openliberty.io)

Adam Bein posted a video [Java EE 8 on Java 9](http://adambien.blog/roller/abien/entry/java_ee_8_on_java) on how to deploy a Java 9 application on Open Liberty using netbeans. Its a great video and worth a look.

I decided to use the same approach as Adam to deploy a JSF Application on [Eclipse Oxygen](https://www.eclipse.org/downloads/packages/release/Oxygen/1A)

This post deals with installation and the first part of the project installing the core application, the next post will expand on this by building a JSF 2.3 application

# Installation

## Java 9

Ensure you are running Java 9 on both classpath and JAVA\_HOME, and also ensure you have [maven](https://maven.apache.org/) installed

<div class="wp-caption aligncenter" id="attachment_2730" style="width: 310px">![](https://i0.wp.com/www.javabullets.com/wp-content/uploads/2017/12/2017-12-12-19_57_25-Select-C__Windows_System32_cmd.exe_.png?resize=300%2C122&ssl=1)dos prompt

</div>## Open Liberty

Open Liberty came from IBM open sourcing WebSphere Liberty, and is a fully compliant Java EE 7 server. They also have an early release [Java EE 8 server](https://developer.ibm.com/wasdev/blog/2017/11/24/java-ee-8-implementation-november-liberty-beta/), which is getting improved all the time in their development builds. We will use a development build for this project, which can be downloaded from –

<div class="wp-caption aligncenter" id="attachment_2718" style="width: 310px">![](https://i0.wp.com/www.javabullets.com/wp-content/uploads/2017/12/screenshot-openliberty.io-2017-12-11-10-54-23.png?resize=300%2C145&ssl=1)Open Liberty Development Download

</div>## Eclipse

Eclipse Oxygen also has Java 9 release available at [download](https://marketplace.eclipse.org/content/java-9-support-beta-oxygen) – I am using the Java EE version of the eclipse

<div class="wp-caption aligncenter" id="attachment_2717" style="width: 310px">![](https://i0.wp.com/www.javabullets.com/wp-content/uploads/2017/12/screenshot-www.eclipse.org-2017-12-11-10-56-16.png?resize=300%2C145&ssl=1)Eclipse Download

</div>Work through the installation instructions. This is just unzipping Open Liberty Server to your preferred location, and similarly for Eclipse Oxygen

Start Eclipse Oxygen –

<div class="wp-caption aligncenter" id="attachment_2732" style="width: 310px">![](https://i0.wp.com/www.javabullets.com/wp-content/uploads/2017/12/2017-12-12-20_19_05-workspaceJSFJ9New-Eclipse-300x179.png?resize=300%2C179&ssl=1)Eclipse Oxygen

</div>## Installing Open Liberty on Eclipse Oxygen

Finally we need to install “IBM Liberty Development Tools for Oxygen” – Help &gt; Eclipse Marketplace

<div class="wp-caption aligncenter" id="attachment_2716" style="width: 231px">![](https://i0.wp.com/www.javabullets.com/wp-content/uploads/2017/12/marketplace.png?resize=221%2C300&ssl=1)IBM Liberty Developer Tools for Oxygen

</div>Then connect up our Open Liberty server on the Servers tab

![](https://i0.wp.com/www.javabullets.com/wp-content/uploads/2017/12/2017-12-12-20_26_24-New-Server.png?resize=263%2C300&ssl=1)

Finally point at your Open Liberty deployment location, and ensure you are using Java 9 –

![](https://i0.wp.com/www.javabullets.com/wp-content/uploads/2017/12/2017-12-12-20_28_16-New-Server-Runtime.png?resize=263%2C300&ssl=1)

You can click finish here

Finally we need to install the Java EE 8 Feature –

- Double Click “WebSphere Application Server Liberty”

![](https://i0.wp.com/www.javabullets.com/wp-content/uploads/2017/12/2017-12-12-20_32_51-workspaceJSFJ9New-WebSphere-Application-Server-Liberty-at-localhost-Eclipse-300x179.png?resize=300%2C179&ssl=1)

- Click “Open server configuration” then “Feature”

![](https://i0.wp.com/www.javabullets.com/wp-content/uploads/2017/12/2017-12-12-20_33_12-workspaceJSFJ9New-WebSphere-Application-Server-Liberty_servers_defaultServer_s.png?resize=1%2C1&ssl=1)![](https://i0.wp.com/www.javabullets.com/wp-content/uploads/2017/12/2017-12-12-20_33_12-workspaceJSFJ9New-WebSphere-Application-Server-Liberty_servers_defaultServer_s.png?resize=1%2C1&ssl=1)![](https://i0.wp.com/www.javabullets.com/wp-content/uploads/2017/12/2017-12-12-20_33_12-workspaceJSFJ9New-WebSphere-Application-Server-Liberty_servers_defaultServer_s.png?ssl=1)

Then “Add…” and select “javaee-8.0”

![](https://i0.wp.com/www.javabullets.com/wp-content/uploads/2017/12/2017-12-12-20_33_24-Add-Features.png?resize=243%2C300&ssl=1)

Id also remove JSF 2.3 as thats included in javaee-8.0

You could start the server now if you want

# First Project

The best archetype Ive found for Java EE 8 is also from Adam Bein.

To run it simply type –

```
<pre class="">mvn archetype:generate -DinteractiveMode=false -DarchetypeGroupId=com.airhacks -DarchetypeArtifactId=javaee8-essentials-archetype -DarchetypeVersion=0.0.2 -DgroupId=com.javabullets.javaee8 -DartifactId=javaee8
```

Then lets compile straight away and make sure there are no errors –

```
<pre class="">E:\code\javaee8>mvn clean package
```

Note the archetype is compiled against Java 8, we will move it to Java 9 in the next section

The source code is available at <https://github.com/farrelmr/javaee8>

## Open in Eclipse

In “Enterprise Explorer” select –

Import &gt; Import… &gt; Maven &gt; Existing Maven Projects

Navigate to your Java EE 8 directory, click Finish and let Eclipse load your project into Eclipse

Open the pom.xml file and change source and target from 1.8 to 1.9 –

```
<pre class=""> <properties>
    <maven.compiler.source>1.9</maven.compiler.source>
    <maven.compiler.target>1.9</maven.compiler.target>
    <failOnMissingWebXml>false</failOnMissingWebXml>
 </properties>
```

Then run maven (right click the project &gt; Run As… &gt; maven install)

## Add Project to Open Liberty

Go to –

Servers &gt; “WebSphere Application Server Liberty” &gt; Right Click “Add and Remove…”

![](https://i0.wp.com/www.javabullets.com/wp-content/uploads/2017/12/2017-12-12-21_00_53-Add-and-Remove....png?resize=285%2C300&ssl=1)

- Move our javaee8 application from Available to Configured
- Press Finish

## Start Open Liberty

Servers &gt; “WebSphere Application Server Liberty” &gt; Right Click “Start”

You will get an error message about setting a keystore. I am just cancelling this as its used by the “local connector” feature. Ive not found a way to clear this error on Eclipse – but will post when I have.

The server will start and you can access the pre-installed application on –

<http://localhost:9080/javaee8/resources/ping>

![](https://i0.wp.com/www.javabullets.com/wp-content/uploads/2017/12/2017-12-12-21_12_24-Mozilla-Firefox.png?resize=300%2C179&ssl=1)

### Woohoo running Java 9 on Java EE 8 Open Liberty!!!!

# Conclusion

This post uses Adam Bein’s approach to running Java 9 on Java EE 8 Open Liberty – but demonstrates how you can integrate this into Eclipse Oxygen. The next post will build on this archetype to create a simple JSF 2.3 application

Finally I think it is great to see the progress being made to provide Java EE 8 on Java 9, and would like to thank the developers involved in this work