---
id: 61
title: 'Comparable vs Comparator Comparison'
date: '2014-01-29T12:27:48+00:00'
author: 'Martin Farrell'
layout: post
guid: 'http://glenware.wordpress.com/?p=61'
permalink: /comparable-vs-comparator-comparison/
geo_public:
    - '0'
thrive_post_fonts:
    - '[]'
categories:
    - CoreJava
tags:
    - comparable
    - comparator
    - 'Core Java'
---

**java.lang.Comparable**

\[sourcecode language=”java”\] Comparable.compareTo(Object toCompare)

\[/sourcecode\] - Natural ordering – eg List of Users ordered by Id
- Implemented against class
- Sorting Method – int compareTo(Object o1)
- CurrentObject vs compareToObject
- compareTo return –

 1. positive <span style="font-size:small;"><span style="font-family:Tahoma;font-size:small;"><span style="font-family:Tahoma;font-size:small;">–</span></span><span style="font-size:small;"> if ( currentObject &gt; compareToObject)</span></span>

 2. zero <span style="font-size:small;"><span style="font-family:Tahoma;font-size:small;"><span style="font-family:Tahoma;font-size:small;">–</span></span><span style="font-size:small;"> if ( currentObject == compareToObject)</span></span>

 3. negative <span style="font-size:small;"><span style="font-family:Tahoma;font-size:small;"><span style="font-family:Tahoma;font-size:small;">–</span></span><span style="font-size:small;"> if ( currentObject &lt; compareToObject)</span></span>

\[sourcecode language=”java”\] public class Bicycle implements Comparable {  private int size;

 public void setSize(int size) {

 this.size = size;

 }

 public int getSize() {

 return size;

 }

 @Override  
 public int compareTo(Object arg0) {

 Bicycle bicycleCompareTo =(Bicycle) arg0;

 if ( this.bicycleId &gt; bicycleCompareTo.bicycleId ) {

 return 1;

 } else if ( this.bicycleId &lt; bicycleCompareTo.bicycleId ) {

 return -1;

 } else {

 return 0;

 }

 }

}  
\[/sourcecode\]

- Calling method –

\[sourcecode language=”java”\] Collections.sort(List)

\[/sourcecode\] **java.util.Comparator**

- Alternative ordering – eg List of Bicycles ordered by BikeSize
- Multiple Comparators

\[sourcecode language=”java”\] Comparator.compare(Object obj1, Object obj2)

\[/sourcecode\] - Nested static classes
- Sorting Method – int compare(Object o1, Object o2)

 1. positive –<span style="font-size:small;"><span style="font-size:small;"> if ( o1 &gt; o2 )</span></span>

 2. zero <span style="font-size:small;"><span style="font-family:Tahoma;font-size:small;"><span style="font-family:Tahoma;font-size:small;">–</span></span><span style="font-size:small;"> if ( o1 == o2 )</span></span>

 3. negative <span style="font-size:small;"><span style="font-family:Tahoma;font-size:small;"><span style="font-family:Tahoma;font-size:small;">–</span></span><span style="font-size:small;"> if ( o1 &lt; o2 )</span></span>

- Calling –

\[sourcecode language=”java”\] Collections.sort(List, Comparator)

\[/sourcecode\] - Example –

\[sourcecode language=”java”\] public class BicycleSizeComparator implements Comparator&lt;Bicycle&gt;{  @Override  
 public int compare(Bicycle bicycle1, Bicycle bicycle2) {

 if ( bicycle1.getSize() &gt; bicycle2.getSize() ) {

 return 1;

 } else if ( bicycle1.getSize() &lt; bicycle2.getSize() ) {

 return -1;

 } else {

 return 0;

 }

 }

}  
\[/sourcecode\]