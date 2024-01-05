---
id: 1243
title: 'How Lambda&#8217;s And Anonymous Inner Classes(AIC) Work'
date: '2017-02-05T21:24:38+00:00'
author: 'Martin Farrell'
excerpt: 'This post compares anonymous inner classes and lambda''s, before looking at the decompiled byte code to see how lambda''s are implemented'
layout: post
guid: 'https://glenware.wordpress.com/?p=1243'
permalink: /how-lambdas-and-anonymous-inner-classesaic-work/
publicize_twitter_user:
    - glenwareltd
thrive_post_fonts:
    - '[]'
categories:
    - Java
    - Java8
tags:
    - java
    - java8
    - lambda
    - 'lambda''s'
---

At first glance a lambda looks like a short hand version of an anonymous inner class. But they are not. This post covers the differences between lambda’s and AIC’s

## Key Points

- Lambdas implement a functional interface
- Anonymous Inner Classes can extend a class or implement an interface with any number of methods
- Variables – Lambda’s can only access final or effectively final
- State – Anonymous inner classes can use instance variables so can have state, lambda’s cannot
- Scope – Lambda part of its enclosing scope – so cant define a variable with same name as variable in enclosing scope
- Compilation – Anonymous compiles to a class, while lambda is an invokedynamic instruction

## How they work

### Anonymous Inner Classes(AIC’s)

- Compiler generates a class file for each anonymous inner class
- For example – AnonymousInnerClass$1.class
- Like all classes it needs to be loaded and verified at startup

### Lambda

The key to the lambda implementation is the InvokeDynamic instruction introduced in Java 7. This allows dynamic languages to bind to symbol at runtime.

A lambda works like this –

- Generates invokedynamic call site, and uses a lambdafactory to return the functional implementation
- lambda convered to a method to be invoked by invokedynamic
- Method stored in class as private static method
- Two lambda types – non-capturing – only uses fields inside its body  
    – capturing – accesses fields outside its body

#### Non-capturing Lambda

Doesnt access fields outside its body

```
public class NonCapturingLambda {
    public static void main(String[] args) {
        Runnable nonCapturingLambda = () -> System.out.println("NonCapturingLambda");
        nonCapturingLambda.run();
    }
}
```

If we decode the class file using the [cfr decompiler](http://www.benf.org/other/cfr/), we see the –

- LambdaMetafactory
- Lambda is a static void method in our class

```
java -jar cfr_0_119.jar NonCapturingLambda --decodelambdas false
/*
 * Decompiled with CFR 0_119.
 */
import java.io.PrintStream;
import java.lang.invoke.LambdaMetafactory;

public class NonCapturingLambda {
    public static void main(String[] args) {
        Runnable nonCapturingLambda = (Runnable)LambdaMetafactory.metafactory(null, null, null, ()V, lambda$0(), ()V)();
        nonCapturingLambda.run();
    }

    private static /* synthetic */ void lambda$0() {
        System.out.println("NonCapturingLambda");
    }
}
```

#### Capturing Lambda

Accesses final or effectively final fields outside its body –

```
public class CapturingLambda {
    public static void main(String[] args) {
        String effectivelyFinal = "effectivelyFinal";
        Runnable capturingLambda = () -> System.out.println("capturingLambda " + effectivelyFinal);
        capturingLambda.run();
    }
}
```

This decompiles to –

```
java -jar cfr_0_119.jar CapturingLambda --decodelambdas false
/*
 * Decompiled with CFR 0_119.
 */
import java.io.PrintStream;
import java.lang.invoke.LambdaMetafactory;

public class CapturingLambda {
    public static void main(String[] args) {
        String effectivelyFinal = "effectivelyFinal";
        Runnable capturingLambda = (Runnable)LambdaMetafactory.metafactory(null, null, null, ()V, lambda$0(java.lang.String ), ()V)((String)effectivelyFinal);
        capturingLambda.run();
    }

    private static /* synthetic */ void lambda$0(String string) {
        System.out.println("capturingLambda " + string);
    }
}
```

The interesting part is the lambda$0 method signature has gone from empty to taking a parameter String

### References

<https://www.infoq.com/articles/Java-8-Lambdas-A-Peek-Under-the-Hood>

<http://cr.openjdk.java.net/~briangoetz/lambda/lambda-translation.html>