---
id: 2616
title: 'JShell in Five Minutes'
date: '2017-10-01T21:21:01+00:00'
author: 'Martin Farrell'
layout: post
guid: 'https://www.javabullets.com/?p=2616'
permalink: /jshell-in-five-minutes/
thrive_post_fonts:
    - '[]'
image: /wp-content/uploads/2017/10/IMG_20170930_1027323.jpg
categories:
    - Java9
tags:
    - java
    - java9
    - jshell
---

This post builds on my [My Top Java 9 Features](https://www.javabullets.com/top-java-9-features/) post by looking more in depth at these features. Here we show you how you can learn jshell in five minutes, and improve your Java 9 development experience.

## Getting Started

Assuming you have [downloaded](http://www.oracle.com/technetwork/java/javase/downloads/jdk9-downloads-3848520.html), and installed Java 9 then you can start the shell by typing –

```
<pre class="">jshell
```

Or if you want verbose –

```
<pre class="">C:\jdk9TestGround>jshell -v
| Welcome to JShell -- Version 9
| For an introduction type: /help intro

jshell>
```

## Variables

Simply type the variable, with or without semi-colons –

```
<pre class="">jshell> int i = 1;
i ==> 1
| created variable i : int
```

Unassigned values are automatically assigned to a variable beginning with **$** –

```
<pre class="">jshell> "Hello World"
$1 ==> "Hello World"
| created scratch variable $1 : String
```

This means we can reuse the value later –

```
<pre class="">jshell> System.out.println($1);
Hello World
```

## Control Flows

The next step in jshell is to use control flows (for, if, while, …). We can do this by entering our condition, using return for each new line –

```
<pre class="">jshell> if ("Hello World".equals($1)) {
 ...> System.out.println("Woohoo my if condition works");
 ...> }
Woohoo my if condition works
```

A quick tip is to use *TAB* for code completion

## Methods

We can declare a method in a similar way as Flow control, and press <return> for each new line –</return>

```
<pre class="">jshell> String helloWorld() {
 ...> return "hello world";
 ...> }
| created method helloWorld()
```

Then call it –

```
<pre class="">jshell> System.out.println(helloWorld());
hello world
```

We can also change methods in our shell, and have methods calling methods that arent defined yet –

```
<pre class="">jshell> String helloWorld() {
 ...> return forwardReferencing();
 ...> }
| modified method helloWorld(), however, it cannot be invoked until method forwardReferencing() is declared
| update overwrote method helloWorld()
```

Now we fix the method –

```
<pre class="">jshell> String forwardReferencing() {
 ...> return "forwardReferencing";
 ...> }
| created method forwardReferencing()
| update modified method helloWorld()
```

## Classes

We can also define classes in jshell –

```
<pre class="">jshell> class HelloWorld {
 ...> public String helloWorldClass() {
 ...> return "helloWorldClass";
 ...> }
 ...> }
| created class HelloWorld
```

And assign and access them –

```
<pre class="">jshell> HelloWorld hw = new HelloWorld();
hw ==> HelloWorld@27a5f880
| created variable hw : HelloWorld

jshell> System.out.println(hw.helloWorldClass());
helloWorldClass
```

## imports

The current classes in the workspace can be seen using the /imports command.

Additional classes can be added through the –class-path, or –module-path –

```
<pre class="">jshell --class-path ...
```

Or you can use /env within the shell –

```
<pre class="">/env
```

## Useful Commands

Now we’ve got the basics here are some quick commands –

| Tab | Code completion |
|---|---|
| /vars | list of variables in the current shell |
| /methods | list of methods in current shell |
| /list | All code snippets in jshell session |
| /imports | Current imports in the shell |
| /methods | list of methods in current shell |
| /types | Current classes defined in the shell, in the case above we would see “class HelloWorld” |
| /edit | Lets you edit your session in an editor(default to JEditPad) |
| /exit | close session |

## Conclusion

The real strength of the JShell REPL editor is you can test out snippets and API’s. This is really useful for saving time, and experimenting when learning Java9. In fact even if you are not moving to Java 9 straight away its worth installing JDK9 to use jshell as a testing ground.