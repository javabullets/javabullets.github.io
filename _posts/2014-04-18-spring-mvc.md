---
id: 105
title: 'Spring MVC'
date: '2014-04-18T21:50:15+00:00'
author: 'Martin Farrell'
layout: post
guid: 'http://glenware.wordpress.com/?p=105'
permalink: /spring-mvc/
geo_public:
    - '0'
thrive_post_fonts:
    - '[]'
categories:
    - Java
tags:
    - 'Dependency Injection'
    - Spring
    - 'Spring Configuration'
---

Spring MVC is one of the most commonly used Spring Modules. At its core is the DispatcherServlet.

The DispatcherServlet offers a range of features including –

- Separation of roles between controller, validator, command object, form object, model object, DispatcherServlet, handler mapping, view resolver
- Use of POJO’s
- Customisable binding and Validation
- Model Map allows different front end solutions to be used

The DispatcherServlet binds to WebApplicationContext, and special beans –

- DispatcherServlet
- WebApplicationContext – Bound to request as attribute for controller. Default key – DispatcherServlet.WEB\_APPLICATION\_CONTEXT\_ATTRIBUTE
- Controllers
- Handler Mappings – Examples include URL matching
- Locale Resolvers – Map to locale
- Theme Resolver – Resolve themes for personalized layouts
- View Resolvers – Map view names to views
- Multipart Resolver – File Uploads
- Handler Exception Resolvers – Handle exceptions

**DispatcherServlet Configuration**

\[sourcecode lang=”xml”\]  &lt;servlet&gt;  
 &lt;servlet-name&gt;spring&lt;/servlet-name&gt;  
 &lt;servlet-class&gt;org.springframework.web.servlet.DispatcherServlet&lt;/servlet-class&gt;  
 &lt;load-on-startup&gt;1&lt;/load-on-startup&gt;  
 &lt;/servlet&gt;  
   
 &lt;servlet-mapping&gt;  
 &lt;servlet-name&gt;spring&lt;/servlet-name&gt;  
 &lt;url-pattern&gt;/&lt;/url-pattern&gt;  
 &lt;/servlet-mapping&gt;  
\[/sourcecode\]

By default this will look for a configuration under /WEB-INF/spring-servlet.xml

You can also use ContextLoaderListener to load multiple configuration files

\[sourcecode lang=”xml”\] &lt;listener&gt;  
 &lt;listener-class&gt;org.springframework.web.context.ContextLoaderListener&lt;/listener-class&gt;  
&lt;/listener&gt;  
\[/sourcecode\] Allows you to specify multiple xml files –

\[sourcecode lang=”xml”\] &lt;context-param&gt;  
 &lt;param-name&gt;contextConfigLocation&lt;/param-name&gt;  
 &lt;param-value&gt;  
 /WEB-INF/applicationContext\*.xml  
 classpath:someother-context.xml  
 &lt;/param-value&gt;  
&lt;/context-param&gt;  
\[/sourcecode\] **Configuration**

Like all Spring configurations you have the option of either –

- XML Config
- Annotation Driven Context

Im not going to cover the XML configuration beyond configuration of annotation driven context, as its covered very well in the Spring documentation.

**Preconfiguration**

The servlet configuration above will serve all requests. The starting point is to separate requests for resources from spring MVC configurations –

\[sourcecode lang=”xml”\] &lt;mvc:resources mapping="/assets/\*\*" location="classpath:/META-INF/resources/webjars/"/&gt;  
 &lt;mvc:resources mapping="/css/\*\*" location="/css/"/&gt;  
 &lt;mvc:resources mapping="/img/\*\*" location="/img/"/&gt;  
 &lt;mvc:resources mapping="/js/\*\*" location="/js/"/&gt;  
\[/sourcecode\] We’re using DefaultAnnotationHandlerMapping. This needs to be configured by –

\[sourcecode lang=”xml”\] &lt;context:component-scan base-package="com.glenware.parkrunpb" /&gt;  
&lt;mvc:annotation-driven/&gt;  
\[/sourcecode\] Other options are BeanNameUrlHandlerMapping, ControllerBeanNameHandlerMapping, ControllerClassNameHandlerMapping and SimpleUrlHandlerMapping

Finally we configure the ViewResolver –

\[sourcecode lang=”xml”\] &lt;bean id="jspViewResolver"  
 class="org.springframework.web.servlet.view.InternalResourceViewResolver"&gt;  
 &lt;property name="viewClass" value="org.springframework.web.servlet.view.JstlView" /&gt;  
 &lt;property name="prefix" value="/WEB-INF/jsp/" /&gt;  
 &lt;property name="suffix" value=".jsp" /&gt;  
 &lt;/bean&gt;  
\[/sourcecode\] This will serve JSTL pages of the pattern –

**prefix/&lt;controllerRequest&gt;.suffix –&gt; /WEB-INF/jsp/&lt;controllerRequest&gt;.jsp**

Other options for ViewResolvers include BeanNameViewResolver, TilesViewResolver and ContentNegotiatingViewResolver

Finally we should configure the messages –

\[sourcecode lang=”xml”\]  &lt;bean id="messageSource"  
 class="org.springframework.context.support.ReloadableResourceBundleMessageSource"&gt;  
 &lt;property name="basename" value="classpath:messages" /&gt;  
 &lt;property name="defaultEncoding" value="UTF-8" /&gt;  
 &lt;/bean&gt;

\[/sourcecode\] **Configuration of Controllers**

The most basic controller is

\[sourcecode lang=”java”\] package com.glenware.parkrunpb.controller; import org.springframework.stereotype.Controller;  
import org.springframework.web.bind.annotation.RequestMapping;

@Controller  
public class AboutController {

 @RequestMapping("/about")  
 public String about() {  
 return "about";s  
 }

}  
\[/sourcecode\]

Lets break this down –

1\. @Controller – This annotation instructs the Spring container that the object is a Controller

2\. @RequestMapping – Signifies the use of DefaultAnnotationHandlerMapping. In this instance it will serve requests for /about

The @RequestMapping can also be used at class level – eg

\[sourcecode lang=”java”\] @Controller  
@RequestMapping("api")  
public class JSONController

\[/sourcecode\] 3\. Finally we see the about method which simply forwards to “about”. This is combined with the ViewResolver –

**prefix/&lt;controllerRequest&gt;.suffix –&gt; /WEB-INF/jsp/about.jsp**

**Forms**

Any real app would involve some transfer of data from the server.

An example form would be of the structure –

\[sourcecode lang=”html”\] &lt;form:form method="post" action="addAdmin.html" commandName="prCourse" role="form" class="form-horizontal"&gt;

&lt;form:errors path="\*" cssClass="errorblock" element="div" /&gt;

&lt;div class="form-group"&gt;  
&lt;label class="col-sm-2 control-label" for="prName"&gt;Course Name&lt;/label&gt;  
&lt;div class="col-sm-4"&gt;  
&lt;form:input type="text" path="prName" class="form-control" /&gt;  
&lt;/div&gt;  
&lt;/div&gt;

&lt;div class="form-group"&gt;  
&lt;label class="col-sm-2 control-label" for="url"&gt;Link&lt;/label&gt;  
&lt;div class="col-sm-4"&gt;  
&lt;form:input type="text" path="url" class="form-control" /&gt;  
&lt;/div&gt;  
&lt;/div&gt;

&lt;div class="form-group"&gt;  
&lt;label class="col-sm-2 control-label" for="averageTime"&gt;Average Time(s)&lt;/label&gt;  
&lt;div class="col-sm-4"&gt;  
&lt;form:input type="text" path="averageTime" class="form-control" /&gt;  
&lt;/div&gt;  
&lt;/div&gt;

&lt;div class="form-group"&gt;  
&lt;label class="col-sm-2 control-label" for="regionId"&gt;Course&lt;/label&gt;  
&lt;div class="col-sm-4"&gt;  
&lt;form:select path="regionId" class="form-control"&gt;  
&lt;form:options items="${prRegionMap}" /&gt;  
&lt;/form:select&gt;  
&lt;/div&gt;  
&lt;/div&gt;

&lt;div class="form-group"&gt;  
&lt;div class="col-sm-2"&gt;&lt;/div&gt;  
&lt;div&gt;  
&lt;input type="submit"  
value="&lt;spring:message code="label.addparkrun"/&gt;"  
class="btn btn-primary btn-lg" /&gt;  
&lt;/div&gt;  
&lt;/div&gt;

&lt;/form:form&gt;

\[/sourcecode\] The key points are –

- commandName=”prCourse” – This is a reference to the form object being passed
- action=”addAdmin.html” – This is the Controller action that will process the object
- &lt;form:errors path=”\*” cssClass=”errorblock” element=”div” /&gt; – Prints out errors

The mapping and method for processing this object is –

\[sourcecode lang=”java”\] @RequestMapping(value = "/addAdmin", method = RequestMethod.POST)  
public String addPRCourse( @Valid @ModelAttribute("prCourse") PRCourse prCourse,  
BindingResult result,  
Map&lt;String, Object&gt; map ) {

if ( result.hasErrors() ) {  
List&lt;PRCourse&gt; prCourseList = parkrunService.listPRCourse();  
map.put("prCourseList", prCourseList);

List&lt;PRRegion&gt; prRegionList = parkrunService.listPRRegion();  
Map &lt;Integer, String&gt; prRegionMap = new LinkedHashMap&lt;Integer, String&gt; ();  
for ( PRRegion prRegion : prRegionList ) {  
prRegionMap.put(prRegion.getId(), prRegion.getRegionName());  
}  
map.put("prRegionMap", prRegionMap);

return "admin";  
} else {  
prCourse.setPrregion( parkrunService.getPRRegion( prCourse.getRegionId() ) );  
parkrunService.addPRCourse(prCourse);  
}  
return "redirect:/admin";  
}  
\[/sourcecode\]

The key points are –

- @Valid @ModelAttribute(“prCourse”) PRCourse prCourse
- @ModelAttribute(“prCourse”) – Maps to commandName=”prCourse”
- @Valid – References JSR303 validation in PRCourse Object
- Check for errors – result.hasErrors()
- Map&lt;String, Object&gt; map – Key, Values passed to View layer
- return “redirect:/admin” – Is a send redirect, other options are forward or direct straight to the jsp

**Passing Single Parameters**

**@RequestParam**

A RequestParam represents a link would be of the form –

http://localhost:8080/mysite/requestParamExample?id=123

\[sourcecode lang=”java”\]  @RequestMapping(value = "/requestParamExample", method = RequestMethod.GET)  
 public String requestParamExample(@RequestParam("id") String id, Model model) {  
 …  
 return "requestParamExample";  
 }

\[/sourcecode\] **@PathVariable**

A RequestParam represents a link would be of the form –

http://localhost:8080/mysite/pathVariableExample/id/123

\[sourcecode lang=”java”\] @RequestMapping(value = "/pathVariableExample", method = RequestMethod.GET)  
public String pathVariableExample(@PathVariable("id") Integer id) {  
return "pathVariableExample";  
}  
\[/sourcecode\] **File Upload**

File Upload is covered here –

http://docs.spring.io/spring/docs/3.0.0.M3/reference/html/ch16s08.html

**JSON**

You can also use Spring MVC to serve JSON, when combined with the Jackson libraries and the @ResponseBody annotation –

\[sourcecode lang=”java”\] @Controller  
@RequestMapping("api")  
public class JSONController {  
…  
@RequestMapping("/listcourses")  
@ResponseBody  
public List&amp;lt;PRCourse&amp;gt; admin( Map&amp;lt;String, Object&amp;gt; map ) {  
map.put("prCourse", new PRCourse());  
List&amp;lt;PRCourse&amp;gt; prCourseList = parkrunService.listPRCourse();  
return prCourseList;  
}  
…  
}  
\[/sourcecode\] **References**

http://docs.spring.io/spring/docs/3.0.0.M3/reference/html/pt03.html