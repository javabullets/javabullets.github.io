---
id: 584
title: 'InvocationTargetException in transform method on plugin &#8216;class WebappClassLoader&#8217;'
date: '2016-05-26T10:34:53+00:00'
author: 'Martin Farrell'
layout: post
guid: 'https://glenware.wordpress.com/?p=584'
permalink: /invocationtargetexception-in-transform-method-on-plugin-class-webappclassloader/
thrive_post_fonts:
    - '[]'
categories:
    - BuildTools
    - Uncategorized
tags:
    - hotswapagent
    - tomcat
---

I really like [hotswapagent ](http://hotswapagent)but had the above error on start up. I had to do some digging – but concluded there was nothing to worry about, and the code is simply checking the old location and the new location for the WebappClassLoader

I had seen a post with a similar error on Tomcat8, but Im running Apache Tomcat/[7.0.69.](http://7.0.69.) The exception is listed below – but here is my analysis.

The method org.hotswap.agent.plugin.tomcat.WebappLoaderTransformer.patchWebappClassLoader first tries to find getResource in tomcats WebappClassLoader, but doesnt find it. It then tries WebappClassLoaderBase and starts successfully?

The Hotswapagent states that this method is in WebappClassLoaderBase for tomcat8, and WebappClassLoader for 7 but I checked the tomcat code –

[http://svn.apache.org/repos/asf/tomcat/tc7.0.x/tags/TOMCAT\_7\_0\_69/java/org/apache/catalina/loader/](http://svn.apache.org/repos/asf/tomcat/tc7.0.x/tags/TOMCAT_7_0_69/java/org/apache/catalina/loader/)

And it confirms that it is WebappClassLoaderBase for 7 too

Everything looks ok to me and the plugin has started – but thought this post might be of interest

Exception –

HOTSWAP AGENT: 9:54:35.317 ERROR (org.hotswap.agent.annotation.handler.OnClassLoadedHandler) – InvocationTargetException in transform method on plugin ‘class org.hotswap.agent.plugin.tomcat.TomcatPlugin’ class ‘org/apache/catalina/loader/WebappClassLoader’.  
java.lang.reflect.InvocationTargetException  
at sun.reflect.NativeMethodAccessorImpl.invoke0(Native Method)  
at sun.reflect.NativeMethodAccessorImpl.invoke(NativeMethodAccessorImpl.java:57)  
at sun.reflect.DelegatingMethodAccessorImpl.invoke(DelegatingMethodAccessorImpl.java:43)  
at java.lang.reflect.Method.invoke(Method.java:606)  
at org.hotswap.agent.annotation.handler.OnClassLoadedHandler.transform(OnClassLoadedHandler.java:157)  
at org.hotswap.agent.annotation.handler.OnClassLoadedHandler$1.transform(OnClassLoadedHandler.java:76)  
at org.hotswap.agent.util.HotswapTransformer.transform(HotswapTransformer.java:129)  
at sun.instrument.TransformerManager.transform(TransformerManager.java:188)  
at sun.instrument.InstrumentationImpl.transform(InstrumentationImpl.java:424)  
at java.lang.ClassLoader.defineClass1(Native Method)  
at java.lang.ClassLoader.defineClass(ClassLoader.java:800)  
at java.security.SecureClassLoader.defineClass(SecureClassLoader.java:142)  
at java.net.URLClassLoader.defineClass(URLClassLoader.java:449)  
at java.net.URLClassLoader.access$100(URLClassLoader.java:71)  
at java.net.URLClassLoader$1.run(URLClassLoader.java:361)  
at java.net.URLClassLoader$1.run(URLClassLoader.java:355)  
at java.security.AccessController.doPrivileged(Native Method)  
at java.net.URLClassLoader.findClass(URLClassLoader.java:354)  
at java.lang.ClassLoader.loadClass(ClassLoader.java:425)  
at java.lang.ClassLoader.loadClass(ClassLoader.java:358)  
at java.lang.Class.forName0(Native Method)  
at java.lang.Class.forName(Class.java:191)  
at org.apache.catalina.loader.WebappLoader.createClassLoader(WebappLoader.java:721)  
at org.apache.catalina.loader.WebappLoader.startInternal(WebappLoader.java:582)  
at org.apache.catalina.util.LifecycleBase.start(LifecycleBase.java:147)  
at org.apache.catalina.core.StandardContext.startInternal(StandardContext.java:5449)  
at org.apache.catalina.util.LifecycleBase.start(LifecycleBase.java:147)  
at org.apache.catalina.core.ContainerBase$StartChild.call(ContainerBase.java:1572)  
at org.apache.catalina.core.ContainerBase$StartChild.call(ContainerBase.java:1562)  
at java.util.concurrent.FutureTask.run(FutureTask.java:262)  
at java.util.concurrent.ThreadPoolExecutor.runWorker(ThreadPoolExecutor.java:1145)  
at java.util.concurrent.ThreadPoolExecutor$Worker.run(ThreadPoolExecutor.java:615)  
at java.lang.Thread.run(Thread.java:745)  
Caused by: org.hotswap.agent.javassist.NotFoundException: getResource(..) is not found in org.apache.catalina.loader.WebappClassLoader  
at org.hotswap.agent.javassist.CtClassType.getDeclaredMethod(CtClassType.java:1186)  
at org.hotswap.agent.plugin.tomcat.WebappLoaderTransformer.patchWebappClassLoader(WebappLoaderTransformer.java:188)  
… 33 more

HOTSWAP AGENT: 9:54:35.411 INFO (org.hotswap.agent.config.PluginRegistry) – Plugin ‘org.hotswap.agent.plugin.tomcat.TomcatPlugin’ initialized in ClassLoader ‘WebappClassLoader  
context:  
delegate: false  
repositories:  
———-&gt; Parent Classloader:  
java.net.URLClassLoader@3936d1b4  
‘.  
HOTSWAP AGENT: 9:54:35.411 INFO (org.hotswap.agent.plugin.tomcat.TomcatPlugin) – Tomcat plugin initialized – Tomcat version ‘7.0.69.0’