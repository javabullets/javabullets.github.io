---
id: 2185
title: 'What does Spring DelegatingFilterProxy do?'
date: '2017-05-17T12:35:43+00:00'
author: 'Martin Farrell'
layout: post
guid: 'https://www.javabullets.com/?p=2185'
permalink: /what-does-the-spring-delegatingfilterproxy-do/
thrive_post_fonts:
    - '[]'
categories:
    - Java
    - Spring
    - spring-security
tags:
    - DelegatingFilterProxy
    - Spring
    - 'Spring Security'
---

What does Spring DelegatingFilterProxy do? I had never given it much thought about how Spring Security integrates with web.xml until I had to diagnose an issue involving the DelegatingFilterProxy and my Spring Security configuration.

![Isle Of Harris](https://i0.wp.com/www.javabullets.com/wp-content/uploads/2017/05/steve-taylor-110050.jpg?resize=600%2C342&ssl=1)

# What does Spring DelegatingFilterProxy do?

I knew the starting point is the springSecurityFilterChain which uses the DelegatingFilterProxy, and this would instantiate the Spring Security filters according to my spring configuration –

```

    <filter>
        <filter-name>springSecurityFilterChain</filter-name>
        <filter-class>org.springframework.web.filter.DelegatingFilterProxy</filter-class>
    </filter>
    <filter-mapping>
        <filter-name>springSecurityFilterChain</filter-name>
    </filter-mapping>
```

But what next?

## DelegatingFilterProxy

A look at the javadoc for DelegatingFilterProxy states that –

> Proxy for a standard Servlet Filter, delegating to a Spring-managed bean that implements the Filter interface

It further states that the filter-name corresponds to a bean in the Spring application context.

So in terms of Spring Security the DelegatingFilterProxy will look though the Spring Application Context for a bean “springSecurityFilterChain”. The only requirement for Delegated beans is that they must implement javax.servlet.Filter.

## Initializing the springSecurityFilterChain

The springSecurityFilterChain is initialised in your spring configuration by, which will be passed to your DispatcherServlet –

```
<http ...=""> </http>
```

We can see this in action when we include code to add remove filters in Spring Security –

```

<http ...="">
    ...        
    <custom-filter after="FORM_LOGIN_FILTER" ref="mySecurityFilter"></custom-filter>
    ...
</http>
```

You can also see a list of filters created when you up spring security logging

## What about Spring Boot?

Spring Security configuration for Spring Boot is simply a matter of adding a reference to “spring-boot-starter-security” to gradle or maven. We can then fine tune the security configurations through @EnableWebSecurity and overriding the configure method of a class extending WebSecurityConfigurerAdapter.

If we dig around under the hood we find that the DelegatingFilterProxy is still used. With the “springSecurityFilterChain” instantiated in SecurityFilterAutoConfiguration, which populates the DelegatingFilterProxyRegistrationBean with “springSecurityFilterChain” (AbstractSecurityWebApplicationInitializer.DEFAULT\_FILTER\_NAME) –

```

@Bean
@ConditionalOnBean(name = DEFAULT_FILTER_NAME)
public DelegatingFilterProxyRegistrationBean securityFilterChainRegistration(SecurityProperties securityProperties) {
    DelegatingFilterProxyRegistrationBean registration = new DelegatingFilterProxyRegistrationBean(DEFAULT_FILTER_NAME);
    registration.setOrder(securityProperties.getFilterOrder());
    registration.setDispatcherTypes(getDispatcherTypes(securityProperties));
    return registration;
}
```

## Reference

<http://docs.spring.io/spring-security/site/docs/current/reference/htmlsingle/#delegating-filter-proxy>

<https://spring.io/blog/2013/07/03/spring-security-java-config-preview-web-security/>

<http://docs.spring.io/spring-security/site/docs/current/reference/htmlsingle/#delegating-filter-proxy>