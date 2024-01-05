---
id: 1303
title: 'Java8 &#8211; Methods in Interfaces'
date: '2017-03-19T21:15:00+00:00'
author: 'Martin Farrell'
layout: post
guid: 'https://glenware.wordpress.com/?p=1303'
permalink: /java8-methods-in-interfaces/
publicize_twitter_user:
    - glenwareltd
post_views_count:
    - '6'
thrive_post_fonts:
    - '[]'
categories:
    - Java
---

My previous post on [streams](https://www.javabullets.com/2017/02/07/java-8-streams-cookbook/) demonstrates how useful this feature is to Java8. However it created a problem for the API designers. The problem was how to we extend the existing Collection’s API without breaking existing Collections implementations(guava, apache).

The solution was to allow methods in interfaces, meaning that any implementations already carry an implementation of the extension.

A good example is –

```
<pre class="prettyprint">
public interface Collection<e> extends Iterable<e> {
    // ....
    default Stream<e> stream() {
        return StreamSupport.stream(spliterator(), false);
    }
    // ....
}
</e></e></e>
```

Rules on default methods

- Always public
- default – keyword

## Multiple Inheritance

Java always had multiple inheritance in interfaces, but it wasn’t an issue as it didn’t matter which version of the inherited method was implemented.

```
<pre class="prettyprint">
interface Fred {
    void run();
}
public class OldSkoolMultipleInheritance implements Runnable, Fred {
    @Override
    public void run() {
        System.out.println("Runnable and Fred share the method signature run()");
    }
}
```

It’s more complex when default methods are involved, and java 8 has an order of precedence –

- classes &gt; interfaces
- children &gt; parent
- else select or override implementation

## Examples

```
<pre class="prettyprint">
interface Interface1 {
    default void defaultMethod() {
        System.out.println("print me!");
    }
}
interface Interface2 {
    default void defaultMethod() {
        System.out.println("no print me!");
    }
}
```

- Classes &gt; Interfaces –

```
<pre class="prettyprint">
public class DefaultMethodMultipleInheritance implements Interface1, Interface2 {
    @Override
    public void defaultMethod() {
        System.out.println("I win");
    }
}
```

- Child Interfaces &gt; Parent Interfaces –

```
<pre class="prettyprint">
interface Interface3 extends Interface2 {
    default void defaultMethod() {
        System.out.println("No no print me!!!");
    }
}
public class DefaultMethodMultipleInheritance implements Interface2, Interface3 {
    public static void main(String[] args) {
        new DefaultMethodMultipleInheritance().defaultMethod();
    }
}
```

```
Output - No no print me!!!
```

- Re-implement method in the DefaultMethodsClass and call the specific implementation using super –

```
Interface1.super.newWave(); or Interface2.super.newWave();
```

eg –

```
<pre class="prettyprint">
public class DefaultMethodMultipleInheritance implements Interface1, Interface2 {
    @Override
    public void defaultMethod() {
        Interface1.super.defaultMethod();
    }
}
```

## static methods

The reasoning here is to keep static methods specific to your interface in one location –

- public
- static
- Never inherited