---
id: 1213
title: 'Google &#8211; Site Reliability Engineering'
date: '2017-02-01T09:16:26+00:00'
author: 'Martin Farrell'
excerpt: 'Googles Site Reliabilty Engineering book can now be accessed at <a href="https://landing.google.com/sre/book.html">Site Reliability Engineering</a>'
layout: post
guid: 'https://glenware.wordpress.com/?p=1213'
permalink: /google-site-reliability-engineering/
publicize_twitter_user:
    - glenwareltd
thrive_post_fonts:
    - '[]'
categories:
    - devops
    - Java
    - 'Site Reliability Engineering'
---

Id previously posted a review of the O’Reilly [Site Reliablity Engineering](https://www.javabullets.com/2016/12/06/best-tech-book-ive-read-this-year) book by Betsy Beyer(Editor) and Chris Jones(Editor). I consider it an important book for any developer or admin to read as it covers googles approach to deploying and scaling their systems. The good news is google have released the book online at [Site Reliability Engineering](https://landing.google.com/sre/book.html)

The book is structured as a series of essays, and the best approach to reading it is to choose the chapters that look like they are most useful to you, and read them. In my case the most relevent chapters were –

- Structure of Googles Applications
- Risk and SLA’s
- Monitoring and Automation

The ultimate aim of an Site Reliability Engineer is that the site should be self managing, and the SRE writes the systems to achieve this.

The most important points I learnt were –

- Monitoring and Health Checks – They recommend using prometheus.io, and build alerting on top
- SLA – Does not have to be all encompassing – for example if you cant serve images, but can provide a text based service then you can split the SLA
- Simplicity – The most important skill a developer can have is to keep it simple

## Putting It Into Practice

From my point of view the most important thing to add to my applications was health checks and monitoring. This was easy since most modern frameworks, Spring Boot and dropwizard, already contain health check API’s.