---
id: 1111
title: 'Why do we still create Util classes?'
date: '2016-12-13T13:52:31+00:00'
author: 'Martin Farrell'
layout: post
guid: 'https://glenware.wordpress.com/?p=1111'
permalink: /why-do-we-still-create-util-classes/
publicize_twitter_user:
    - glenwareltd
thrive_post_fonts:
    - '[]'
categories:
    - Java
---

Its been niggling at me for a while, a few years even, but why do we insist on creating our own util classes like StringUtil’s, DateUtil’s or CollectionUtil’s when we have great open source projects that contain these classes and methods? In fact recently I foolishly created a CSVUtil class instead of using [apache commons csv](https://commons.apache.org/proper/commons-csv/) or [opencsv](http://opencsv.sourceforge.net/)

The most common method you see is a StringUtil.isNullOrEmpty. It will be similar to –

```
public static boolean isNullOrEmpty(String myString) {
   if (myString == null || "".equals(myString)) {
      return true;
   } else {
      return false;
   }
}
```

Its not bad code, and keeps you DRY (Dont Repeat Yourself) from repeated null or empty string checks.

But the problem is –

- Reinventing the wheel because there are a lot of great libraries providing this util and more
- Created an artifact that you need to support, write unit tests
- Impact on code coverage – if you dont unit test then you get a black mark against your code coverage
- Chance you have introduced a bug

One reason we see code like this is they are an organisational software legacy, where in the pre-Java8 world we checked for null as we didnt have Optional classes.

## What to use instead?

- [Apache Commons](http://commons.apache.org) – In my experience this is the most common set of utilities, and probably closely match the util they would replace
- [Google Guava](https://github.com/google/guava) – These are newer than Apache Commons, although are now well established. The approach is slightly different and allow you to method chain. This might suit your coding style better

## Selection

The choice of library isnt as important as consistency – so try not to mix apache commons in one part of the code, with the guava implementation in another part of the code. This will make future maitenance easier

## Caveat

As with all the best rules there are times when you need to break them. I would create a util class when –

- Specific Implementation – Package I’m using doesnt contain the method I need, but I would try to use the existing util as the basis for this
- pre-Java8 – I might create a util as a facade on say Date’s or because I dont have Optional
- *Procrastination* – The final reason for creating the util is to procrastinate, and avoid my main task! I create the util then at least I can feel productive at my next standup 🙂

So my New Years resolution for 2017 is to stop creating these classes and use tried and tested utils, and try and replace them when I see them!