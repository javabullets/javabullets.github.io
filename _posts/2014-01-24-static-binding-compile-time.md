---
id: 56
title: 'Static Binding &#8211; Compile Time'
date: '2014-01-24T22:00:50+00:00'
author: 'Martin Farrell'
layout: post
guid: 'http://glenware.wordpress.com/?p=56'
permalink: /static-binding-compile-time/
geo_public:
    - '0'
thrive_post_fonts:
    - '[]'
categories:
    - CoreJava
tags:
    - 'Core Java'
---

- Compile time
- private, final and static
- Overloaded methods

In the example below the compile will resolve the overloaded method, and call the total(Integer integer) method

\[sourcecode language=”java”\] public class StaticBinding {  
 public static void main(String args\[\]) {  
 Number n = new Integer();  StaticBinding staticBinding = new StaticBinding();  
 staticBinding.total(n);  
 }

 public void total(Number number) {  
 System.out.println("total(Number number));  
 }

 public void total(Integer integer) {  
 System.out.println("total(Integer integer)");  
 }  
}  
\[/sourcecode\]