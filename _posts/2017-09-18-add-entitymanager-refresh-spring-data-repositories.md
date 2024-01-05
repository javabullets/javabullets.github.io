---
id: 2537
title: 'Add EntityManager.refresh to all Spring Data Repositories'
date: '2017-09-18T13:06:28+00:00'
author: 'Martin Farrell'
layout: post
guid: 'https://www.javabullets.com/?p=2537'
permalink: /add-entitymanager-refresh-spring-data-repositories/
thrive_post_fonts:
    - '[]'
image: /wp-content/uploads/2017/09/dario-gartmann-111605.jpg
categories:
    - Java8
    - Spring
    - 'Spring Data'
tags:
    - 'custom repository'
    - java
    - Spring
    - 'Spring Data'
---

In my previous post [Access the EntityManager from Spring Data JPA](https://www.javabullets.com/access-entitymanager-spring-data-jpa/) I showed how to extend a single Spring Data JPA repository to access the [EntityManager.refresh](https://docs.oracle.com/javaee/6/api/javax/persistence/EntityManager.html#refresh(java.lang.Object)) method. This post demonstrates how to Add EntityManager.refresh to all Spring Data Repositories.

## Source Code

The first step is to define your interface –

```
package com.glenware.springboot.repository;

import org.springframework.data.repository.NoRepositoryBean;
import org.springframework.data.repository.Repository;
import org.springframework.data.repository.CrudRepository;

import java.io.Serializable;
import java.util.Optional;

@NoRepositoryBean
public interface CustomRepository<t extends="" id="" serializable="">
extends CrudRepository<t id=""> {
    void refresh(T t);
}</t></t>
```

The key points are –

- @NoRepositoryBean – This prevents an instance of a Repository being created
- Extending the CrudRepository – You need to decided which Repository to extend. Im using CrudRepository as this was used in the previous post
- void refresh method signature is the same as the EntityManager.refresh method

### Implementation

The next step is to implement this interface in a custom repository –

```
package com.glenware.springboot.repository;

import org.springframework.data.jpa.repository.support.SimpleJpaRepository;
import org.springframework.transaction.annotation.Transactional;
import org.springframework.data.jpa.repository.support.JpaEntityInformation;

import javax.persistence.EntityManager;
import java.io.Serializable;

public class CustomRepositoryImpl<t extends="" id="" serializable="">
extends SimpleJpaRepository>T, ID> implements CustomRepository<t id=""> {

    private final EntityManager entityManager;

    public CustomRepositoryImpl(JpaEntityInformation entityInformation, 
                                EntityManager entityManager) {
        super(entityInformation, entityManager);
        this.entityManager = entityManager;
    }

    @Override
    @Transactional
    public void refresh(T t) {
        entityManager.refresh(t);
    }
}
</t></t>
```

Key points are –

- Extend [SimpleJpaRepository](https://docs.spring.io/spring-data/jpa/docs/current/api/org/springframework/data/jpa/repository/support/SimpleJpaRepository.html) repository. The SimpleJpaRepository is the default implementation for CrudRepository
- Constructor is the [SimpleJpaRepository ](https://docs.spring.io/spring-data/jpa/docs/current/api/org/springframework/data/jpa/repository/support/SimpleJpaRepository.html)constructor taking the JpaEntityInformation and EntityManager objects.
- We save a local copy of the EntityManager
- refresh method simply calls the EntityManager.refresh method

The final step is to let Spring Data know that its base class is CustomRepositoryImpl –

```
package com.glenware.springboot;

import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;
import org.springframework.data.jpa.repository.config.EnableJpaRepositories;

import com.glenware.springboot.repository.CustomRepositoryImpl;

@SpringBootApplication
@EnableJpaRepositories(repositoryBaseClass = CustomRepositoryImpl.class)
public class ParkrunpbApplication {
   public static void main(String[] args) {
      SpringApplication.run(ParkrunpbApplication.class, args);
   }
}
```

Key Points –

- [EnableJpaRepositories](https://docs.spring.io/spring-data/data-jpa/docs/current/api/org/springframework/data/jpa/repository/config/EnableJpaRepositories.html#basePackages--) -This annotation enables JPA repositories, and will scan com.glenware.springboot by default
- repositoryBaseClass attribute is used to let the Spring Data configuration know we are overriding the default base class

## All Together Now

We can then use this repository in our classes so we change our repository from the previous [post](https://www.javabullets.com/access-entitymanager-spring-data-jpa/) from CrudRepository to extend CustomRepository –

```
package com.glenware.springboot.repository;

import com.glenware.springboot.form.ParkrunCourse;

public interface ParkrunCourseRepository 
   extends CustomRepository<parkruncourse long=""> {
}
</parkruncourse>
```

We can now access the EntityManager.refresh method using –

```
parkrunCourseRepository.refresh( parkrunCourse );
```

The above code was tested by running it against Spring Boot (1.5.6-Release) which used Spring Data JPA 1.11.6.Release. I can add the code to github if requested

## Gotcha’s

One area you need to check is what version of Spring Data JPA you are running for extending repositories. I’ve had to adapt this approach for older repositories, although this is the current method using Spring Data JPA 1.11.6 Release