---
id: 88
title: 'Spring Framework'
date: '2014-04-05T20:22:07+00:00'
author: 'Martin Farrell'
layout: post
guid: 'http://glenware.wordpress.com/?p=88'
permalink: /spring-framework/
geo_public:
    - '0'
thrive_post_fonts:
    - '[]'
categories:
    - Uncategorized
tags:
    - Spring
---

The last post discussed Dependency Injection(DI), and included an example using the Spring framework. Spring is a dependency injection(DI) and aspect oriented(AOP) container.

Its features are –

- Aspect Oriented Programming
- POJO’s
- Reduce boilerplate through templates
- Encourage testability through loose coupling

It supports –

- Setter
- Constructor

**Spring Container**

The core of Spring Container are the BeanFactory or ApplicationContext interfaces. Its is best to think of the ApplicationContext as a more heavyweight version of the BeanFactory

**BeanFactory**

- org.springframework.beans.factory.BeanFactory interface
- Basic unit of dependency injection
- FactoryPattern applying DI
- Collection of beans and associations between beans
- Deals with initialization and destruction of Beans
- Example – **XMLBeanFactory**

**ApplicationContext**

- org.springframework.context.ApplicationContext
- Enterprise specific container
- Extends on BeanFactory
- Support for application lifecycle events, internationalization and validation
- Examples –

- **ClassPathXmlApplicationContext**
- **FileSystemXmlApplicationContext**
- **XmlWebApplicationContext**

**Bean Lifecycle**

- Instantiate Bean
- Inject Value and Bean References into bean properties
- If BeanNameAware – setBeanName()
- If BeanFactoryAware – setBeanFactory()
- If ApplicationContextAware – setApplicationContext
- If BeanPostProcessor interface – postProcessBeforeInitialization
- If InitializingBean interface – afterPropertiesSet – or init-method
- If BeanPostProcessor – postProcessAfterInitialization() – Bean Remains in ApplicationContext until ApplicationContext destroyed
- If DisposableBean interface – destroy() methods or destroy-method