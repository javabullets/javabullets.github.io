---
id: 2103
title: Autoboxing
date: '2014-01-20T14:58:35+00:00'
author: 'Martin Farrell'
layout: post
guid: 'http://glenware.wordpress.com/?p=38'
permalink: /autoboxing/
geo_public:
    - '0'
thrive_post_fonts:
    - '[]'
categories:
    - CoreJava
tags:
    - autoboxing
    - 'Core Java'
---

Autoboxing was added in Java 1.5 as a means of converting between primitives and their object wrappers, and back again. The transformation is done by the Java compiler using the valueOf, xxxValue methods.

Examples –

\[sourcecode language=”java”\] Integer i = 10; i = i + 10;  
\[/sourcecode\]

So this is great news because you can stop using primitives? Not quite – you still incur an overhead on object creation –

\[sourcecode language=”java”\] Integer total = 0;  
for (int i=1; i&lt;5000; i++){  
 total+=i;  
}  
\[/sourcecode\] The compiler would generate code similar to –

\[sourcecode language=”java”\] Integer total = 0;  
for(int i=1; i&lt;5000; i++){  
 int result = total.intValue() + i;  
 total = new Integer(result);  
}  
\[/sourcecode\] 