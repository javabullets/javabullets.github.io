---
id: 2682
title: 'Spring Boot as a Windows Service in 5 minutes'
date: '2017-12-06T13:05:28+00:00'
author: 'Martin Farrell'
layout: post
guid: 'https://www.javabullets.com/?p=2682'
permalink: /running-spring-boot-windows-service-5-minutes/
thrive_post_fonts:
    - '[]'
categories:
    - 'Spring Boot'
tags:
    - 'Spring Boot'
    - 'windows service'
    - winsw
---

I recently had to deploy a Spring Boot application as a windows service and am surprised how easy it was using [winsw](https://github.com/kohsuke/winsw). I’d previously written about using [procrun – Java Programs as Windows Services](https://www.javabullets.com/procrun-java-programs-as-windows-services/), but winsw is much easier

## Getting Started

There is a Section 59 of the Spring Boot documentation is about [Installing Spring Boot applications](https://docs.spring.io/spring-boot/docs/current/reference/html/deployment-install.html#deployment-windows), and points you towards a [github page](https://github.com/snicoll-scratches/spring-boot-daemon). This example uses that project for inspiration.

## Project

Im going to use the Spring IO “Serving Web Content” project as a starting point, so go to the web page and download the example from git or as a zip file.

<div class="wp-caption aligncenter" id="attachment_2686" style="width: 310px">![Running Spring Boot From Command Line](https://i0.wp.com/www.javabullets.com/wp-content/uploads/2017/12/springboot-2.png?resize=300%2C111&ssl=1)Running Spring Boot From Command Line

</div>We can then see our application running –

<div class="wp-caption aligncenter" id="attachment_2687" style="width: 310px">![Spring MVC Example](https://i0.wp.com/www.javabullets.com/wp-content/uploads/2017/12/springmvc.png?resize=300%2C188&ssl=1)Spring MVC Example

</div>## Wrapping as a Windows Service

- Download [winsw](https://github.com/kohsuke/winsw/releases) from github – remember to choose the correct version depending on the version of .net you are running
- Create windowsservice directory and copy the exe to this location

<div class="wp-caption aligncenter" id="attachment_2688" style="width: 310px">![Windows Service Directory](https://i0.wp.com/www.javabullets.com/wp-content/uploads/2017/12/directory.png?resize=300%2C111&ssl=1)Windows Service Directory

</div>- I renamed the *gs-serving-web-content-0.1.0.jar* to *gs-serving-web-content.jar*
- Rename winsw exe from *WinSW.NET4.exe* to *gs-serving-web-content.exe*
- Create a xml file named *gs-serving-web-content.*xml with the following content –

```
<pre class=""><?xml version="1.0" encoding="UTF-8"?>
<service>
 <id>gs-serving-web-content</id>
 <name>gs-serving-web-content</name>
 <description>gs-serving-web-content Windows Service</description>
 <executable>java</executable>
 <arguments>-jar "gs-serving-web-content.jar"</arguments>
 <logmode>rotate</logmode>
</service>
```

- We can then install with gs-serving-web-content.exe install (you may need to run as administrator)

![](https://i0.wp.com/www.javabullets.com/wp-content/uploads/2017/12/install.png?resize=300%2C107&ssl=1)

- We can then run this as a windows service –

![Windows Service](https://i0.wp.com/www.javabullets.com/wp-content/uploads/2017/12/gs-service.png?resize=300%2C50&ssl=1)

Windows Service

- To uninstall we run – gs-serving-web-content.exe uninstall

## Alternatives

I looked at procrun as an alternative wrapper for Spring Boot – but couldnt get it to work. It probably can – but needs more time.

## Conclusion

Im really impressed with winsw for installing Spring boot applications as windows services. Its really simple, and you can pass external application.properties files in through the xml configuration