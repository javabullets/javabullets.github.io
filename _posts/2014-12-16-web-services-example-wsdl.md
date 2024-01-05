---
id: 353
title: 'Web Services &#8211; Example WSDL'
date: '2014-12-16T15:31:34+00:00'
author: 'Martin Farrell'
layout: post
guid: 'http://glenware.wordpress.com/?p=353'
permalink: /web-services-example-wsdl/
thrive_post_fonts:
    - '[]'
categories:
    - 'Web Services'
tags:
    - wsdl
---

A WSDL contract defines the following Web Service information –

- Web Service Data Type
- Web Service messages
- Web Service interfaces
- Bindings between the messages and the representation of the data
- Transport details for the web services.

**Example**

\[sourcecode lang=”xml”\] &lt;?xml version="1.0" encoding="UTF-8" standalone="no"?&gt;  
&lt;wsdl:definitions name="WSDLExampleApplication"  
 xmlns:wsdl="http://schemas.xmlsoap.org/wsdl/"  
 xmlns:soap="http://schemas.xmlsoap.org/wsdl/soap/"  
 xmlns:xsd="http://www.w3.org/2001/XMLSchema"  
 xmlns:tns="http://glenware.com/webservice/WSDLExampleApplication"  
 targetNamespace="http://glenware.com/webservice/WSDLExampleApplication"  
 xmlns:http="http://schemas.xmlsoap.org/wsdl/http/"  
 xmlns:errors="http://glenware.com/Errors" &gt;  &lt;wsdl:import location="Errors.wsdl" namespace="http://glenware.com/Errors" /&gt;

 &lt;wsdl:types&gt;  
 &lt;xsd:schema&gt;  
 &lt;xsd:import  
 namespace="http://glenware.com/webservice/WSDLExampleApplication"  
 schemaLocation="../schema/WSDLExampleApplication.xsd" /&gt;  
 &lt;/xsd:schema&gt;  
 &lt;/wsdl:types&gt;

 &lt;wsdl:message name="getMyDetailsRequest"&gt;  
 &lt;wsdl:part name="request" element="tns:getMyDetailsRequest"&gt;&lt;/wsdl:part&gt;  
 &lt;/wsdl:message&gt;  
 &lt;wsdl:message name="getMyDetailsResponse"&gt;  
 &lt;wsdl:part name="response" element="tns:getMyDetailsResponse"&gt;&lt;/wsdl:part&gt;  
 &lt;/wsdl:message&gt;

 &lt;wsdl:portType name="WSDLExampleApplicationPort"&gt;  
 &lt;wsdl:operation name="getMyDetails"&gt;  
 &lt;wsdl:input message="tns:getMyDetailsRequest"&gt;&lt;/wsdl:input&gt;  
 &lt;wsdl:output message="tns:getMyDetailsResponse"&gt;&lt;/wsdl:output&gt;  
 &lt;/wsdl:operation&gt;  
 &lt;/wsdl:portType&gt;

 &lt;wsdl:binding name="WSDLExampleApplicationBinding" type="tns:WSDLExampleApplicationPort"&gt;  
 &lt;soap:binding style="document" transport="http://schemas.xmlsoap.org/soap/http" /&gt;  
 &lt;wsdl:operation name="getMyDetails"&gt;  
 &lt;soap:operation  
 soapAction="http://glenware.com/webservice/WSDLExampleApplication/getMyDetails"  
 style="document" /&gt;  
 &lt;wsdl:input&gt;  
 &lt;soap:body use="literal" /&gt;  
 &lt;/wsdl:input&gt;  
 &lt;wsdl:output&gt;  
 &lt;soap:body use="literal" /&gt;  
 &lt;/wsdl:output&gt;  
 &lt;/wsdl:operation&gt;  
 &lt;/wsdl:binding&gt;

 &lt;wsdl:service name="WSDLExampleApplication"&gt;  
 &lt;wsdl:port binding="tns:WSDLExampleApplicationBinding" name="WSDLExampleApplicationPort"&gt;  
 &lt;soap:address location="http://localhost:8181/cxf/webservice/WSDLExampleApplication" /&gt;  
 &lt;/wsdl:port&gt;  
 &lt;/wsdl:service&gt;

&lt;/wsdl:definitions&gt;  
\[/sourcecode\]

This shows the two parts of the wsdl –

- logical part – contains types(*wsdl:types*), message(*wsdl:message*), portType(*wsdl:portType*) elements. Defines service interface and message exchanged
- concrete part – binding(*wsdl:binding*) and service(*wsdl:service*) elements, transportation type

The datatypes are defined in the external “../schema/WSDLExampleApplication.xsd” – this allows types to be shared amongst wsdl’s. This follows standard schema rules –

&gt; Support for simple types (xsd:string, xsd:int, xsd:long, …)  
&gt; Support for complex types –

\[sourcecode lang=”xml”\] &lt;xsd:complexType name="getMyDetailsRequest"&gt;  
 &lt;xsd:sequence&gt;  
 &lt;xsd:element name="nameString" type="xsd:string" /&gt;  
 &lt;xsd:element name="count" type="xsd:int" /&gt;  
 &lt;/xsd:sequence&gt;  
&lt;/xsd:complexType&gt;  
\[/sourcecode\] 