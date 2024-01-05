---
id: 190
title: 'parkrunPB – Spring 3 and Spring MVC – Part 2'
date: '2014-08-15T13:55:02+00:00'
author: 'Martin Farrell'
layout: post
guid: 'http://glenware.wordpress.com/?p=190'
permalink: /parkrunpb-spring-3-and-spring-mvc-part-2/
geo_public:
    - '0'
thrive_post_fonts:
    - '[]'
categories:
    - Java
tags:
    - parkrunPB
    - Spring
    - 'spring MVC'
    - Spring3
    - Spring4
---

While the AboutController is an exciting introduction to Spring MVC it doesn’t really do much except return the About screen. The real power of Spring MVC is GET’ing and POST’ing forms.

The Admin Screen demonstrates this functionality. First through a form –

```

<form:form method="post"
           action="addAdmin.html"
           commandName="prCourse"
	     role="form"
           class="form-horizontal"> 
	<form:errors path="*" cssClass="errorblock" element="div" /> 
	<div class="form-group">
		<label class="col-sm-2 control-label" for="prName">Course
			Name</label>
		<div class="col-sm-4">
			<form:input type="text" path="prName" class="form-control" />
		</div>
	</div> 
	<div class="form-group">
		<label class="col-sm-2 control-label" for="url">Link</label>
		<div class="col-sm-4">
			<form:input type="text" path="url" class="form-control" />
		</div>
	</div> 
	<div class="form-group">
		<label class="col-sm-2 control-label" for="averageTime">Average
			Time(s)</label>
		<div class="col-sm-4">
			<form:input type="text" path="averageTime" class="form-control" />
		</div>
	</div> 
	<div class="form-group">
		<label class="col-sm-2 control-label" for="regionId">Course</label>
		<div class="col-sm-4">
			<form:select path="regionId" class="form-control">
				<form:options items="${prRegionMap}" />
			</form:select>
		</div>
	</div> 
	<div class="form-group">
		<div class="col-sm-2"></div>
		<div>
			<input type="submit"
				value="<spring:message code="label.addparkrun"/>"
				class="btn btn-primary btn-lg" />
		</div>
	</div> 
</form:form>
```

The key points on this JSP are –

&gt; Use of form tag to bind form variables to the PRCourse object (prCourse)  
&gt; User of form tag to take input  
&gt; tag for error reporting – note that we can make this more specific to the level of the specific error

Lets consider the backing AdminController class –

\[sourcecode lang=”java”\] @Controller  
@SessionAttributes("prRegionMap")  
public class AdminController {  @Autowired  
 private ParkrunService parkrunService;

 @RequestMapping("/admin")  
 public String admin( Map&lt;String, Object&gt; map ) {  
 map.put("prCourse", new PRCourse());  
 List&lt;PRCourse&gt; prCourseList = parkrunService.listPRCourse();  
 map.put("prCourseList", prCourseList);  
 return "admin";  
 }

 @ModelAttribute("prRegionMap")  
 public Map &lt;Integer, String&gt; createPrRegionMap() {  
 List&lt;PRRegion&gt; prRegionList = parkrunService.listPRRegion();  
 Map &lt;Integer, String&gt; prRegionMap = new LinkedHashMap&lt;Integer, String&gt; ();  
 for ( PRRegion prRegion : prRegionList ) {  
 prRegionMap.put(prRegion.getId(), prRegion.getRegionName());  
 }  
 return prRegionMap;  
 }

 @RequestMapping(value = "/addAdmin", method = RequestMethod.POST)  
 public String addPRCourse(@Valid @ModelAttribute("prCourse") PRCourse prCourse,  
 BindingResult result,  
 Map&lt;String, Object&gt; map ) {  
 if ( result.hasErrors() ) {  
 List&lt;PRCourse&gt; prCourseList = parkrunService.listPRCourse();  
 map.put("prCourseList", prCourseList);  
 } else {  
 prCourse.setPrregion( parkrunService.getPRRegion( prCourse.getRegionId() ) );  
 parkrunService.addPRCourse(prCourse);  
 }  
 return "redirect:/admin";  
 }

 @RequestMapping("/deleteAdmin/{prCourseId}")  
 public String removePRCourse(@PathVariable("prCourseId") Integer prCourseId) {  
 parkrunService.removePRCourse(prCourseId);  
 return "redirect:/admin";  
 }

}  
\[/sourcecode\]

The key points here are –

&gt; @Controller – Marks a class as a controller. This means any Java class can be effectively converted to a controller  
&gt; @SessionAttributes(“prRegionMap”) – Adds a ModelAttribute to the session. This links to createPrRegionMap() method  
&gt; @RequestMapping(“/admin”) – This method deals with the setting up the form when it is first called. The key functionality is loading the “prCourseList” list of courses, and returning to the “admin” screen  
&gt; @RequestMapping(value = “/addAdmin”, method = RequestMethod.POST) – This is the key action on the form.  
 &gt; @Valid @ModelAttribute(“prCourse”) PRCourse prCourse – marks the PRCourse object that is returned for JSR-303 validation  
 &gt; Test on errors – if no errors we submit to the parkrunService.addPRCourse method  
&gt; @RequestMapping(“/deleteAdmin/{prCourseId}”) – Deletes a course as required. This is called using the GET parameter, although could use POST  
 &gt;

| [delete](deleteAdmin/${prCourse.id}) JSR303 Validation

JSR303 is a way of reducing boilerplate validation code on tests like @NotNull, @NotEmpty, @Range

For example in PRCourse –

\[sourcecode lang=”java”\] @NotEmpty  
 private String prName  @Range(min = 1)  
 private int averageTime;  
\[/sourcecode\]

These are then linked to the corresponding

NotEmpty.prCourse.prName = You must provide a course name.  
NotEmpty.prCourse.url = You must provide a course link.  
Range.prCourse.averageTime = Range must be greater than 1

Sometimes however you need validation to be a bit more involved, and Spring MVC supports this too –

\[sourcecode lang=”java”\] public class PRRegionValidator implements Validator {  private static final Logger logger = LoggerFactory  
 .getLogger(PRRegionValidator.class);

 @Override  
 public boolean supports(Class&lt;?&gt; clazz) {  
 return clazz.isAssignableFrom(PRPredict.class);  
 }

 @Override  
 public void validate(Object obj, Errors errors) {

 PRPredict prPredict = (PRPredict) obj;

 ValidationUtils.rejectIfEmptyOrWhitespace(errors, "courseId",  
 "NotEmpty.prPredict.courseId", "You must specify a course.");

 ValidationUtils.rejectIfEmptyOrWhitespace(errors, "mm",  
 "NotNull.prPredict.mm",  
 "You must set the number of minutes between 0s and 59s.");

 Integer mmInt = null;  
 try {  
 mmInt = Integer.parseInt(prPredict.getMm());  
 } catch (NumberFormatException nfe) {  
 errors.rejectValue("mm", "NotNull.prPredict.mm",  
 "You must set the number of minutes between 0s and 59s.");  
 return;  
 }

 if (mmInt == null) {  
 logger.info("mmInt is null " + mmInt);  
 }

 if (mmInt &lt; 0 || mmInt &gt; 60) {  
 errors.rejectValue("mm", "NotNull.prPredict.mm",  
 "You must set the number of minutes between 0s and 59s.");  
 }

 logger.info("" + mmInt);  
 prPredict.setMmInt(mmInt);

 ValidationUtils.rejectIfEmptyOrWhitespace(errors, "ss",  
 "NotNull.prPredict.ss",  
 "You must set the number of seconds between 0s and 59s.");

 Integer ssInt = null;  
 try {  
 ssInt = Integer.parseInt(prPredict.getSs());  
 } catch (NumberFormatException nfe) {  
 errors.rejectValue("ss", "NotNull.prPredict.ss",  
 "You must set the number of seconds between 0s and 59s.");  
 return;  
 }

 if (ssInt &lt; 0 || ssInt &gt; 60) {  
 errors.rejectValue("ss", "NotNull.prPredict.ss",  
 "You must set the number of seconds between 0s and 59s.");  
 }  
 logger.info("" + ssInt);  
 prPredict.setSsInt(ssInt);

 }

 }  
\[/sourcecode\]

Which is accessed through –

\[sourcecode lang=”java”\] @InitBinder("prPredict")  
protected void initBinder(WebDataBinder binder) {  
 binder.setValidator(new PRRegionValidator());  
}  
\[/sourcecode\] 