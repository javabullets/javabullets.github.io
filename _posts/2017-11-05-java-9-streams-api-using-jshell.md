---
id: 2663
title: 'Java 9 Streams API using JShell'
date: '2017-11-05T21:33:11+00:00'
author: 'Martin Farrell'
layout: post
guid: 'https://www.javabullets.com/?p=2663'
permalink: /java-9-streams-api-using-jshell/
thrive_post_fonts:
    - '[]'
image: /wp-content/uploads/2017/11/paul-morris-132832.jpg
categories:
    - Java
    - Java9
tags:
    - 'Java 9'
    - 'Streams API'
---

This post looks at the Java 9 Streams API using JShell. The Streams API changes build on the success of Streams in Java 8, and introduce a number of utility methods – takeWhile, dropWhile and iterate. This post continues [My Top Java 9 ](https://www.javabullets.com/top-java-9-features/)Features, and explores these methods using Jshell.

# Streams API

The Streams API and Lambda’s were the most successful features of Java 8, and the changes in Java 9 build on this with some new utility methods

## [jshell&gt; Stream.of(1,2,3,4,5).takeWhile(p -&gt; p Lets now return all values greater than 3, and we see the predicate instantly returns false and we get nothing returned

```
<pre class="">jshell> Stream.of(1,2,3,4,5).takeWhile(p -> p > 3).forEach(System.out::print);

jshell>
```

- Unordered Lists - Longest list of values until predicate fails, although there may be values down Stream that satisfy the predicate and these wont be returned

We can see this below where the list only returns 2, even though the final element is 1, while the ordered list would have returned the 1 and the 2 -

```
<pre class="">jshell> Stream.of(2,3,6,5,1).takeWhile(p -> p 
<h2><a href="https://docs.oracle.com/javase/9/docs/api/java/util/stream/Stream.html#dropWhile-java.util.function.Predicate-">dropWhile (Predicate super T> predicate)</a></h2>
<p>dropWhile provides the opposite behaviour of takeWhile, so records are dropped while a predicate is true. As before we have similar considerations for sorted and unsorted lists.</p>
```

- Ordered Lists - Will return the longest list of records excluding those elements that satisfy the predicate

```
<pre class="">jshell> Stream.of(1,2,3,4,5).dropWhile(p -> p 
```

- Unordered Lists - Will drop the first records that satisfy the predicate -

```
<pre class="">jshell> Stream.of(2,3,6,5,1).dropWhile(p -> p  Stream.of(1,2,3,5,6).dropWhile(p -> p 
<h2>dropWhile/takeWhile Conclusions</h2>
<p>The conclusion is that you need to take care when working with unordered list, unless the side effects are acceptable in your code. Although I cant think of a use case where I could accept the random element of unordered lists, I am sure some exist.</p>
<p> </p>
<h2><a href="https://docs.oracle.com/javase/9/docs/api/java/util/stream/Stream.html#iterate-T-java.util.function.Predicate-java.util.function.UnaryOperator-">iterate (T seed,Predicate super T> hasNext,UnaryOperator<t> next)</t></a></h2>
<p>This operates in a similar way to a for-loop. Taking a start value(T seed), exit condition(Predicate super T> hasNext) and whether we have a next value(Predicate super T> hasNext)</p>
<p>The iterate method has a exit condition attached -</p>
<pre class="">jshell> Stream.iterate(1, i -> i  i + 1).forEach(System.out::println);
1
2
3
4
5
<h1>Conclusion</h1>
<p>dropWhile and takeWhile present some useful utility methods for the Java Streams API. The major implication being whether your streams is ordered or unordered. The Stream.iterate method allows us to have for-loop functionality inside a Stream. I look forward to hearing peoples experiences with these new methods.</p>
```](<http://Predicate<? super T> predicate)(https://docs.oracle.com/javase/9/docs/api/java/util/stream/Stream.html#takeWhile-java.util.function.Predicate-“>takeWhile</a></h2>
<p>There are 2 types of list to consider when using takeWhile –</p>
<ul>
<li>Ordered Lists – Longest list of values until predicate fails</li>
</ul>
<p>So in this list we want all values less than 3, so we get 1 and 2 returned –</p>
<pre class=>)