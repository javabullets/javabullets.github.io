---
id: 453
title: 'Spring Autowiring'
date: '2015-09-10T07:52:41+00:00'
author: 'Martin Farrell'
layout: post
guid: 'https://glenware.wordpress.com/?p=453'
permalink: /spring-autowiring/
thrive_post_fonts:
    - '[]'
categories:
    - Uncategorized
---

There is an eternal struggle in Spring as to whether xml or annotations are best. The truth is there is no “best” solution and sometimes xml is best, and sometimes annotations are best.

There are 4 types of autowiring –

- byName – Match by name or id. If no match then unwired
- byType – Match by type. If no match the unwired
- constructor – Match constructor arguments
- autodetect – constructor then byType

The default-autowire is none