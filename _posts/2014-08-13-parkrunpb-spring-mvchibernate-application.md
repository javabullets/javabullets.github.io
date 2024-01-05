---
id: 160
title: 'parkrunPB &#8211; Spring MVC/Hibernate Application'
date: '2014-08-13T19:52:34+00:00'
author: 'Martin Farrell'
layout: post
guid: 'http://glenware.wordpress.com/?p=160'
permalink: /parkrunpb-spring-mvchibernate-application/
geo_public:
    - '0'
thrive_post_fonts:
    - '[]'
categories:
    - Java
    - Spring
tags:
    - hibernate
    - jackson
    - mockito
    - parkrunPB
    - 'spring MVC'
    - Spring3
    - 'twitter bootstrap'
---

I wrote this app to demonstrate some of the features of a number of technologies, and primarily Spring MVC.

The main technologies I’ll be demonstrating are –

- Twitter Bootstrap and WebJar
- Spring 3 and Spring MVC
- Hibernate
- Mockito
- Jackson JSON RESTFUL API

The source code can be downloaded from <https://github.com/farrelmr/parkrunPB>

Ive deliberately not strayed into proper Spring 4 MVC/Java 8 territory as I want to cover these in a future tutorial

 **parkrunPB**

The application itself is based on one of my favourite hobbies [parkrunning](http://www.parkrun.org.uk/). These are free weekly 5km running time trials, which started in London, but have expanded throughout the UK and globally. They are open to all, so you see everything from Olympic athletes to walkers. You also have people running with buggies or dogs. If your into running or want to get into running then it’s a good place to start.

The application is fairly simple – take your 5k time at one parkrun, then estimate your time on other courses by using the ratio of average times for the courses. Ive restricted the courses to only those in Scotland – but you could add more.