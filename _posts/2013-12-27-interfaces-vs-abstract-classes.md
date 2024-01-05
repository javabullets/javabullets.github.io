---
id: 2098
title: 'Interfaces vs Abstract Classes'
date: '2013-12-27T21:28:50+00:00'
author: 'Martin Farrell'
layout: post
guid: 'http://glenware.wordpress.com/?p=19'
permalink: /interfaces-vs-abstract-classes/
geo_public:
    - '0'
thrive_post_fonts:
    - '[]'
categories:
    - Uncategorized
tags:
    - 'Abstract Class'
    - 'Core Java'
    - interface
---

Why write another article on the differences between java interfaces and abstract classes? Well I’m getting used to using WordPress so thought it was a good place to start.

The key differences are –

**abstract class – IS-A**

- IS-A relationship
- Cannot be instantiated
- Uses abstract keyword
- Abstract methods must use abstract keyword
- Used when you want the programmer to complete the implementation
- Faster in JVM does not have to lookup

**interface – CAN-DO**

- CAN-DO relationship
- Cannot be instantiated
- No implementation
- Polymorphism
- public abstract by default
- public static final by default
- Provides a contract/rules – idea of what needs to be done, not how
- Slower in JVM as extra indirection looking up implementation

**When to use an interface and abstract class?**

The advantage of an interface is that you have a contract which any class can conform to, including existing classes. The advantage of an abstract class is that you can implement and encapsulate some core logic for your application, leaving the developer to code there own implementation

You may also want to use both, for example java.util.List is the interface, and java.util.AbstractList contains a core. You can also see this approach used in frameworks

Reference  
<http://mindprod.com/jgloss/interfacevsabstract.html>