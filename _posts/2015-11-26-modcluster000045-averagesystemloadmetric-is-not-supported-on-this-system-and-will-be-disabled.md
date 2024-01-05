---
id: 488
title: 'MODCLUSTER000045: AverageSystemLoadMetric is not supported on this system and will be disabled.'
date: '2015-11-26T13:25:10+00:00'
author: 'Martin Farrell'
layout: post
guid: 'https://glenware.wordpress.com/?p=488'
permalink: /modcluster000045-averagesystemloadmetric-is-not-supported-on-this-system-and-will-be-disabled/
geo_public:
    - '0'
thrive_post_fonts:
    - '[]'
categories:
    - Uncategorized
---

Turns out cpu isn’t a valid load-metric on jboss wildfly on windows

Standalone.xml  
\[sourcecode lang=”xml”\] &lt;subsystem xmlns="urn:jboss:domain:modcluster:1.2"&gt;  
 &lt;mod-cluster-config advertise-socket="modcluster" proxy-list="localhost:1234" advertise="false" ttl="10" connector="ajp"&gt;  
 &lt;dynamic-load-provider&gt;  
 &lt;load-metric type="cpu"/&gt;  
 &lt;/dynamic-load-provider&gt;  
 &lt;/mod-cluster-config&gt;  
 &lt;/subsystem&gt;  
 \[/sourcecode\]

Logs –

MODCLUSTER000001: Initializing mod\_cluster version 1.3.0.Final  
MODCLUSTER000045: AverageSystemLoadMetric is not supported on this system and will be disabled.

The list of valid values is defined under –

https://github.com/wildfly/wildfly/blob/master/mod\_cluster/extension/src/main/resources/schema/jboss-as-mod-cluster\_2\_0.xsd#L267

\[sourcecode lang=”xml”\] &lt;xs:simpleType name="loadMetricEnum"&gt;  
 &lt;xs:restriction base="xs:token"&gt;  
 &lt;xs:enumeration value="cpu"/&gt;  
 &lt;xs:enumeration value="mem"&gt;  
 &lt;xs:annotation&gt;  
 &lt;xs:documentation&gt;Deprecated.&lt;/xs:documentation&gt;  
 &lt;/xs:annotation&gt;  
 &lt;/xs:enumeration&gt;  
 &lt;xs:enumeration value="heap"/&gt;  
 &lt;xs:enumeration value="sessions"/&gt;  
 &lt;xs:enumeration value="requests"/&gt;  
 &lt;xs:enumeration value="send-traffic"/&gt;  
 &lt;xs:enumeration value="receive-traffic"/&gt;  
 &lt;xs:enumeration value="busyness"/&gt;  
 &lt;/xs:restriction&gt;  
 &lt;/xs:simpleType&gt;  
\[/sourcecode\] Change it to request –

\[sourcecode lang=”xml”\] &lt;subsystem xmlns="urn:jboss:domain:modcluster:1.2"&gt;  
 &lt;mod-cluster-config advertise-socket="modcluster" proxy-list="localhost:1234" advertise="false" ttl="10" connector="ajp"&gt;  
 &lt;dynamic-load-provider&gt;  
 &lt;load-metric type="requests"/&gt;  
 &lt;/dynamic-load-provider&gt;  
 &lt;/mod-cluster-config&gt;  
 &lt;/subsystem&gt;  
\[/sourcecode\] After –

MODCLUSTER000001: Initializing mod\_cluster version 1.3.0.Final  
9MODCLUSTER000032: Listening to proxy advertisements on /localhost:23364