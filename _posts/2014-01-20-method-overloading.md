---
id: 2104
title: 'Method Overloading'
date: '2014-01-20T16:40:43+00:00'
author: 'Martin Farrell'
layout: post
guid: 'http://glenware.wordpress.com/?p=40'
permalink: /method-overloading/
geo_public:
    - '0'
thrive_post_fonts:
    - '[]'
categories:
    - CoreJava
tags:
    - 'Core Java'
    - overloading
---

- Same name, different signature
- Can Overload – number, type or order of arguments
- Can Overload Constructors
- Can Overload static, private, final methods
- Static Binding
- Compile-time binding

**Overloading in Action**

Here are some legal overloading examples –

\[sourcecode language=”java”\] public class LegalOverloadingExamples {&gt;   
 // Overloaded constructor  
 public LegalOverloadingExamples() {}  
 public LegalOverloadingExamples(String overLoadedConstructorExample) {}  // Parameter Overloading  
 public String parameterOverloading() { return ""; }  
 public String parameterOverloading(String parameterOverloadingString){ return ""; }  
 public String parameterOverloading(String parameterOverloadingString, int parameterOverloadingInt) {}) { return ""; }

 // Return Type Overloading  
 public int returnTypeOverloading() { return 1; }  
 public Integer returnTypeOverloading() { return new Integer(1); }  
 public double returnTypeOverloading() { return 1.0d; }  
 public Double returnTypeOverloading() { return new Double(1.0d); }  
}  
\[/sourcecode\]

There are also some instances where overloading is not possible –

\[sourcecode language=”java”\] public class IllegalOverloadingExamples {  // Overloaded constructor   
 public IllegalOverloadingExamples(String overLoadedConstructorExample) {}  
 public IllegalOverloadingExamples(String illegalOverLoadedConstructorExample) {}  
 // Same signature as above

 // Parameter Overloading  
 public String parameterOverloading() { return ""; }  
 public String parameterOverloading(String parameterOverloadingString){ return ""; }  
 public String parameterOverloading(String illegalOverloadedParameterOverloadingString) {}) { return ""; }  
 // Same signature as above  
}  
\[/sourcecode\]