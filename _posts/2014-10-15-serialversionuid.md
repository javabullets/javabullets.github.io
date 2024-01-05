---
id: 266
title: SerialVersionUID
date: '2014-10-15T20:15:23+00:00'
author: 'Martin Farrell'
layout: post
guid: 'http://glenware.wordpress.com/?p=266'
permalink: /serialversionuid/
geo_public:
    - '0'
thrive_post_fonts:
    - '[]'
categories:
    - CoreJava
    - Java
---

The SerialVersionUID is used as part of Serialization/de-Serialization. Its purpose is to ensure that when the object will not be deserialized if the SerialVersionUID’s dont match. If the ID’s dont match then a java.io.InvalidClassException is thrown.

The SerialVersionUID must be static, final, long and preferably private – eg private static final long serialVersionUID = 1L;

It can be generated by –

- User – You will often see code specified with 1L, 2L. This approach allows the developer to track changes in code like adding a new field
- Util – IDE or the JDK’s serialver util
- Compiler – There is also the option to leave it to the compiler to add this value, but this adds an overhead to the code so should be avoided

The key to using any of these approaches is to be consistent, and be sure to update it as the class changes

**References**

http://stackoverflow.com/questions/888335/why-generate-long-serialversionuid-instead-of-a-simple-1l