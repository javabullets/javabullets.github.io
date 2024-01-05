---
id: 2116
title: 'Explaining Functional Programming'
date: '2017-01-06T10:04:05+00:00'
author: 'Martin Farrell'
layout: post
guid: 'https://glenware.wordpress.com/?p=1134'
permalink: /explaining-functional-programming/
publicize_twitter_user:
    - glenwareltd
thrive_post_fonts:
    - '[]'
categories:
    - Java
---

It occured to me that while I know what Functional Programming is, I would struggle to explain it. My description would be something like –

```
You know how Object Oriented Programming is about objects? Well Functional Programming is about functions, you know lambda's, Haskell and Scala
```

Mmmmnnnn…. not good.

This post is my attempt to provide a better description of functional programming, and let me know any feedback or mistakes

## What is Functional Programming?

Functional Programming uses mathematical functions to solve problems. Functions take an input and give an output, without changing the input –

```
f(x) -> some function of x
```

## Common Features

- Functions are first class entities – can be assigned to variables, passed as arguments, or returned from other functions
- No state
- No side effects – Anything the code does except produce an output from given inputs

Consider the example below the where setting I changes the changeState field, and also log. The are side effects as they go beyond returning an output from an input.

```
public class SideEffectExample {
   ```
private
``` ```
static
``` ```
final
``` ```
Logger LOGGER = Logger.getLogger(SideEffectExample.
``````
class
``````
.getName());
```
   boolean changeState = false;
   int i = 1;
   public int setI(int i) {
       this.i = i;
       this.changeState = true;
       LOGGER.info("change state");
       return i + 1;
   }
}
```

- Lazy evaluation – Compiler can decided when best to run a function

The degree to which the above features are enforced depends on the language

## Advantages of Functional Programming?

Functional Programming means that a function will give the same output for a given input, and doesnt alter state. This makes it well suited for problems involving –

- concurrency
- multi-threading
- multi-processors

## Reference

[https://en.wikipedia.org/wiki/Side\_effect\_%28computer\_science%29](https://en.wikipedia.org/wiki/Side_effect_%28computer_science%29)