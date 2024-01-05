---
id: 427
title: 'RESTful And HATEOAS Web Services'
date: '2015-01-27T11:00:32+00:00'
author: 'Martin Farrell'
layout: post
guid: 'https://glenware.wordpress.com/?p=427'
permalink: /restful-and-hateoas-web-services/
thrive_post_fonts:
    - '[]'
categories:
    - HATEOAS
    - REST
    - 'Web Services'
---

**RESTful – Representational State Transfer**

RESTful Web services are an alternative to SOAP. It is designed to be simple, lightweight and fast, with the following constraint’s –

- Resources – Easily understood directory structure URI
- Uniform Interface – GET, POST, PUT and DELETE
- Messages – Can return any format – HTML, XML, plain text, …
- Stateless – Interactions not stored

**Common Operations**

Consider the example in my [apache cxf jax rs](https://www.javabullets.com/2015/01/15/apache-cxf-jax-rs/) post. The GET, PUT and PATCH operations are idempontent. This means that the same parameters should return the same results

*GET* – Select – Idempotent

\[sourcecode\]GET /customerservice/customers/1\[/sourcecode\] *POST* – Create

\[sourcecode\]POST /customerservice/customers\[/sourcecode\] *PUT* – Update or Create – Idempotent

\[sourcecode\]PUT /customerservice/customers\[/sourcecode\] *PATCH* – Update only – Idempotent

\[sourcecode\]PATCH /mypath/1\[/sourcecode\] *DELETE* – Remove record

\[sourcecode\]DELETE /customerservice/customers/1\[/sourcecode\] **HATEOAS – Hypermedia as the Engine of Application State**

HATEOAS is a further constraint on RESTful services. It basically requires a response to return the location of the data. For example –

\[sourcecode\] GET customerservice/customers/1 HTTP/1.1 HTTP/1.1 200 OK &lt;?xml version="1.0"?&gt;  
&lt;customer&gt;  
 &lt;id&gt;1&lt;/id&gt;  
&lt;/customer&gt;  
\[/sourcecode\]

Would become –

\[sourcecode lang=”xml”\] &lt;?xml version="1.0"?&gt;  
&lt;customer&gt;  
 &lt;id&gt;1&lt;/id&gt;  
 &lt;link rel="self" href="/customerservice/customers/1" /&gt;  
&lt;/customer&gt;  
\[/sourcecode\] **References**

[http://en.wikipedia.org/wiki/Representational\_state\_transfer](http://en.wikipedia.org/wiki/Representational_state_transfer)  
<http://en.wikipedia.org/wiki/HATEOAS>  
<http://docs.oracle.com/javaee/6/tutorial/doc/gijqy.html>  
<https://www.javabullets.com/2015/01/15/apache-cxf-jax-rs/>  
<https://github.com/spring-projects/spring-hateoas>