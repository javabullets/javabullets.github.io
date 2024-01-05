---
id: 2499
title: 'Access the EntityManager from Spring Data JPA'
date: '2017-09-03T21:34:19+00:00'
author: 'Martin Farrell'
layout: post
guid: 'https://www.javabullets.com/?p=2499'
permalink: /access-entitymanager-spring-data-jpa/
thrive_post_fonts:
    - '[]'
thrive_share_count:
    - '{"facebook":2,"twitter":0,"plusone":0,"pinterest":0,"linkedin":0,"total":2,"last_fetch":1548958906,"url":"https://www.javabullets.com/access-entitymanager-spring-data-jpa/"}'
image: /wp-content/uploads/2017/05/steve-taylor-110050.jpg
categories:
    - Java8
    - 'Spring Boot'
    - 'Spring Data'
tags:
    - 'custom repository'
    - EntityManager
    - JPA
    - 'Spring Data'
---

Spring Data JPA allows you to rapidly develop your data access layer through the use of Repository interfaces. Occasionally you will need to access the EntityManager from Spring Data JPA. This post shows you how to access the EntityManager.

## EntityManager

The purpose of the EntityManager is to interact with the persistence context. The persistence context will then manage entity instances and their associated lifecycle. This was covered in my blog post on the [JPA Entity Lifecycle](https://www.javabullets.com/jpa-entity-lifecycle/)

Spring Data JPA does an excellent job abstracting you from the EntityManager through its Repository interfaces –

- [Repository](https://docs.spring.io/spring-data/commons/docs/current/api/org/springframework/data/repository/Repository.html?is-external=true)
- [CrudRepository](https://docs.spring.io/spring-data/commons/docs/current/api/org/springframework/data/repository/CrudRepository.html?is-external=true)
- [JPARepository](https://docs.spring.io/spring-data/jpa/docs/1.11.6.RELEASE/api/org/springframework/data/jpa/repository/JpaRepository.html#getOne-ID-)

But occasionally you need to access the EntityManager.

## EntityManager.refresh

An example of this is the refresh method. The refresh method refreshes the state of an instance from the database, and overwrites the copy held by the EntityManager. This ensures the EntityManager manager has the most up to date version of the data

## Spring Data JPA Example

Lets use the JPA object from my normal [test ground](https://github.com/farrelmr/parkrunpbreboot) –

```

@Entity
@Table(name = "PARKRUNCOURSE")
public class ParkrunCourse {
   @Id
   @Column(name = "PRCOURSE_ID")
   @GeneratedValue
   private Long courseId;
   @Column(name = "COURSENAME")
   private String courseName;
   @Column(name = "URL")
   private String url;
   @Column(name = "AVERAGETIME")
   private Long averageTime;
}
```

And its associated repository –

```

public interface ParkrunCourseRepository extends CrudRepository {}
```

This is a standard implementation of a Spring repository, with the CrudRepository taking ParkrunCourse, and its key type Long

Create Custom Interfaces and Implmentation

The first step is to define a new interface with the same signature as the underlying EntityManager method we want to access –

```

public interface ParkrunCourseRepositoryCustom {
   void refresh(ParkrunCourse parkrunCourse);
}
```

The key point is the custom implementation must end with “Custom”, unless overridden in Spring Data configuration.

Next we provide the implementation for this interface, and inject the EntityManager –

```

import javax.persistence.PersistenceContext;
import javax.persistence.EntityManager;
import com.glenware.springboot.form.ParkrunCourse;
import org.springframework.transaction.annotation.Transactional;
public class ParkrunCourseRepositoryImpl implements ParkrunCourseRepositoryCustom {
   @PersistenceContext
   private EntityManager em;
   @Override
   @Transactional
   public void refresh(ParkrunCourse parkrunCourse) {
      em.refresh(parkrunCourse);
   }
}
```

We must end our implementation name with “Impl”

We then change the ParkrunCourseRepository interface to –

```

public interface ParkrunCourseRepository extends CrudRepository, ParkrunCourseRepositoryCustom {
}
```

We can then refresh our JPA object –

```

@Autowired
private ParkrunCourseRepository parkrunCourseRepository;
ParkrunCourse parkrunCourse = parkrunCourseRepository.findOne(1L);
// Do some work & in the mean time the database has been updated by a batch job
// refresh object and now up to date
parkrunCourseRepository.refresh(parkrunCourse);
```

## Conclusions

This approach shows how to access the EntityManager using Spring Data JPA. The advantage of this approach is you can access the EntityManager for a specific JPA implementation. The disadvantage of this approach is that you would need to repeat this task for each JPA implementation. The next post looks at a more generic approach to the custom repository implementation, allowing other JPA objects to benefit.