---
id: 2113
title: 'RESTful and HATEOAS Services &#8211; Revisited'
date: '2016-08-31T13:22:19+00:00'
author: 'Martin Farrell'
layout: post
guid: 'https://glenware.wordpress.com/?p=701'
permalink: /restful-and-hateoas-services-revisited/
thrive_post_fonts:
    - '[]'
categories:
    - Uncategorized
---

I get a lot of traffic for my original article on RESTful and HATEOAS services, so I’ve decided to update and revise the article.

# What are RESTful Services?

REST stands for Representational State Transfer. The main selling point of RESTful Services is that they are designed to be used on the internet using HTTP. It has the following constraints –

- Resources – Easily understood directory structure URI
- Uniform Interface – Create, Retrieve, Update, Delete are accessed over HTTP via POST, GET, PUT, and DELETE methods
- Messages – Most commonly these are JSON, but can be any format – HTML, XML, plain text, …
- Stateless – Interactions not stored, and state is handled in the request

## RESTful Examples

| GET | Select | GET /customerservice/customers/1 |
|---|---|---|
| POST | Create | POST /customerservice/customers |
| PUT | Update or Create – Idempotent | PUT /customerservice/customers |
| PATCH | Update only – Idempotent | PATCH /customerservice/customers/1 |
| DELETE | Remove record | DELETE /customerservice/customers/1 |

The HTTP return codes are used for success/failure –

| 1xx | Informational |
|---|---|
| 2xx | Success |
| 3xx | Redirection |
| 4xx | Client Error |
| 5xx | Server Error |

# HATEOAS – Hypermedia as the Engine of Application State

HATEOAS is an additional constraint on RESTful services. It requires the response to return the location of the data, and you can also return a list of other operations at that point. The advantage of this approach is that you can navigate the RESTful service model.

## HATEOAS Examples

GET customerservice/customers/1 HTTP/1.1 HTTP/1.1 200 OK

&lt;?xml version=”1.0″?&gt;  
&lt;customer&gt;  
&lt;id&gt;1&lt;/id&gt;  
&lt;/customer&gt;

Would have an additional link paramater to show its source –

&lt;?xml version=”1.0″?&gt;  
&lt;customer&gt;  
&lt;id&gt;1&lt;/id&gt;  
&lt;link rel=”self” href=”http://localhost:8080/customerservice/customers/1″ /&gt;  
&lt;/customer&gt;

## References

- http://www.restapitutorial.com/httpstatuscodes.html
- http://en.wikipedia.org/wiki/Representational\_state\_transfer
- http://en.wikipedia.org/wiki/HATEOAS
- http://docs.oracle.com/javaee/6/tutorial/doc/gijqy.html
- https://www.javabullets.com/2015/01/15/apache-cxf-jax-rs/
- https://github.com/spring-projects/spring-hateoas