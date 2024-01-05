---
id: 1221
title: 'Another Java Lamdba Post'
date: '2017-02-05T21:06:58+00:00'
author: 'Martin Farrell'
excerpt: 'I held back writing this post because I felt the last thing the Java world needs is another Lambda post, but the main reason I blog is to improve my ability to explain technologies, so I decided to do it anyway.'
layout: post
guid: 'https://glenware.wordpress.com/?p=1221'
permalink: /another-java-lamdba-post/
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
---

The syntax of a lambda is simple –

```
(parameters) -> expression
(parameters) -> { statements; }
```

## Examples

```
Runnable r1 = () -> System.out.println("Hello World");

// or Streams
List winnersOfToursLessThan3500km = 
                           tdfWinners
                               .stream()
                               .filter(d -> d.getLengthKm() > 3500) // Separate out Tours less than 3500km
                               .map(Winner::getName) // Get names of winners
                               .collect(toList()); // Return a list
```

Key points are –

- Concise – Before Java8 we would use anonymous classes to deliver “similar” functionality
- Anonymous – no explicit name
- Function – not associated with a particular class
- Argument – can be passed as argument or Stored in a variable

## Functional Interfaces

The key requirement for a lambda is that the interface it implements can only have a Single Abstract Method(SAM). However they can have any number of default and static methods

## @FunctionalInterface Annotation

@FunctionalInterface is Optional, and enforces this rule by generating a compiler error –

```
Invalid '@FunctionalInterface' annotation; NotFunctionalInteface is not a functional interface
```

Java8 contains a number of useful interfaces in java.util.function –

```
Predicate   - boolean test(T t) - True/False condition on t
Consumer    - void accept(T t)  - Accepts t but returns no result
Function<t r=""> - R apply(T t)      - Takes t and returns an instance of R</t>
```

## Functional Descriptor

Describes the signature of the Functional Interface –

```
Predicate - t  -> boolean
Consumer  - t  -> void
Function  - t  -> R
```

## Accessing Class Variables

- final or “effectively final”(not changed after initialization)

```
        String finalString = "final string";
        String effectivelyFinalString = "effectively final string";
        Runnable r = () -> {
            System.out.println("Hi im " + finalString);
            System.out.println("Hi im " + effectivelyFinalString);
        };
        new Thread(r).start();
```

But trying to change effectivelyFinalString results stops it compiling –

```
       String finalString = "final string";
        String effectivelyFinalString = "effectively final string";
        Runnable r = () -> {
            System.out.println("Hi im " + finalString);
            System.out.println("Hi im " + effectivelyFinalString);
        };
        effectivelyFinalString = "now i wont compile";
        new Thread(r).start();
```

The reason this works is that the lambda is accessing a copy of the final or “effectively final” field.

## Method References – ::

The final piece of the lambda jigsaw is Method References. Method references are simply a further shorthand in the body of the lambda – so where we have a lambda –

```
List winnersOfToursGreaterThan3500km = 
          tdfWinners
             .stream()
             .filter(d -> d.getLengthKm() >= 3500)
             .map(w -> w.getName())
             .collect(toList());
```

We see map has the lambda – w -&gt; w.getName(). This represents this method –

```
 Stream map(Function super T, ? extends R> mapper);
```

A method reference lets us shorthand that too –

```
List winnersOfToursGreaterThan3500km = 
          tdfWinners
             .stream()
             .filter(d -> d.getLengthKm() >= 3500)
             .map(W::getName)
             .collect(toList());
```

As with a lot of Java8 the purpose of this shorthand is to allow us to write cleaner more concise code

There are 4 types of method reference in Java 8 –

| static method | `ContainingClass::staticMethodName` |
|---|---|
| instance method of a particular object | `containingObject::instanceMethodName` |
| instance method of an arbitrary object of a particular type | `ContainingType::methodName` |
| constructor | `ClassName::new` |