---
id: 2100
title: 'What is immutability?'
date: '2014-01-10T13:34:53+00:00'
author: 'Martin Farrell'
layout: post
guid: 'http://glenware.wordpress.com/?p=25'
permalink: /what-is-immutability/
geo_public:
    - '0'
thrive_post_fonts:
    - '[]'
categories:
    - Uncategorized
tags:
    - immutability
    - String
---

The freedictionary (<span style="font-size:small;">[<span style="text-decoration:underline;"><span style="text-decoration:underline;"><span style="color:#0000ff;font-size:small;"><span style="text-decoration:underline;"><span style="color:#0000ff;font-size:small;">http://www.thefreedictionary.com/immutability</span></span></span></span></span>](http://www.thefreedictionary.com/immutability)<span style="font-size:small;">) defines immutable as – “Not subject or susceptible to change.”</span></span>

In code terms this means we cannot alter an object once it has been created. You will mostly hear the term discussed in conjunction with java.lang.String

**So what does an immutable class look like?**

Consider this example –

\[sourcecode language=”java”\] public final class ImmutableObject {  private final String imVar;

 public ImmutableObject(String imVar) {

 this.imVar = imVar;

 }

 public String getImVar() {

 return this.imVar;

 }

}  
\[/sourcecode\]

No setter method – You can only set imVar thru the constructor, and access it thru getImVar

This class also demonstrates some rules of thumb for creating immutable objects –

- Class is final
- Variables are final and private
- No setter methods – variables populated thru constructor

**Why would I use this?**

- Data cannot be changed – encapsulation means that other people cannot
- Change your objects state Thread safety – since the variables cannot
- Be changed once set then there are no issues with thread safety
- Re-usability – you can cache objects and reuse them

**Sounds good, anything else?**

You can circumvent immutability in java using reflection – but if your aim is to make use of the advantages immutable objects give you then you should avoid this. There may be times when this is a genuine requirement, but if your intention is to be clever then the best way to demonstrate your prowess is to KISS (Keep it Simple, Stupid)

**What is String immutability?**

The most common example of immutability in java is the String class –

\[sourcecode language=”language="java'”\] String s1 = "sample text"; s1.substring(0,6);

System.out.println(s1); // s1 – "sample text"

String s2 = s1.substring(0, 6);

System.out.println(s2); // s1 – "sample text", s2 – "sample"

s1 = s2;

System.out.println(s1); // s1 – "sample", s2 – "sample"  
\[/sourcecode\]

The example above shows the initial text “sample text” remains unchanged inspite of the operations on s1, and when we create a new object s2 a new String is created. We then assign s2 to s1, and note that s1 is now “sample”. This is because the value has not changed but the reference has

**Why are Strings immutable in java?**

- String Pool – Java stores all Strings in a String pool of the Java
- Heap. When a String is created the JVM will check the String pool to see if it exists, and if so it will link to that instance

\[sourcecode language=”java”\] String s1 = "test"; String s2 = "test"; // this will link to s1

System.out.println(s1 == s2); // true  
\[/sourcecode\]

Immutability is key to this functionality –

- Hashcode – The hashcode of a String is used in HashMaps, and
- Immutability guarantees that it will always be the same
- Security – It is common to see a database connection passed as a String. String pooling guarantees this cannot be changed
- Concurrency – Immutable objects are threadsafe by design

**References**

<span style="font-size:small;">[<span style="text-decoration:underline;"><span style="text-decoration:underline;"><span style="color:#0000ff;font-size:small;"><span style="text-decoration:underline;"><span style="color:#0000ff;font-size:small;">http://programmers.stackexchange.com/questions/195099/why-is-string-immutable-in-java</span></span></span></span></span>](http://programmers.stackexchange.com/questions/195099/why-is-string-immutable-in-java)</span>

<span style="font-size:small;">[<span style="text-decoration:underline;"><span style="text-decoration:underline;"><span style="color:#0000ff;font-size:small;"><span style="text-decoration:underline;"><span style="color:#0000ff;font-size:small;">http://java.dzone.com/articles/why-string-immutable-java</span></span></span></span></span>](http://java.dzone.com/articles/why-string-immutable-java)</span>

<span style="font-size:small;">[<span style="text-decoration:underline;"><span style="text-decoration:underline;"><span style="color:#0000ff;font-size:small;"><span style="text-decoration:underline;"><span style="color:#0000ff;font-size:small;">http://stackoverflow.com/questions/20945049/is-a-java-string-really-immutable</span></span></span></span></span>](http://stackoverflow.com/questions/20945049/is-a-java-string-really-immutable)</span>

**Are Strings truly immutable?**

I was on stackoverflow recently and noticed this question asking if Strings are truly immutable. The user provides this example code –

\[sourcecode language=”java”\] String s1 = "Hello World"; String s2 = "Hello World";

String s3 = s1.substring(6);

System.out.println(s1); // Hello World

System.out.println(s2); // Hello World

System.out.println(s3); // World

Field field = String.class.getDeclaredField("value");

field.setAccessible(true);

char\[\] value = (char\[\])field.get(s1);

value\[6\] = ‘J’;

value\[7\] = ‘a’;

value\[8\] = ‘v’;

value\[9\] = ‘a’;

value\[10\] = ‘!’;

System.out.println(s1); // Hello Java!

System.out.println(s2); // Hello Java!

System.out.println(s3); // World  
\[/sourcecode\]

On the surface of it looks like our clever user has found a flaw in the String classes immutability, as they have replaced ‘World’ with ‘Java!’. The substring s3 is unchanged, when you might expect it to output ‘Java!’

**So, whats the story?**

Well the answer is in 2 parts –

1\. Immutability – The above example suggests that String objects are not immutable. This is sort of correct, but the true nature of the String objects immutablity is that the String object is only immutable on the public interface.

2\. Reflection – Reflection can be used to “tinker around under the hood” of the String object. It is advisable not to do this under normal circumstances – in fact I cannot think of an instance where you would want to add code like the above into your codebase.