---
id: 651
title: 'Wildfly / Nexus'
date: '2016-07-13T16:00:14+00:00'
author: 'Martin Farrell'
layout: post
guid: 'https://glenware.wordpress.com/?p=651'
permalink: /wildfly-nexus/
thrive_post_fonts:
    - '[]'
categories:
    - Uncategorized
---

Lets say you are struggling to confirm what version of a war or ear you have deployed to a wildfly server, or need to quickly reconcile a version. One option you have if you are using nexus is to access the standalone.xml config on wildfly, and go to the deployments section –

&lt;deployments&gt;  
&lt;deployment name=”mywar.war” runtime-name=”mywar.war”&gt;  
&lt;content sha1=”a416a4430cb8d7292f9e5f0c2c80ad38348f10e2″/&gt;  
&lt;/deployment&gt;  
&lt;/deployments&gt;

You can then use the sha1 checksum to do a search against your nexus repository using artifact search