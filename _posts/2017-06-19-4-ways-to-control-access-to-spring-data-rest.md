---
id: 2271
title: '4 Ways To Control Access to Spring Data REST'
date: '2017-06-19T18:51:07+00:00'
author: 'Martin Farrell'
layout: post
guid: 'https://www.javabullets.com/?p=2271'
permalink: /4-ways-to-control-access-to-spring-data-rest/
thrive_post_fonts:
    - '[]'
image: /wp-content/uploads/2017/06/martins-zemlickis-57243.jpg
categories:
    - 'Spring Boot'
    - 'Spring Data'
tags:
    - RepositoryDetectionStrategies
    - 'spring data REST'
    - 'spring framework'
---

# 4 Ways To Control Access to Spring Data REST

This post looks at 4 ways to control access to Spring Data REST using [RepositoryDetectionStrategies’s](http://docs.spring.io/spring-data/rest/docs/current/api/org/springframework/data/rest/core/mapping/RepositoryDetectionStrategy.RepositoryDetectionStrategies.html)

This post forms part of a series looking at Spring Data REST –

- [Introduction to Spring Data REST](https://www.javabullets.com/spring-data-rest-introduction/)
- [Spring Security and Spring Data REST](https://www.javabullets.com/spring-security-spring-data-rest/)
- [Securing Spring Data REST with PreAuthorize](https://www.javabullets.com/securing-spring-data-rest-preauthorize/)

## RepositoryDetectionStrategies

You can control access to Spring Data REST using the 4 RepositoryDetectionStrategies –

<div class="row gr gr1"><div class="colm twc"><div class="gri left">![]()</div><div class="grt left">#### Field

ALL

</div><div class="clear"></div></div><div class="colm twc lst"><div class="gri left">![]()</div><div class="grt left">#### Description

Exposes all Spring Data Repositories regardless of annotations or access control

</div><div class="clear"></div></div></div><div class="row gr gr1"><div class="colm twc"><div class="gri left">![]()</div><div class="grt left">
ANNOTATED

</div><div class="clear"></div></div><div class="colm twc lst"><div class="gri left">![]()</div><div class="grt left">
Only Repositories exposed with RepositoryRestResource or RestResource, and the exported flag is not set to false.

</div><div class="clear"></div></div></div><div class="row gr gr1"><div class="colm twc"><div class="gri left">![]()</div><div class="grt left">
DEFAULT

</div><div class="clear"></div></div><div class="colm twc lst"><div class="gri left">![]()</div><div class="grt left">
public Spring Data Repositories or ones annotated with RepositoryRestResource

</div><div class="clear"></div></div></div><div class="row gr gr1"><div class="colm twc"><div class="gri left">![]()</div><div class="grt left">
VISIBILITY

</div><div class="clear"></div></div><div class="colm twc lst"><div class="gri left">![]()</div><div class="grt left">
Only public Spring Data Repositories

</div><div class="clear"></div></div></div>## Source Code

- <https://github.com/farrelmr/introtospringdatarest/tree/5.0.0>
- <https://github.com/farrelmr/introtospringdatarest/releases/tag/5.0.0>

You can run the examples by –

```
mvnw spring-boot:run
```

## Examples

Ive created a Spring Rest Configuration class for this example –

```

@Component
public class SpringRestConfiguration extends RepositoryRestConfigurerAdapter {
@Override
public void configureRepositoryRestConfiguration(RepositoryRestConfiguration config) {
config.setRepositoryDetectionStrategy(RepositoryDetectionStrategy.RepositoryDetectionStrategies.DEFAULT);
}
}
```

All of the examples will be changing the enumerated value of RepositoryDetectionStrategy.RepositoryDetectionStrategies, and testing what REST end points are exposed.

I have also annoted one of our Spring Data Repositories –

```

@RepositoryRestResource
@PreAuthorize("hasRole('ROLE_USER')")
public interface ParkrunCourseRepository extends CrudRepository<ParkrunCourse, Long> {
@Override
@PreAuthorize("hasRole('ROLE_ADMIN')")
ParkrunCourse save(ParkrunCourse parkrunCourse);
}
```

### RepositoryDetectionStrategies.DEFAULT

A default setting RepositoryDetectionStrategy detect –

- public interfaces
- annotated with RepositoryRestResource or RestResource

So with our example we would expect to see both interfaces exposed as REST end points

We can confirm this by calling http://localhost:8080/rest/profile –

```
curl -u user:user -X GET http://localhost:8080/rest/profile
{
 "_links" : {
 "self" : {
 "href" : "http://localhost:8080/rest/profile"
 },
 "secrets" : {
 "href" : "http://localhost:8080/rest/profile/secrets"
 },
 "parkrunCourses" : {
 "href" : "http://localhost:8080/rest/profile/parkrunCourses"
 }
 }
}

```

### RepositoryDetectionStrategies.ANNOTATED

Annotated will only detect interfaces annotated with RepositoryRestResource or RestResource, and an exported flag that is true(default). In our test case we would only expect to see the end point for ParkrunCourseRepository to be exposed as it is annotated with @RepositoryRestResource

We can test and confirm this –

```
curl -u user:user -X GET http://localhost:8080/rest/profile
{
 "_links" : {
 "self" : {
 "href" : "http://localhost:8080/rest/profile"
 },
 "parkrunCourses" : {
 "href" : "http://localhost:8080/rest/profile/parkrunCourses"
 }
 }
}
```

### RepositoryDetectionStrategies.VISIBILITY

VISIBILITY will only detect REST and expose reportories based if they are publicly exposed.

Lets make a change to SecretRepository to leave the interface with default visibility –

```
interface SecretRepository extends CrudRepository<Secret, Long> {
}
```

We would not expect to see this end point, and would only expect to see the ParkrunCourseRepository end point. This is confirmed when we call the rest/profile –

```
curl -u user:user -X GET http://localhost:8080/rest/profile
{
 "_links" : {
 "self" : {
 "href" : "http://localhost:8080/rest/profile"
 },
 "parkrunCourses" : {
 "href" : "http://localhost:8080/rest/profile/parkrunCourses"
 }
 }
}
```

### RepositoryDetectionStrategies.ALL

Finally if we use RepositoriesDetectionStrategies.ALL with SecretRepository at default access. Then we would expect to see both the parkrunCourses and Secret’s end point. All will also expose end points that have an exported false attribute.

Restart the server and test –

```
curl -u user:user -X GET http://localhost:8080/rest/profile
{
 "_links" : {
 "self" : {
 "href" : "http://localhost:8080/rest/profile"
 },
 "parkrunCourses" : {
 "href" : "http://localhost:8080/rest/profile/parkrunCourses"
 },
 "secrets" : {
 "href" : "http://localhost:8080/rest/profile/secrets"
 }
 }
}
```

## Conclusions

This post looked at 4 ways to control access to Spring Data REST using RepositoryDetectionStrategies allow or restrict access to the underlying Spring Data JPA repositories. The options are – ALL, ANNOTATED, VISIBILITY or DEFAULT