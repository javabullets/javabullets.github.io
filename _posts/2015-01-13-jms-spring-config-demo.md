---
id: 381
title: 'Apache CXF &#8211; JMS Spring Config'
date: '2015-01-13T12:47:06+00:00'
author: 'Martin Farrell'
layout: post
guid: 'https://glenware.wordpress.com/?p=381'
permalink: /jms-spring-config-demo/
thrive_post_fonts:
    - '[]'
categories:
    - Spring
    - 'Web Services'
tags:
    - 'Apache CXF'
    - 'Contract Last'
---

The JMS Spring Config Demo (samples\\jms\_spring\_config) uses the wsdl\_first code, and adds JMS transport through configuration

**WSDL**

The WSDL remains unchanged, with the port defined –

\[sourcecode lang=”xml”\] &lt;wsdl:service name="CustomerServiceService"&gt;  
 &lt;wsdl:port name="CustomerServicePort" binding="tns:CustomerServiceServiceSoapBinding"&gt;  
 &lt;soap:address location="http://localhost:9090/CustomerServicePort"/&gt;  
 &lt;/wsdl:port&gt;  
&lt;/wsdl:service&gt;  
\[/sourcecode\] **Spring Configuration**

JMS is configured through the server-applicationContext.xml –

\[sourcecode lang=”xml”\] &lt;bean id="jmsConnectionFactory" class="org.springframework.jms.connection.SingleConnectionFactory"&gt;  
 &lt;property name="targetConnectionFactory"&gt;  
 &lt;bean class="org.apache.activemq.ActiveMQConnectionFactory"&gt;  
 &lt;property name="brokerURL" value="tcp://localhost:61616" /&gt;  
 &lt;/bean&gt;  
 &lt;/property&gt;  
&lt;/bean&gt; &lt;bean id="jmsConfig" class="org.apache.cxf.transport.jms.JMSConfiguration"  
 p:connectionFactory-ref="jmsConnectionFactory"  
 p:targetDestination="test.queue"  
/&gt;

&lt;!– JMS Endpoint –&gt;  
&lt;jaxws:endpoint xmlns:customer="http://customerservice.example.com/"  
 id="CustomerServiceHTTP" address="jms://"  
 implementor="com.example.customerservice.server.CustomerServiceImpl"&gt;  
 &lt;jaxws:features&gt;  
 &lt;bean class="org.apache.cxf.feature.LoggingFeature" /&gt;  
 &lt;bean class="org.apache.cxf.transport.jms.JMSConfigFeature" p:jmsConfig-ref="jmsConfig" /&gt;  
 &lt;/jaxws:features&gt;  
&lt;/jaxws:endpoint&gt;  
\[/sourcecode\]

There are 3 point of connectivity –

- jmsConnectionFactory – Inject ActiveMQConnectionFactory into Spring’s JMS SingleConnectionFactory
- jmsConfig – Inject the jmsConnectionFactory and queue name
- jaxws:endpoint – Inject jmsConfig into the jaxws:endpoint