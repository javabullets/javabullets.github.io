---
id: 2101
title: 'Object Oriented Principles'
date: '2014-01-10T14:00:25+00:00'
author: 'Martin Farrell'
layout: post
guid: 'http://glenware.wordpress.com/?p=30'
permalink: /object-oriented-principles/
geo_public:
    - '0'
thrive_post_fonts:
    - '[]'
categories:
    - Uncategorized
tags:
    - abstraction
    - encapsulation
    - inheritance
    - OOP
    - polymorphism
---

I was interviewing some students for a placement, and asked the standard “What are the four principles of OO development?” question as a warm-up. I was surprised that they struggled to name all four principles off the top of their head. In their defence the courses they came from were not purely software driven, and also would have covered other programming styles like procedural.

Lets start – the four principles of OOP are –

- Abstraction
- Encapsulation
- Polymorphism
- Inheritance

**Abstraction**

Abstraction is about exposing just enough information or functions, while hiding the complexity. This can be seen in Java through the external methods (public methods) in classes and interfaces, for example –

\[sourcecode language=”java”\] String encapsulationExample = "encapsulation".substring(0, 6);

\[/sourcecode\] The developer only needs to know what the substring method does, and doesn’t need to understand how it does it. The implementation is “encapsulated”.

**Encapsulation**

This is the flipside of abstraction. With encapsulation we hide the implementation of the abstracted methods. This means the internal implementation of the external methods. An example of this is getter/setter methods to access private members of a class

**Polymorphism**

Polymorphism in coding terms can be summed up as “one method, many implementations”. A literal definition is “of many forms” (“many” – poly, “forms” – morph). It allows us to define specialisms of methods in subclasses through overloading and overriding

I could waste my time coming up with an example of polymorphism in practice, but the best example Ive found is from stackoverflow (<span style="font-size:small;">[<span style="text-decoration:underline;"><span style="text-decoration:underline;"><span style="color:#0000ff;font-size:small;"><span style="text-decoration:underline;"><span style="color:#0000ff;font-size:small;">http://stackoverflow.com/questions/154577/polymorphism-vs-overriding-vs-overloading/154939#154939</span></span></span></span></span>](http://stackoverflow.com/questions/154577/polymorphism-vs-overriding-vs-overloading/154939)<span style="font-size:small;">).</span></span>

\[sourcecode language=”java”\] public abstract class Human {  
… public abstract void goPee();  
…  
}  
\[/sourcecode\] \[sourcecode language=”java”\] public class Male extends Human {  
 …  
 @Override  
 public void goPee() {  
 System.out.println("Stand Up");  
 }  
 …  
}  
\[/sourcecode\] \[sourcecode language=”java”\] public class Female extends Human {  
 …  
 @Override  
 public void goPee() {  
 System.out.println("Sit Down");  
 }  
 …  
}

public static void main(String args) {

 ArrayList&lt;Human&gt; group = new ArrayList&lt;Human&gt;();

 group.add(new Male());  
 group.add(new Female());  
 // … add more…

 // tell the class to take a pee break  
 for (Human person : group) {  
 person.goPee();  
 }

}  
\[/sourcecode\]

Output –  
Stand Up  
Sit Down

**Inheritance**

Inheritance is the process where a classes behaviour is derived from an existing superclass. This allows the developer to have common code contained in the superclass, and specialisms in the subclasses. A subclass inherits in java using the extends keyword.

**References**

<span style="font-size:small;">[<span style="text-decoration:underline;"><span style="text-decoration:underline;"><span style="color:#0000ff;font-size:small;"><span style="text-decoration:underline;"><span style="color:#0000ff;font-size:small;">http://stackoverflow.com/questions/154577/polymorphism-vs-overriding-vs-overloading/154939#154939</span></span></span></span></span>](http://stackoverflow.com/questions/154577/polymorphism-vs-overriding-vs-overloading/154939)</span>