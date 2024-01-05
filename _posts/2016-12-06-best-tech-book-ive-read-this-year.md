---
id: 1091
title: 'Best Tech Book I&#8217;ve Read This Year'
date: '2016-12-06T21:49:54+00:00'
author: 'Martin Farrell'
layout: post
guid: 'https://glenware.wordpress.com/?p=1091'
permalink: /best-tech-book-ive-read-this-year/
publicize_twitter_user:
    - glenwareltd
thrive_post_fonts:
    - '[]'
categories:
    - devops
    - Java
    - reliability-engineering
---

I had seen [Site Reliability Engineering: How Google Runs Production Systems](https://www.amazon.com/Site-Reliability-Engineering-Production-Systems/dp/149192912X/ref=sr_1_1?ie=UTF8&qid=1481053377&sr=8-1&keywords=site+reliability+engineering+how+google+runs+production+systems) by Betsy Beyer(Editor) and Chris Jones(Editor) discussed on tech forums for a while before I bought it. The posts were all saying how thought provoking the book was, and how it led the developers to change their approach to building applications. I could totally relate to this after reading the book.

The book is a blueprint for how google manages services, and is structured as a collection of essays covering subjects including –

- Structure of Googles Applications
- Risk and SLA’s
- Monitoring and Automation

The advantage of this approach is you can focus on the chapters which are most relevant to you, so if you are supporting a 1bn users you will have different needs than someone who wants a side project to self manage itself.

## Site Reliability Engineer – SRE

The basic idea of a Site Reliability Engineer is that developers develop systems that support the deployed application. Specifically creating structures that monitor, scale and self repair. This means that the application can scale itself. Obviously its not that easy, and google ceases work on Site Reliability once the SLA has been reached, and interestingly if the SLA isnt reached in a given time period then they will stop the service themselves to check for other systems relying on the service

## Key Things I Learned

- Monitoring and Health Checks – From a Java point of view I can monitor and write health check statistics and monitor these through [prometheus.io](https://prometheus.io/), and build alerting on top
- SLA – The SLA does not have to be all encompassing – for example if you cant serve images, but can provide a text based service then you can split the SLA
- Simplicity – The most important thing a developer can learn is to keep it simple

I did feel that there was a lot of material for large teams, and not every workplace has the skills for a dedicated SRS team. In these circumstances it is important for each layer to accept part of the responsibility. As a developer the most important thing I can do is keep it simple, and make metrics available.

## Conclusion

My advice is to get this book, and choose the principles that best suit your organisation.