---
id: 548
title: 'Dropwizard Microservices'
date: '2016-04-12T22:06:45+00:00'
author: 'Martin Farrell'
layout: post
guid: 'https://glenware.wordpress.com/?p=548'
permalink: /dropwizard-microservices/
geo_public:
    - '0'
thrive_post_fonts:
    - '[]'
categories:
    - Uncategorized
tags:
    - dropwizard
    - microservices
---

I had read about [Dropwizard](http://www.dropwizard.io) when I started using [Spring Boot](http://projects.spring.io/spring-boot), but decided to stick with Spring as I was more familiar with Spring. Then I read [Building Microservices](http://shop.oreilly.com/product/0636920033158.do) book, by Sam Reilly, and he recommended it for microservices development. So I thought I should look at dropwizard again, and learn more about it.

**What is dropwizard?**

Dropwizard provided the inspiration for spring boot. It is a collection of what it considers best-of-breed libraries and provides –

*“out-of-the-box support for sophisticated configuration, application metrics, logging, operational tools, and much more”*

At its core is –

- [Jetty](http://www.eclipse.org/jetty)
- [Jersey](https://jersey.java.net)
- [Jackson](http://wiki.fasterxml.com/JacksonHome)
- [Guava](https://github.com/google/guava) – google core libraries

It also includes [metrics](http://metrics.codahale.com), [Logback](http://logback.qos.ch) and [slf4j](http://www.slf4j.org), [Liquidbase](http://www.liquibase.org), [Freemarker](http://freemarker.sourceforge.net) and [Mustache](http://mustache.github.io) and [Joda Time](http://joda-time.sourceforge.net)

The final product is a single big jar

**Sample Project**

dropwizard recommends maven so I’ll stick with that. Not much to our basic pom – just a dropwizard dependency –

\[sourcecode lang=”xml”\] &lt;dependency&gt;  
 &lt;groupId&gt;io.dropwizard&lt;/groupId&gt;  
 &lt;artifactId&gt;dropwizard-core&lt;/artifactId&gt;  
 &lt;version&gt;0.9.2&lt;/version&gt;  
 &lt;/dependency&gt;  
\[/sourcecode\] We then use the shade plugin to create our big jar –  
\[sourcecode lang=”xml”\] &lt;plugin&gt;  
 &lt;groupId&gt;org.apache.maven.plugins&lt;/groupId&gt;  
 &lt;artifactId&gt;maven-shade-plugin&lt;/artifactId&gt;  
 &lt;configuration&gt;  
 &lt;createDependencyReducedPom&gt;true&lt;/createDependencyReducedPom&gt;  
 &lt;filters&gt;  
 &lt;filter&gt;  
 &lt;artifact&gt;\*:\*&lt;/artifact&gt;  
 &lt;excludes&gt;  
 &lt;exclude&gt;META-INF/\*.SF&lt;/exclude&gt;  
 &lt;exclude&gt;META-INF/\*.DSA&lt;/exclude&gt;  
 &lt;exclude&gt;META-INF/\*.RSA&lt;/exclude&gt;  
 &lt;/excludes&gt;  
 &lt;/filter&gt;  
 &lt;/filters&gt;  
 &lt;transformers&gt;  
 &lt;transformer implementation="org.apache.maven.plugins.shade.resource.ServicesResourceTransformer" /&gt;  
 &lt;transformer implementation="org.apache.maven.plugins.shade.resource.ManifestResourceTransformer"&gt;  
 &lt;mainClass&gt;com.glenware.dropwizard.TodaysDateApplication&lt;/mainClass&gt;  
 &lt;/transformer&gt;  
 &lt;/transformers&gt;  
 &lt;/configuration&gt;  
 &lt;executions&gt;  
 &lt;execution&gt;  
 &lt;phase&gt;package&lt;/phase&gt;  
 &lt;goals&gt;  
 &lt;goal&gt;shade&lt;/goal&gt;  
 &lt;/goals&gt;  
 &lt;/execution&gt;  
 &lt;/executions&gt;  
 &lt;/plugin&gt;  
\[/sourcecode\]

The core of the application is the “Application” class –  
\[sourcecode lang=”java”\] public class TodaysDateApplication extends Application&lt;TodaysDateConfiguration&gt; {  
 public static void main(String\[\] args) throws Exception {  
 new TodaysDateApplication().run(args);  
 }

 @Override  
 public String getName() {  
 return "TodaysDate-microservice";  
 }

 @Override  
 public void initialize(Bootstrap&lt;TodaysDateConfiguration&gt; bootstrap) {  
 }

 @Override  
 public void run(TodaysDateConfiguration configuration, Environment environment) throws Exception {  
 environment.jersey().register(new TodaysDateResource());  
 environment.healthChecks().register("TodaysDateHC", new TodaysDateHealthCheck());  
 }  
}  
\[/sourcecode\]

The key points are –

- The main method starts an instance of the jetty server
- The run method then sets up two parameters
- REST server, TodaysDateResource
- Healthcheck class, TodaysDateHealthCheck. This isn’t necessary but you do get a warning on startup if you don’t set one

The configuration class itself is just a placeholder –

\[sourcecode lang=”java”\] public class TodaysDateConfiguration extends Configuration {  
}  
\[/sourcecode\] We can add our own configuration here to accept parameters from the yml config file.

The healthcheck class is also quite basic, and just increments a count on the number of times the service is called –

\[sourcecode lang=”java”\] public class TodaysDateHealthCheck extends HealthCheck {  
 @Override  
 protected Result check() throws Exception {  
 return Result.healthy();  
 }  
}  
\[/sourcecode\] Finally we get to the core of the application –

\[sourcecode lang=”java”\] @Path("/todaysdate")  
@Produces(MediaType.APPLICATION\_JSON)  
public class TodaysDateResource {  
 @GET  
 public String todaysDate() {  
 return "{\\"todaysdate\\": " + new DateTime().toString("dd/MM/yyyy") + "}";  
 }  
}  
\[/sourcecode\] This is simply a Jersey REST application –

- Serves on path /todaysdate, and returns a JSON message like – { “todaysdate” : “12/04/2016” }

There is one additional configuration file – config.yml –

\[sourcecode\] logging:  
 level: INFO server:  
 applicationConnectors:  
 – type: http  
 port: 8080  
 adminConnectors:  
 – type: http  
 port: 8001  
\[/sourcecode\]

The obvious question is what is, and why [YML](http://www.yaml.org). Im told its human-friendly configuration, and to be honest it doesnt bother me whether config is xml, properties or whatever the framework is using as long as I can get the information I need. In this example I’ve included some config to allow you to change ports

**Putting it all together**

`mvn clean package`

`java -jar target/dropwizard-dates-microservice-0.0.1-SNAPSHOT.jar server src/main/resources/server-config.yml`

You can then access the service through your browser at –

http://localhost:8080/todaysdate

\[sourcecode lang=”json”\]{"todaysdate": 12/04/2016 }\[/sourcecode\] http://localhost:8081/healthcheck?pretty=true

\[sourcecode lang=”json”\] {  
 "TodaysDateHC" : {  
 "healthy" : true  
 },  
 "deadlocks" : {  
 "healthy" : true  
 }  
}\[/sourcecode\] **Conclusions**

This example barely scrapes the surface of dropwizard, but half the battle of coding is to get a HelloWorld application to compile. Its also too soon to provide a comparison between Spring Boot and dropwizard just yet.

**Source Code**

[dropwizard-microservices](https://github.com/farrelmr/dropwizard-microservices/)