---
id: 366
title: 'Apache CXF &#8211; JMS Queue'
date: '2015-01-12T22:10:25+00:00'
author: 'Martin Farrell'
layout: post
guid: 'https://glenware.wordpress.com/?p=366'
permalink: /apache-cxf-jms-queue/
geo_public:
    - '0'
thrive_post_fonts:
    - '[]'
categories:
    - Spring
    - 'Web Services'
tags:
    - 'Apache CXF'
    - CXF
---

This sample(samples\\java\_first\_jms) demonstrates how to connect to web services across JMS transport, instead of HTTP as in the previous examples.

**Web Service Contract – jms\_greeter.wsdl**

- &lt;wsdl:operation name=”greetMe”&gt;
- &lt;wsdl:operation name=”sayHi”&gt;
- &lt;wsdl:operation name=”greetMeOneWay”&gt;

The wsdl:binding contains the important information that this sample will operate on JMS –

- &lt;soap:binding style=”document” transport=”http://cxf.apache.org/transports/jms”/&gt;

Finally the wsdl:port definition defines the JMS Queue properties –

\[sourcecode lang=”xml”\] &lt;wsdl:service name="JMSGreeterService"&gt;  
&lt;wsdl:port binding="tns:JMSGreeterPortBinding" name="GreeterPort"&gt;  
&lt;jms:address  
destinationStyle="queue"  
jndiConnectionFactoryName="ConnectionFactory"  
jndiDestinationName="dynamicQueues/test.cxf.jmstransport.queue"&gt;  
&lt;jms:JMSNamingProperty name="java.naming.factory.initial" value="org.apache.activemq.jndi.ActiveMQInitialContextFactory"/&gt;  
&lt;jms:JMSNamingProperty name="java.naming.provider.url" value="tcp://localhost:61616"/&gt;  
&lt;/jms:address&gt;  
&lt;jms:clientConfig useConduitIdSelector="false"/&gt;  
&lt;/wsdl:port&gt;  
&lt;/wsdl:service&gt;  
\[/sourcecode\] The implementation classes can be generated through the maven wsdl2java task.

**Service Implementation**

GreeterJMSImpl implements the JMSGreeterPortType interface –

\[sourcecode lang=”java”\] @javax.jws.WebService(portName = &amp;amp;quot;GreeterPort&amp;amp;quot;,  
 serviceName = &amp;amp;quot;JMSGreeterService&amp;amp;quot;,  
 targetNamespace = &amp;amp;quot;http://cxf.apache.org/jms\_greeter&amp;amp;quot;,  
 endpointInterface = &amp;amp;quot;org.apache.cxf.jms\_greeter.JMSGreeterPortType&amp;amp;quot;,  
 wsdlLocation = &amp;amp;quot;file:./wsdl/jms\_greeter.wsdl&amp;amp;quot;)  
public class GreeterJMSImpl implements JMSGreeterPortType {  
//…  
}  
\[/sourcecode\] - portName = “GreeterPort” –&gt; maps to wsdl:port attribute – name
- serviceName = “JMSGreeterService” –&gt; maps to wsdl:service attribute – name
- targetNamespace = “http://cxf.apache.org/jms\_greeter” –&gt; wsdl:definitions attribute targetNamespace
- endpointInterface = “org.apache.cxf.jms\_greeter.JMSGreeterPortType” –&gt; Interface that is implemente
- wsdlLocation = “file:./wsdl/jms\_greeter.wsdl” –&gt; wsdl location on disc

**Client**

The Client class demonstrates how simple it is to call a JMS service using the wsdl URI –

\[sourcecode lang=”java”\] JMSGreeterService service = new JMSGreeterService(wsdl.toURI().toURL(), SERVICE\_NAME);  
JMSGreeterPortType greeter = (JMSGreeterPortType)service.getPort(PORT\_NAME, JMSGreeterPortType.class); System.out.println(&amp;amp;quot;Invoking sayHi…&amp;amp;quot;);  
System.out.println(&amp;amp;quot;server responded with: &amp;amp;quot; + greeter.sayHi());  
System.out.println();

System.out.println(&amp;amp;quot;Invoking greetMe…&amp;amp;quot;);  
System.out.println(&amp;amp;quot;server responded with: &amp;amp;quot; + greeter.greetMe(System.getProperty(&amp;amp;quot;user.name&amp;amp;quot;)));  
System.out.println();

System.out.println(&amp;amp;quot;Invoking greetMeOneWay…&amp;amp;quot;);  
greeter.greetMeOneWay(System.getProperty(&amp;amp;quot;user.name&amp;amp;quot;));  
System.out.println(&amp;amp;quot;No response from server as method is OneWay&amp;amp;quot;);  
System.out.println();  
\[/sourcecode\]

**ActiveMQ**

The demonstration stores the message queue in Memory –

\[sourcecode lang=”java”\] public final class EmbeddedBroker {  
 private EmbeddedBroker() { }  public static void main(String\[\] args) throws Exception {  
 BrokerService broker = new BrokerService();  
 broker.setPersistenceAdapter(new MemoryPersistenceAdapter());  
 broker.setDataDirectory(&amp;amp;quot;target/activemq-data&amp;amp;quot;);  
 broker.addConnector(&amp;amp;quot;tcp://localhost:61616&amp;amp;quot;);  
 broker.start();  
 System.out.println(&amp;amp;quot;JMS broker ready …&amp;amp;quot;);  
 Thread.sleep(125 \* 60 \* 1000);  
 System.out.println(&amp;amp;quot;JMS broker exiting&amp;amp;quot;);  
 broker.stop();  
 System.exit(0);  
 }  
}  
\[/sourcecode\]

**Running The Example**

mvn install (this will build the demo)

In separate command windows/shells:

mvn -Pjms.broker  
mvn -Pserver  
mvn -Pclient

With the output –

\[sourcecode\] INFO: Creating Service {http://cxf.apache.org/jms\_greeter}JMSGreeterService from  
 WSDL: file:/…/apache-cxf-3.0.3/samples/jms\_queue/src/main/config/jms\_  
greeter.wsdl  
Invoking sayHi…  
server responded with: Bonjour Invoking greetMe…  
server responded with: Hello Martin

Invoking greetMeOneWay…  
No response from server as method is OneWay

Invoking sayHi with JMS Context information …  
server responded with: Bonjour  
Received expected contents in response context  
\[/sourcecode\]