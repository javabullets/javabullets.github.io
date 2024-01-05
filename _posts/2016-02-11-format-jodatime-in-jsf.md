---
id: 530
title: 'Format JodaTime in JSF'
date: '2016-02-11T14:10:22+00:00'
author: 'Martin Farrell'
layout: post
guid: 'https://glenware.wordpress.com/?p=530'
permalink: /format-jodatime-in-jsf/
thrive_post_fonts:
    - '[]'
categories:
    - Uncategorized
---

My Bean has a jodatime DateTime object –

**private DateTime myDateTime;**

And I need to format the date as a String in a JSF. The easiest way is to call the DateTime object’s toString method(note the single quotes) –

**&lt;h:outputText value=”#{myDateTime.toString(‘dd/MM/YYYY’)}” /&gt;**