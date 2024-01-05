---
id: 2105
title: 'Overriding @Override'
date: '2014-01-22T14:48:38+00:00'
author: 'Martin Farrell'
layout: post
guid: 'http://glenware.wordpress.com/?p=45'
permalink: /overriding-override/
geo_public:
    - '0'
thrive_post_fonts:
    - '[]'
categories:
    - CoreJava
tags:
    - 'Core Java'
    - overriding
---

Method overriding is often confused with method overloading. To restate – overloading is changing the signature of a method within a class, so multiple methods can share the same name, but different attributes. Method overloading concerns changing the behavior of a method in a subclass.

The rules can be summarised –

- Same declared name as super-class
- Signature and return type remain the same
- Cant override private, static and final methods
- Best Practice – @Override annotation
- Can only throw wider exception – eg if parent throws IOException, you can throw Exception but not FileNotFoundException
- Cannot reduce visibility
- Dynamic Binding @Runtime
- Access level cannot be less than parent – eg public parent cannot have protected or default child
- Polymorphism

**Example**

\[sourcecode language=”java”\] class Bicycle {

 public void ride() {

 System.out.println("Just riding");

 }

}

class RaceBike extends Bicycle {

 public void ride() {

 System.out.println("Ride fast");

 }

 public void sprint() {

 System.out.println("Sprint now!");

 }

}

public class TestOverride {

 public static void main(String \[\] args) {

 TestOverride testOverride = new TestOverride().go();

 }

 void go() {

 Bicycle bicycle = new Bicycle();

 bicycle.ride();

 Bicycle raceBike = new RaceBike();

 raceBike.ride();

 raceBike.sprint();

 }

}  
\[/sourcecode\]

Output

Just riding

Ride fast

Sprint now!