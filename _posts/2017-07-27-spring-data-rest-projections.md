---
id: 2285
title: 'Spring Data REST and Projections'
date: '2017-07-27T20:07:16+00:00'
author: 'Martin Farrell'
layout: post
guid: 'https://www.javabullets.com/?p=2285'
permalink: /spring-data-rest-projections/
thrive_post_fonts:
    - '[]'
image: /wp-content/uploads/2017/07/sai-kiran-anagani-61187.jpg
categories:
    - JPA
    - REST
    - 'Spring Data'
    - 'Spring Data REST'
tags:
    - projections
    - 'Spring Boot'
    - 'Spring Data'
    - 'spring data REST'
---

Spring Data REST and Projection’s is the final post in my series on using [Spring Data REST](http://projects.spring.io/spring-data-rest/). Projections allow you to control exposure to your domain objects in a similar way to Domain Transfer Objects(DTO’s).

# Spring Data REST and Projection’s

This post forms part of a series looking at Spring Data REST –

- [Introduction to Spring Data REST](https://www.javabullets.com/spring-data-rest-introduction/)
- [Spring Security and Spring Data REST](https://www.javabullets.com/spring-security-spring-data-rest/)
- [Securing Spring Data REST with PreAuthorize](https://www.javabullets.com/securing-spring-data-rest-preauthorize/)
- [4 Ways To Control Access to Spring Data REST](https://www.javabullets.com/4-ways-to-control-access-to-spring-data-rest/)

## What are Projections?

It is a common practice to use Domain Transfer Object’s in REST API design as a method of separating the API from its underlying model. This is particularly relevent to Spring Data JPA REST where you may want to restrict what is visible to clients.

Spring Data JPA REST allows you to achieve similar through the use of Projections.

## Example

If we stick with the example used in the previous posts we define our JPA object as –

```

@Entity
public class ParkrunCourse {
@Id
@GeneratedValue(strategy=GenerationType.IDENTITY)
private long id;
private String courseName;
private String url;
private Long averageTime;
}
```

We then have our Spring Data JPA repository –

```

@RepositoryRestResource
public interface ParkrunCourseRepository extends CrudRepository<ParkrunCourse, Long> {
}
```

Now lets say we want to restrict our ParkrunCourse JPA object, and not return the id or url. We then define our projection as –

```

@Projection(name = "parkrunCourseExcerpt", types = ParkrunCourse.class)
public interface ParkrunCourseExcerpt {
String getCourseName();
Long getAverageTime();
}
```

We then need to re-define our Spring Data REST repository using the excerptProjection attribute –

```

@RepositoryRestResource(excerptProjection = ParkrunCourseExcerpt.class)
public interface ParkrunCourseRepository extends CrudRepository<ParkrunCourse, Long> {
}
```

The projection is then called as –

http://localhost:8080/rest/parkrunCourses/1?projection=parkrunCourseExcerpt

## Discussion

This series of posts have demonstrated how Spring Data REST can be used to turn Spring Data repositories into REST API’s. We have also shown how we can control access to these REST API’s using Spring Security or control visibility using RepositoryDetectionStrategies. This post has shown how we can use Projections in a similar way you would use Data Transfer Objects to control what clients see.

Spring Data REST offers a quick way to expose your database as a REST API without lots of boilerplate code. The downside is you need to spend time ensuring the database you have the right level of access exposed, and what information you are intending to expose.

I see two main use cases for this –

- Exposing an internal database in a decoupled form – You have a number of client applications wanting access to your database and want to ensure access is more loosely coupled than JDBC.
- Public Access Database – You have a public access database that you wish to expose on the internet. The lack of boilerplate and speed with which you can build your API makes Spring Data JPA a good fit

# Conclusions

This post considered how you can use projections to control the view of your Spring Data repositories. We also presented some conclusions on the best use cases for Spring Data REST.