---
id: 171
title: 'parkrunPB &#8211; Twitter Bootstrap and WebJar'
date: '2014-08-13T20:31:14+00:00'
author: 'Martin Farrell'
layout: post
guid: 'http://glenware.wordpress.com/?p=171'
permalink: /twitter-bootstrap-and-webjar/
geo_public:
    - '0'
thrive_post_fonts:
    - '[]'
categories:
    - Java
tags:
    - parkrunPB
    - Spring
    - 'spring MVC'
    - Spring3
    - Spring4
---

In my experience Twitter Bootstrap is a CSS/Javascript framework loved by software developers, and hated by web designers. Software developers love it because they can quickly get a nice looking website up and running, while web designers hate it because it creates a generic look and feel that they see to often on websites. They also rightly claim that it means people don’t understand CSS anymore. As with most technology arguments, both sides are correct. The reason for my choice is that although I can code css, Im better at backend work, and Bootstrap frees me from those worries.

You can use bootstrap or jquery simply by downloading them from their respective websites. This project is taking advantage of the [webjars](http://www.webjars.org/) project. This allows these projects to be imported through your maven pom.xml –

```
<!-- WebJar -->
<dependency>
	<groupId>org.webjars</groupId>
	<artifactId>bootstrap</artifactId>
	<version>${bootstrap.version}</version>
</dependency>
<dependency>
	<groupId>org.webjars</groupId>
	<artifactId>jquery</artifactId>
	<version>1.11.0</version>
</dependency>
<!-- WebJar -->
```

The projects can then be accessed from your jsp’s using a call like –

```
<!-- Bootstrap core CSS -->
<link rel="stylesheet" type="text/css"
	href="${pageContext.request.contextPath}/assets/bootstrap/3.1.1/css/bootstrap.min.css" />
<link rel="stylesheet" type="text/css"
	href="${pageContext.request.contextPath}/assets/bootstrap/3.1.1/css/bootstrap-theme.min.css" />
...
<script type="text/javascript"
        src="${pageContext.request.contextPath}/assets/jquery/1.11.0/jquery.min.js"></script>
    <script
        src="${pageContext.request.contextPath}/assets/bootstrap/3.1.1/js/bootstrap.min.js"></script>
<!-- Bootstrap core CSS --> 
```

**Screens**

There are 3 screens in this application –

 **Homepage**

[![homepagescreenshot](http://glenware.files.wordpress.com/2014/08/homepagescreenshot.png?w=300&resize=577%2C318)](https://glenware.files.wordpress.com/2014/08/homepagescreenshot.png)

 **About**  
 ![aboutuspage](http://glenware.files.wordpress.com/2014/08/aboutuspage.png?w=300&resize=577%2C318)

**CRUD**

[![adminscreen](http://glenware.files.wordpress.com/2014/08/adminscreen.png?w=300&resize=576%2C313)](https://glenware.files.wordpress.com/2014/08/adminscreen.png)