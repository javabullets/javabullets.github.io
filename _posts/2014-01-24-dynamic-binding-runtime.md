---
id: 54
title: 'Dynamic Binding &#8211; Runtime'
date: '2014-01-24T21:59:46+00:00'
author: 'Martin Farrell'
layout: post
guid: 'http://glenware.wordpress.com/?p=54'
permalink: /dynamic-binding-runtime/
geo_public:
    - '0'
thrive_post_fonts:
    - '[]'
categories:
    - CoreJava
tags:
    - 'Core Java'
---

- Runtime time
- Overriden methods

The example below will compile, and the runtime environment will output “Riding fast” –

\[sourcecode language=”java”\] public class DynamicBinding {  
 public static void main(String args\[\]) {  
 Bicycle bicycle = new RaceBike();  
 bicycle.ride();  
 }  
} class Bicycle {  
 public void ride() {  
 System.out.println("Just riding my bike");  
 }  
}

class RaceBike extends Bicycle {  
 @Override  
 public void ride() {  
 System.out.println("Riding fast");  
 }  
}  
\[/sourcecode\]