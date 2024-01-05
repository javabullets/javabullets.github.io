---
id: 2655
title: 'Java9 JShell Examples : Collections Static Factory Methods'
date: '2017-10-23T21:18:23+00:00'
author: 'Martin Farrell'
layout: post
guid: 'https://www.javabullets.com/?p=2655'
permalink: /java9-jshell-examples-collections-static-factory-methods/
thrive_post_fonts:
    - '[]'
image: /wp-content/uploads/2017/09/stefan-kunze-26932.jpg
categories:
    - Java
    - Java9
tags:
    - Collections
    - java
    - java9
    - 'static factory'
---

This post continues my exploration of Java9 features from my [My Top Java 9 Features](https://www.javabullets.com/top-java-9-features/) blog post. Here we are experimenting with Java9 Collections Static Factory Methods in the List, Set and Map interfaces.

# Collections Static Factory Methods

Java9 makes it easier to create immutable lists using its new static Factory Methods

## [List](https://docs.oracle.com/javase/9/docs/api/java/util/List.html#immutable) and [Set](https://docs.oracle.com/javase/9/docs/api/java/util/Set.html#immutable)

There are 12 Set.of and List.of methods –

- List.of() or Set.of()
- List.of(E e1) or Set.of(E e1) to E e10
- List.of(E… elements) or Set.of(E… elements)

### Examples

```
<pre class="">jshell> Set.of()
$1 ==> []
| created scratch variable $1 : Set<object></object>
```

*Note the inference as a List<object> object</object>*

To static <e> List<e> of (E e1, E e2, E e3) –</e></e>

```
<pre class="">jshell> List.of("one","two","three")
$2 ==> [one, two, three]
| created scratch variable $2 : List<string></string>
```

*Note the inference as a List<string> object</string>*

The number of arguments keeps increasing until E e10, at which point we can use vararg –

*static <e> List<e> of (E… elements)</e></e>*

## [Map](https://docs.oracle.com/javase/9/docs/api/java/util/Map.html)

Similarly Map defines –

- static <k> Map<k> of ()</k></k>
- static <k> Map<k> of (K k1, V v1) to (K k10, V v10)</k></k>
- static <k> Map<k> ofEntries (Map.Entry extends K,? extends V&gt;… entries) – Note the use of [Map.Entry](https://docs.oracle.com/javase/9/docs/api/java/util/Map.Entry.html)</k></k>

### Examples

```
<pre class="">jshell> Map.of()
$12 ==> {}

jshell> Map.of("key1", "value1", "key2", "value2")
$13 ==> {key1=value1, key2=value2}
| created scratch variable $13 : Map<string></string>
```

## Characteristics of Collections Static Factory Methods

Common characteristics of these static Factory Methods Lists, Sets and Maps are –

- Structurally Immutable – UnsupportedOperationException is thrown, although the elements themselves are immutable

```
<pre class="">jshell> Set<string> immutableSet = Set.of("one","two","three")
immutableSet ==> [three, two, one]
| created variable immutableSet : Set<string>

jshell> immutableSet.add("four")
| java.lang.UnsupportedOperationException thrown:</string></string>
```

- No Nulls – NullPointerException thrown

```
<pre class="">jshell> List<object> notNullList = List.of(null)
| Warning:
| non-varargs call of varargs method with inexact argument type for last parameter;
| cast to java.lang.Object for a varargs call
| cast to java.lang.Object[] for a non-varargs call and to suppress this warning
| List<object> notNullList = List.of(null);
| ^--^
| java.lang.NullPointerException thrown:
| at List.of (List.java:1030)
| at (#10:1)</object></object>
```

- Serialized – Serialized if elements Serializable

## List Specific Characteristics

- Order – Order is maintained the same as elements input

```
<pre class="">jshell> List<string> immutableList = List.of("one","two","three")
immutableList ==> [one, two, three]
| created variable immutableList : List<string></string></string>
```

## Set Specific Characteristics

- Reject Duplicates – The Set will also reject duplicates at creation time with an IllegalArgumentException –

```
<pre class="">jshell> Set.of("one","one")
| java.lang.IllegalArgumentException thrown: duplicate element: one
```

## Map Specific Characteristics

- Reject Duplicate Keus – The Map will reject duplicate keys with IllegalArgumentException –

```
<pre class="">jshell> Map.of("key1", "value1", "key1", "value2")
| java.lang.IllegalArgumentException thrown: duplicate key: key1
| at ImmutableCollections$MapN.<init> (ImmutableCollections.java:680)
| at Map.of (Map.java:1326)
| at (#15:1)</init>
```

- Iteration is also not guaranteed

# Conclusions

These are a useful and quick method for creating immutable collections, and jshell provides a good test ground to learn about the new methods and their associated characteristics