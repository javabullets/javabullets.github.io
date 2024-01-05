---
id: 419
title: 'Contract First vs Contract Last'
date: '2015-01-16T13:43:37+00:00'
author: 'Martin Farrell'
layout: post
guid: 'https://glenware.wordpress.com/?p=419'
permalink: /contract-first-vs-contract-last/
thrive_post_fonts:
    - '[]'
categories:
    - 'Web Services'
tags:
    - 'Apache CXF'
    - 'Spring WS'
---

There are 2 approaches to defining SOAP web services –

- Contract First – create wsdl first
- Contract Last – code first, and wsdl generated from code

Apache CXF supports both contract first and contract last, but Spring WS only supports Contract First

The preferred approach is Contract First, but here is a list of the advantages and disadvantages of each approach.

**Contract First**

**Advantages**

- Easy for both producer and consumer to see what the contrat is.
- Reuse schema’s and xsd’s
- Could resuse interface if you change language at some point, eg you start developing in ruby, and then later want to switch to java

**Disadvantages**

- Need to learn a new skill – WSDL and XSD

**Contract Last**

**Advantage**

&gt; Retrofit on existing class

**Disadvantage**

- Object-to-XML impedance mismatch – simply put XML != Java Objects. A good example being the difficulty exporting Map’s
- Code changes may cause WSDL changes
- No guarantee what is being sent when Java object converted to XML

**References**

[](http://docs.spring.io/spring-ws/site/reference/html/why-contract-first.html "http://docs.spring.io/spring-ws/site/reference/html/why-contract-first.html")