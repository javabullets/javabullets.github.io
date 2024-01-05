---
id: 2114
title: 'Can Spring Security be auto-generated?'
date: '2016-10-02T22:03:47+00:00'
author: 'Martin Farrell'
layout: post
guid: 'https://glenware.wordpress.com/?p=748'
permalink: /can-we-auto-generate-spring-security/
thrive_post_fonts:
    - '[]'
categories:
    - Spring
    - 'Spring Security'
tags:
    - Spring
    - 'Spring Security'
---

Can Spring Security be auto-generated? I’ve generally found Security requirements are easy to state, but hard to implement. So have been wondering if I can autogenerate my Spring Security configuration.

<div class="fhd lfhd">###  Security Requirements 

 </div>Common security requirements are –

- Access controls on files/directories based on roles, IP address
- Validation of credentials from a authentication provider

<div class="fhd lfhd">###  Coding 

 </div>The problem with coding yourself is –

- Security is complex and you need to know what your doing – there is a lot of information in the Spring Security manual
- Upgrades – Spring Security could upgrade and you could miss out on new features to improve your security
- Bugs – you introduce a bug in your code

It would be easier if I could define my security requirements into a website and autogenerate my security configuration.

<div class="fhd lfhd">###  Prototype 

 </div>I’ve created a prototype of this idea at [spring-security-generator](http://www.glenware.com/spring-security-generator/), with the code released on [github](<http://at https://github.com/farrelmr/spring-security-generator>)

![2016-10-02-21_51_07-spring-security-generator](https://glenware.files.wordpress.com/2016/10/2016-10-02-21_51_07-spring-security-generator.png?resize=1264%2C754)

<div class="fhd lfhd">###  Future 

 </div>I see this idea evolving to include –

- Tutorial – soon to be released
- REST API security
- Automate creation of unit tests, login pages
- Best practice – what are the best practice for configurations of spring security?
- Storing security configuration

I’d be interested in peoples feedback on this project