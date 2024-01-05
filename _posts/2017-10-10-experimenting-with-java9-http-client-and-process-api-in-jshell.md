---
id: 2646
title: 'Experimenting with Java9 HTTP Client and Process API in JShell'
date: '2017-10-10T19:43:35+00:00'
author: 'Martin Farrell'
layout: post
guid: 'https://www.javabullets.com/?p=2646'
permalink: /experimenting-with-java9-http-client-and-process-api-in-jshell/
thrive_post_fonts:
    - '[]'
image: /wp-content/uploads/2017/10/IMG_20171007_111155.jpg
categories:
    - Java
    - Java9
tags:
    - java
    - java9
    - 'process API'
---

This post continues my exploration of Java9 features from my [My Top Java 9 Features](https://www.javabullets.com/top-java-9-features/) blog post. Here we are experimenting with Java9 HTTP/2 Client and Process API in [JShell](https://www.javabullets.com/jshell-in-five-minutes/)

## HTTP/2 Client

The HTTP/2 Client is an incubator project in Java9. This means the API isn’t finalized, so has some scope for change in future versions. The most obvious changes from Java9 to Java10 will be moving it from the jdk.incubator.httpclient module to “http.client” module, plus associated package name changes. Its worth keeping this in mind when using the API.

The HTTP/2 doesnt work straight out the box in jshell, but its ok as it lets us see the Java Platform Modularity System(JPMS) in action. We simply pass the httpclient module into jshell using –add-modules –

```
<pre class="">C:\jdk9TestGround>jshell -v --add-modules jdk.incubator.httpclient
```

We then import the http libraries –

```
<pre class="">jshell> import jdk.incubator.http.*;
```

We can now call a website from jshell –

```
<pre class="">jshell> HttpClient httpClient = HttpClient.newHttpClient();
jshell> HttpRequest httpRequest = HttpRequest.newBuilder().uri(new URI("https://www.javabullets.com")).GET().build();
jshell> HttpResponse<string> httpResponse = httpClient.send(httpRequest, HttpResponse.BodyHandler.asString());
jshell> System.out.println(httpResponse.statusCode());
jshell> System.out.println(httpResponse.body());</string>
```

The most interesting feature is the use of the Builder pattern to create the HTTP Request. This is defined in [HttpRequest.Builder](https://docs.oracle.com/javase/9/docs/api/jdk/incubator/http/HttpRequest.Builder.html) which can be used to construct more complex HttpClient requests including authentication.

The syntax is similar to both the [Jetty HttpClient](http://www.eclipse.org/jetty/documentation/9.4.7.v20170914/) and [okhttp](http://square.github.io/okhttp/) which are Http/2 compliant. It is definitely much simpler than the old approach in Java.

Other useful features of this API are –

- Asynchronous Requests – This is more useful than the above example as it is non-blocking. This is done through the [HttpRequest.sendAsync](http://download.java.net/java/jdk9/docs/api/jdk/incubator/http/HttpClient.html#sendAsync-jdk.incubator.http.HttpRequest-jdk.incubator.http.HttpResponse.BodyHandler-) method
- WebSockets – This is created through the [WebSocket](http://download.java.net/java/jdk9/docs/api/jdk/incubator/http/WebSocket.html) class which has its own [WebSocket.Builder](http://download.java.net/java/jdk9/docs/api/jdk/incubator/http/WebSocket.Builder.html). I’m going to cover this in another post as its clearer than on jshell

## Process API

The [process API](http://download.java.net/java/jdk9/docs/api/java/lang/Process.html) simplifies accessing Process information in Java.

Consider the details of my current Jshell process –

```
<pre class="">jshell> System.out.println(ProcessHandle.current().pid());
8580

jshell> System.out.println(ProcessHandle.current().info());
[user: Optional[NEW-EJ0JTJ5I9B9\javabullets], cmd: C:\Program Files\Java\jdk-9\bin\java.exe, startTime: Optional[2017-10-09T19:41:21.743Z], totalTime: Optional[PT4.625S]]

jshell> System.out.println(ProcessHandle.current().parent());
Optional[6592]
```

We can also access System processes and Id’s –

```
<pre class="">jshell> ProcessHandle.allProcesses().forEach(p -> System.out.println(p.pid()));
8276
9720
8012
480
```

Or Info –

```
<pre class="">jshell> ProcessHandle.allProcesses().forEach(p -> System.out.println(p.info()));

[user: Optional[NEW-EJ0JTJ5I9B9\javabullets], cmd: C:\Program Files (x86)\PFU\ScanSnap\Update\ScanSnapUpdater.exe, startTime: Optional[2017-10-09T18:28:42.812Z], totalTime: Optional[PT0.78125S]]
[user: Optional[NEW-EJ0JTJ5I9B9\javabullets], cmd: C:\Windows\explorer.exe, startTime: Optional[2017-10-09T18:35:08.397Z], totalTime: Optional[PT25.234375S]]
[user: Optional[NEW-EJ0JTJ5I9B9\javabullets], cmd: C:\Windows\System32\cmd.exe, startTime: Optional[2017-10-09T18:36:11.522Z], totalTime: Optional[PT0.078125S]]
```

Now we have access to processes we can kill selective process – lets kill notepad –

```
<pre class="">jshell> Process p = new ProcessBuilder("C:\\Program Files (x86)\\Google\\Chrome\\Application\\chrome.exe").start();
p ==> Process[pid=9644, exitValue="not exited"]

jshell> p.destroy();
```

We also have the option to destroyForcibly if destroy doesnt kill the process.

The above examples show how useful and simple the Process API is for starting, killing and monitoring processes. It free’s us from relying on third party libraries for providing process control