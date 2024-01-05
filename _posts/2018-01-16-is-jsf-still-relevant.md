---
id: 2760
title: 'Would you use JSF for your next project?'
date: '2018-01-16T13:16:36+00:00'
author: 'Martin Farrell'
layout: post
guid: 'https://www.javabullets.com/?p=2760'
permalink: /is-jsf-still-relevant/
thrive_post_fonts:
    - '[]'
thrive_share_count:
    - '{"facebook":0,"twitter":0,"plusone":0,"pinterest":0,"linkedin":0,"total":0,"last_fetch":1557512623,"url":"https://www.javabullets.com/is-jsf-still-relevant/"}'
categories:
    - 'Java EE 8'
    - JSF
tags:
    - 'java ee'
    - 'java ee 8'
    - JSF
    - 'JSF 2.3'
---

There was an excellent stackoverflow blog post last week about the [“Brutal Lifecycle of Javascript Frameworks”](https://stackoverflow.blog/2018/01/11/brutal-lifecycle-javascript-frameworks/). The article was about the speed at which Javascript UI frameworks(angularjs, angular, jquery and react) come into and fall out of fashion. The key metric for this post is questions per month on the framework, which is a reasonable metric to demonstrate these trends. Downloads would have been interesting too.

It got me thinking where are we with JSF, and my starting point was to superimpose JSF on top of the Javascript data –  
![](https://i0.wp.com/www.javabullets.com/wp-content/uploads/2018/01/all.png?resize=400%2C240&ssl=1)

Its hard to see clearly but JSF is in decline based on questions asked on Stackoverflow. If we remove Javascript we can see the decline started around 2013

![](https://i0.wp.com/www.javabullets.com/wp-content/uploads/2018/01/jsf.png?resize=400%2C240&ssl=1)

That said the level of questions is fairly small, and the level is relatively stable

This post tries to understand the current state of JSF, and whether there is still a place for JSF in modern development.

# What is JSF?

JSF is a component-based web framework that is part of Java EE. It was the only frontend framework under Java EE until Java EE 8 added its new MVC framework.

## Whats good about JSF?

For me the key strength of JSF lies in the component frameworks in the JSF ecosystem. In particular [PrimeFaces](https://www.primefaces.org/), or the utility libraries like [omnifaces](http://showcase.omnifaces.org/). They let you quickly get started on projects, have plenty examples and are especially suited in a team or for projects where developers lack frontend skills. The deployment model is often simple, with a single war or ear file per server

The current release of JSF is 2.3, with the specification for 2.4 currently in progress.

## Whats bad about JSF?

In 2014 JSF received criticism from the [thoughtworks techradar](https://www.thoughtworks.com/radar/languages-and-frameworks/jsf), which placed it on hold.

The main part of the criticism was that the JSF model is flawed as it –

> “encourages use of its own abstractions rather than fully embracing the underlying web model”

They do make the concession that the web model is getting more prominence in later versions of JSF.

There were rebuttals against this post particularly relating to more recent JSF versions. But it has contributed to JSF being regarded as a difficult framework to use.

# JSF is Marmite

JSF is the marmite of frontend development.

Whats [marmite](https://en.wikipedia.org/wiki/Marmite)? Its a yeast extract that you spread on toast. Some people love it, some hate it, but there is no middle ground. For the record I hate marmite, but I like JSF.

The reason I like JSF is that you can access good quality components, that are mature and well documented. It also has the advantage of allowing teams that are weak on front end skills to develop professional looking websites. There is a downside that it can be hard to deliver more complex requirements as the Request/Response model is more abstract under JSF.

# Should you use JSF for new projects?

The JSF model has fallen out of favour. It is viewed as a legacy framework against today’s Javascript frameworks with RESTful API backend’s. This has moved the Java to implementing the RESTful microservices. This approach can often scale better than JSF.

The stackoverflow blog [post](https://stackoverflow.blog/2018/01/11/brutal-lifecycle-javascript-frameworks/) shows its not all plain sailing in the frontend Javascript world. The frameworks suffer from relatively short lifespans, although there are migration strategies, you do run the risk of your javascript framework being obsolete.

JSF has the advantage of being a mature model in this respect. Its also worth remembering that if your team is lacking in front end skills then JSF will help you quickly deliver a professional looking website.

# Question

I’d be interested in hearing other peoples experiences, and whether they will be using JSF on future projects