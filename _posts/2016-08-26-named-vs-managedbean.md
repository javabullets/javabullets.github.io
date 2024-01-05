---
id: 688
title: '@Named vs @ManagedBean'
date: '2016-08-26T13:53:14+00:00'
author: 'Martin Farrell'
layout: post
guid: 'https://glenware.wordpress.com/?p=688'
permalink: /named-vs-managedbean/
thrive_post_fonts:
    - '[]'
thrive_share_count:
    - '{"facebook":0,"twitter":0,"plusone":0,"pinterest":null,"linkedin":0,"total":0,"last_fetch":1549935276,"url":"https://www.javabullets.com/named-vs-managedbean/"}'
categories:
    - Uncategorized
---

I was looking through some JSF controllers and it was a mix of @Named and @ManagedBean annotations. These annotations provide similar dependency injection, but there are important differences.

## Containers

- @ManagedBean – javax.faces.bean.ManagedBean – managed by JSF container
- @Named – javax.faces.bean – CDI bean managed by application server

This means @Named beans are visible to the whole JEE container, while @ManagedBean are visible only to the JSF container. The visibility issue is covered in this table

|  | Inject @Named | @ManagedBean |
|---|---|---|
| @Named | Y | Y(\*) |
| @ManagedBean | N | Y(\*) |

\* – only if scope of injected bean is broader

One problem is that @Named requires that you use a JEE enabled container – so you have to use TomEE instead of Tomcat.

## Scope

A further problem in earlier CDI versions was that there was no CDI equivalent of @ViewScoped. This has now been resolve with @javax.faces.view.ViewScope.

You also need to be very careful with mixing JSF and CDI as they use different packages – javax.faces.bean vs javax.enterprise.context.

## So what approach to use?

This is a non-question as from JSF2.3 onwards @ManagedBean is being phased out and the recommended approach is @Named. It is also important not to mix CDI with JSP scopes

However there are circumstances when I would still use @ManagedBean at present –

- Existing Code is using @ManagedBean – I would stick with @ManagedBean until a migration path is determined
- Web Container – You just want to use a web container – then sticking with JSF ManagedBeans would be easier

## Reference

http://germanescobar.net/2010/04/4-areas-of-possible-confusion-in-jee6.html