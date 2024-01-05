---
id: 374
title: 'Apache CXF &#8211; Contract Last Example'
date: '2015-01-12T21:49:33+00:00'
author: 'Martin Farrell'
layout: post
guid: 'https://glenware.wordpress.com/?p=374'
permalink: /apache-cxf-contract-last-example/
thrive_post_fonts:
    - '[]'
categories:
    - Spring
    - 'Web Services'
tags:
    - 'Apache CXF'
    - 'Contract Last'
---

This post looks at the JAX-WS “contract last”/”code first” implementation in samples\\java\_first\_jaxws. This approach means that you create the Java code first, then create or generate the WSDL from that

**Web Service Code**

The web service uses JAX-WS annotations to annotate the interface and implementation. This generates the wsdl at runtime

**Interface**

\[sourcecode lang=”java”\] @WebService  
public interface HelloWorld {  String sayHi(String text);

 String sayHiToUser(User user);

 @XmlJavaTypeAdapter(IntegerUserMapAdapter.class)  
 Map&lt;Integer, User&gt; getUsers();  
}  
\[/sourcecode\]

Implementation –

\[sourcecode lang=”java”\] @WebService(endpointInterface = "demo.hw.server.HelloWorld", serviceName = "HelloWorld")  
public class HelloWorldImpl implements HelloWorld {  
 Map&lt;Integer, User&gt; users = new LinkedHashMap&lt;Integer, User&gt;();  public String sayHi(String text) {  
 System.out.println("sayHi called");  
 return "Hello " + text;  
 }

 public String sayHiToUser(User user) {  
 System.out.println("sayHiToUser called");  
 users.put(users.size() + 1, user);  
 return "Hello " + user.getName();  
 }

 public Map&lt;Integer, User&gt; getUsers() {  
 System.out.println("getUsers called");  
 return users;  
 }

}  
\[/sourcecode\]

@WebService annotation denotes the class as a JAX-WS web service class. The parameters to this method are –

- endpointInterface – Complete name of web service endpoint
- name – Name of Web Service – maps to wsdl:portType
- portName – Maps to wsdl:port
- serviceName – Service endpont interface defined in Web Service contract
- targetNamespace – Used for wsdl:portType and/or wsdl:service
- wsdlLocation – Location of wsdl

The sayHi method takes a String, which is supported by JAX-WS, but we have to use XmlAdapter’s to support the User and Map objects in the other two methods

**UserXmlAdapter**

The User interface associates itself to the UserAdapter through the XmlJavaTypeAdapter annotation –

\[sourcecode lang=”java”\] @XmlJavaTypeAdapter(UserAdapter.class)  
public interface User {  
 String getName();  
}  
\[/sourcecode\] The User interface has its implementation under UserImpl, which links itself to the User type through the XmlType annotation –

\[sourcecode lang=”java”\] @XmlType(name = "User")  
public class UserImpl implements User {  
 //…  
}  
\[/sourcecode\] The mapping between the interface and implementation is then done through the XmlAdapter –

\[sourcecode lang=”java”\] public class UserAdapter extends XmlAdapter&lt;UserImpl, User&gt; {  
 public UserImpl marshal(User v) throws Exception {  
 if (v instanceof UserImpl) {  
 return (UserImpl)v;  
 }  
 return new UserImpl(v.getName());  
 }  public User unmarshal(UserImpl v) throws Exception {  
 return v;  
 }  
}  
\[/sourcecode\]

**Map XmlAdapter**

The getUsers method is interesting because it shows how XmlAdapter can be used as a workaround for JAXB not supporting Maps –

\[sourcecode lang=”java”\] public class IntegerUserMapAdapter extends XmlAdapter&lt;IntegerUserMap, Map&lt;Integer, User&gt;&gt; {  
 public IntegerUserMap marshal(Map&lt;Integer, User&gt; v) throws Exception {  
 IntegerUserMap map = new IntegerUserMap();  
 for (Map.Entry&lt;Integer, User&gt; e : v.entrySet()) {  
 IntegerUserMap.IntegerUserEntry iue = new IntegerUserMap.IntegerUserEntry();  
 iue.setUser(e.getValue());  
 iue.setId(e.getKey());  
 map.getEntries().add(iue);  
 }  
 return map;  
 }  public Map&lt;Integer, User&gt; unmarshal(IntegerUserMap v) throws Exception {  
 Map&lt;Integer, User&gt; map = new LinkedHashMap&lt;Integer, User&gt;();  
 for (IntegerUserMap.IntegerUserEntry e : v.getEntries()) {  
 map.put(e.getId(), e.getUser());  
 }  
 return map;  
 }  
}  
\[/sourcecode\]

**CXFServlet and Spring Integration**

The connectivity of the CXFServlet was covered in the previous post, and this section looks at how the cxf service is configured in cxf-servlet.xml –

\[sourcecode lang=”xml”\] &lt;jaxws:server id="jaxwsService" serviceClass="demo.hw.server.HelloWorld" address="/hello\_world"&gt;  
 &lt;jaxws:serviceBean&gt;  
 &lt;bean class="demo.hw.server.HelloWorldImpl" /&gt;  
 &lt;/jaxws:serviceBean&gt;  
&lt;/jaxws:server&gt;  
\[/sourcecode\] This connects in 3 key attributes –

- serviceClass – HelloWorld interface
- address – /hello\_world – serving Url
- serviceBean – HelloWorldImpl – implementation class

**Running the example**

The simplest way to run the example is through –

mvn clean install (builds the demo and creates a WAR file for optional Tomcat deployment)  
mvn -Pserver (from one command line window — only if using a non-WAR standalone service)  
mvn -Pclient (from a second command line window)

Or run in tomcat by dropping the war in the deploy directory, and accessing on –

http://localhost:8080/java\_first\_jaxws/services/hello\_world?wsdl

This can be called from SOAPUI –

\[sourcecode lang=”xml”\] &lt;soapenv:Envelope xmlns:soapenv="http://schemas.xmlsoap.org/soap/envelope/" xmlns:ser="http://server.hw.demo/"&gt;  
 &lt;soapenv:Header/&gt;  
 &lt;soapenv:Body&gt;  
 &lt;ser:sayHiToUser&gt;  
 &lt;!–Optional:–&gt;  
 &lt;arg0&gt;  
 &lt;!–Optional:–&gt;  
 &lt;name&gt;Martin&lt;/name&gt;  
 &lt;/arg0&gt;  
 &lt;/ser:sayHiToUser&gt;  
 &lt;/soapenv:Body&gt;  
&lt;/soapenv:Envelope&gt;  
\[/sourcecode\] You will get the output –

\[sourcecode lang=”xml”\] &lt;soap:Envelope xmlns:soap="http://schemas.xmlsoap.org/soap/envelope/"&gt;  
 &lt;soap:Body&gt;  
 &lt;ns2:sayHiToUserResponse xmlns:ns2="http://server.hw.demo/"&gt;  
 &lt;return&gt;Hello Martin&lt;/return&gt;  
 &lt;/ns2:sayHiToUserResponse&gt;  
 &lt;/soap:Body&gt;  
&lt;/soap:Envelope&gt;  
\[/sourcecode\] 