---
id: 424
title: 'Mockito &#8211; Partial Mocks &#8211; mock() and spy()'
date: '2015-01-23T11:46:16+00:00'
author: 'Martin Farrell'
layout: post
guid: 'https://glenware.wordpress.com/?p=424'
permalink: /mockito-partial-mocks-mock-and-spy/
thrive_post_fonts:
    - '[]'
categories:
    - Mockito
tags:
    - mockito
---

**Mockito.mock**

Ok so say I have two methods in my class –

\[sourcecode lang=”java”\] public class ToBeMocked {  public String methodOne() {  
 return "methodOne calls methodTwo – " + this.methodTwo();  
 }

 public String methodTwo() {  
 return "Hi Im methodTwo";  
 }

}  
\[/sourcecode\]

I don’t want to test methodTwo, so I need to stub it in my test class. One approach is –

\[sourcecode lang=”java”\] import static org.junit.Assert.\*;  
import static org.mockito.Matchers.\*;  
import static org.mockito.Mockito.\*; import org.junit.\*;  
import org.mockito.\*;

public class ToBeMockedTest {

 @Test  
 public void methodOneMockTest() {  
 ToBeMocked toBeMocked = mock(ToBeMocked.class);

 when(toBeMocked.methodTwo()).thenReturn("Hi Im now mocked");  
 when(toBeMocked.methodOne()).thenCallRealMethod();

 assertEquals("methodOne calls methodTwo – Hi Im now mocked", toBeMocked.methodOne());  
 }

}  
\[/sourcecode\]

The mock method creates an empty object through the cglib, the thenCallRealMethod object then calls the real implementation of the method we want to test. This will result in an output of “methodOne calls methodTwo – Hi Im now mocked”

**Mockito – spy**

Spy can do a number of things –

- Track object interactions
- Partial mocking

This post is interested in the use of spy for partial mocking

\[sourcecode lang=”java”\] import static org.junit.Assert.\*;  
import static org.mockito.Matchers.\*;  
import static org.mockito.Mockito.\*; import org.junit.\*;  
import org.mockito.\*;

public class ToBeMockedTest {

 @Test  
 public void methodOneSpyTest() {  
 ToBeMocked toBeMocked = new ToBeMocked();  
 ToBeMocked spyToBeMocked = Mockito.spy(toBeMocked);

 Mockito.doReturn("Hi Im now spied").when(spyToBeMocked).methodTwo();

 String methodOne = toBeMocked.methodOne();  
 assertEquals("methodOne calls methodTwo – Hi Im now spied", spyToBeMocked.methodOne());  
 }

}  
\[/sourcecode\]

The spy method wraps the existing object, allowing you to control the output of the methods you require. Here the methodTwo implementation can be made to return “methodOne calls methodTwo – Hi Im now spied”