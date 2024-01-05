---
id: 91
title: 'Configuring Spring'
date: '2014-04-08T15:23:12+00:00'
author: 'Martin Farrell'
layout: post
guid: 'http://glenware.wordpress.com/?p=91'
permalink: /configuring-spring/
geo_public:
    - '0'
thrive_post_fonts:
    - '[]'
categories:
    - Uncategorized
tags:
    - 'Dependency Injection'
    - Spring
    - 'Spring Configuration'
---

Spring can be configured through 2 approaches –

- XML
- Annotations

**XML Configuration**

Some examples of configuring beans through xml –

\[sourcecode lang=”xml”\] &lt;beans  
 default-init-method="init"  
 default-destroy-method="destroy"&gt;  &lt;!– Basic coniguration –&gt;  
 &lt;bean id="simpleBean" class="MyClass" /&gt;

 &lt;!– Passing arguments –&gt;

 &lt;!– Constructor – or objects using ref –&gt;  
 &lt;bean id="constructorArgBean" class="MyClass"&gt;  
 &lt;constructor-arg value="constructorArgument"/&gt;  
 &lt;/bean&gt;

 &lt;!– Property – or objects using ref –&gt;  
 &lt;bean id="propertyArgBean" class="MyClass"&gt;  
 &lt;property name="propertyAsArg" value="propertyAsArgExample"/&gt;  
 &lt;property name="propertyAsRef" value="propertyAsRefExample"/&gt;  
 &lt;property name="propertyAsNull"&gt;&lt;null/&gt;&lt;/property&gt;  
 &lt;/bean&gt;

 &lt;!– Constructor and Property –&gt;  
 &lt;bean id="constructorAndPropertyArgBean" class="MyClass"&gt;  
 &lt;constructor-arg value="constructorArgument"/&gt;  
 &lt;property name="propertyAsArg" value="propertyAsArgExample"/&gt;  
 &lt;/bean&gt;

 &lt;!– Factory Method –&gt;  
 &lt;bean id="factoryMethodExample" class="MyClass" factory-method="getInstance" /&gt;

 &lt;!– Property Namespace–&gt;  
 &lt;bean id="propertyNameSpaceExample" class="MyClass" p:varExample="varExample" p:refExample="refExample" /&gt;

 &lt;!– Initialising/Destroying Methods – Can be defaulted in xml header(default-init-method, default-destroy-method) –&gt;  
 &lt;bean id="initDestroyExample" class="MyClass" init-method="init" destroy-method="destroy" /&gt;

 &lt;!– Bean Scope – See Below – default – singleton –&gt;  
 &lt;bean id="prototypeExample" class="MyClass" scope="prototype"/&gt;

 &lt;!– Autowiring – See Below –&gt;  
 &lt;bean id="autowiringExample" class="MyClass" autowire="byName" /&gt;

&lt;/beans&gt;  
\[/sourcecode\]

**Bean Scoping**

\[sourcecode lang=”xml”\] &lt;beans&gt;  &lt;!– Bean Scope Example – default – singleton –&gt;  
 &lt;bean id="prototypeExample" class="MyClass" scope="prototype"/&gt;

&lt;/beans&gt;  
\[/sourcecode\]

- singleton – Single instance per Spring container (default)
- prototype – Bean instantiated everytime requested
- request – Bean definition to HTTP request. Web-capable Spring Context (Spring MVC)
- session – Bean definition to HTTP session. Web-capable Spring Context (Spring MVC)
- global-session – Bean definition to global HTTP session. Only valid in portlet context
- no – no autowiring

**Autowiring**

\[sourcecode lang=”xml”\] &lt;beans&gt;  &lt;!– General Syntax –&gt;  
 &lt;bean id="generalSyntax" class="MyClass" autowire="options" /&gt;

 &lt;!– Autowiring –&gt;  
 &lt;bean id="autowiringExample" class="MyClass" autowire="byName" /&gt;

&lt;/beans&gt;  
\[/sourcecode\]

- none – default
- byName – matches on name/id

\[sourcecode lang=”xml”\] &lt;bean id="propertyArgBean" class="MyClass"&gt;  
 &lt;property name="propertyAsRef" ref="propertyAsRefExample"/&gt;  
 &lt;/bean&gt;  &lt;bean id="propertyAsRef" class="PropertyAsRefClass" /&gt;

 &lt;!– As match by type – automatically matches to propertyAsRef –&gt;  
 &lt;bean id="propertyArgBean" class="MyClass" autowire="byName" /&gt;  
\[/sourcecode\]

- byType – matches on type
- constructor – similar to byType
- autodetect – “Best-fit Autowiring” – constructor first, then byType

**Annotations**

Enabled by –

\[sourcecode lang=”xml”\] &lt;beans&gt;  &lt;!– Enables annotation-based config – only if component-scan not used –&gt;  
 &lt;context:annotation-config/&gt;

 &lt;!– Scan packages for config – can use include-filters, exclude-filters –&gt;  
 &lt;context:component-scan base-package="com.mypackage" /&gt;

&lt;/beans&gt;  
\[/sourcecode\]

**@Autowired**

Candidate for autowiring. Can be applied to –

\[sourcecode lang=”java”\]  // Constructors  
 @Autowired  
 public MyClass(MyProperty myProperty) {  
 super();  
 this.myProperty = myProperty;  
 }

 // Property  
 @Autowired  
 private MyProperty myProperty;

 // Setter Methods  
 @Autowired  
 public void setMyProperty(MyProperty myProperty) {  
 this.myProperty = myProperty;  
 }

\[/sourcecode\] **@Qualifier**

Provides clarification over which dependency to use –

\[sourcecode lang=”java”\] @Autowired  
@Qualifier("myQualifier")  
private MyClass myInstance;  
\[/sourcecode\] **Custom Qualifiers**

\[sourcecode lang=”java”\] import java.lang.annotation.ElementType;  
import java.lang.annotation.Retention;  
import java.lang.annotation.RetentionPolicy;  
import java.lang.annotation.Target;  
import org.springframework.beans.factory.annotation.Qualifier; @Target({ElementType.FIELD,ElementType.PARAMETER,ElementType.TYPE})  
@Retention(RetentionPolicy.RUNTIME)  
@Qualifier  
public @interface MyQualifier {  
}  
\[/sourcecode\]

This can now be used to qualify a class –

\[sourcecode lang=”java”\] @Autowired  
@MyQualifier  
private MyClass myInstance;  
\[/sourcecode\] **Useful links**

http://refcardz.dzone.com/refcardz/spring-annotations

http://refcardz.dzone.com/refcardz/spring-configuration