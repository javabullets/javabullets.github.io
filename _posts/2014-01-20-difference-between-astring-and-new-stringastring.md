---
id: 2102
title: 'Difference between &#8220;aString&#8221; and new String(&#8220;aString&#8221;)'
date: '2014-01-20T12:22:42+00:00'
author: 'Martin Farrell'
layout: post
guid: 'http://glenware.wordpress.com/?p=33'
permalink: /difference-between-astring-and-new-stringastring/
geo_public:
    - '0'
thrive_post_fonts:
    - '[]'
categories:
    - Uncategorized
tags:
    - 'Core Java'
    - String
---

This is another classic interview question – is a String declared –

\[sourcecode language=”java”\]String s1 = "aString";\[/sourcecode\] The same as –

\[sourcecode language=”java”\]String s2 = new String("aString");\[/sourcecode\] Lets test it –

\[sourcecode language=”java”\] String test1 = "test"; – String test2 = "test";  
System.out.println(" \\"test\\" == \\"test\\" " + (test1 == test2)); // true  
String test3 = new String("test");  
System.out.println(" \\"test\\" == new String(\\"test\\") – " + test1 == test3); // false  
System.out.println(" \\"test\\".equals( new String(\\"test\\") ) – " + test1.equals(test3)); // true  
\[/sourcecode\] The Output is –

\[sourcecode language=”java”\] "test" == "test" true – "test" == new String("test") – false  
"test".equals( new String("test") ) – true  
\[/sourcecode\] The first comparison shows that test1 and test2 both share the same object String in the String Pool, the second comparison shows that although the String’s look the same – they do not point to the same memory location and exists on a different location of the String pool

**Whats the point?**

The next question is why/when would you use a String constructor. The most useful time is when using substring. A look at the substring source code, prior to Java 7, shows that it passes the original String’s character array. So if I have a 1GB long String, and am extracting the first letter I am still storing a copy of the original character array. Pretty innefficient, but Im pretty sure the current version of Java has done some work to fix this (need to check up).

If the following structure –

\[sourcecode language=”java”\] String extractedSubString = "extremely…long…string".substring( 0, 1 );  
\[/sourcecode\] Is changed to –

\[sourcecode language=”java”\] String extractedSubString = new String ( "extremely…long…string".substring( 0, 1 ) );  
\[/sourcecode\] This will create a String with a new character array the size of the substring.