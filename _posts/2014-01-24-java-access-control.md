---
id: 59
title: 'Java Access Control'
date: '2014-01-24T22:07:08+00:00'
author: 'Martin Farrell'
layout: post
guid: 'http://glenware.wordpress.com/?p=59'
permalink: /java-access-control/
geo_public:
    - '0'
thrive_post_fonts:
    - '[]'
categories:
    - CoreJava
tags:
    - 'access control'
    - 'Core Java'
---

Java access control is pretty simple –

**Class Level (including enum) – 2 Types**

- Default – Can only be seen within class
- public – Can be seen from anywhere

**Methods and Variables**

- public – Can be seen anywhere
- protected – Package and Subclasses
- default – Package Private
- private – Class Only