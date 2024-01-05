---
id: 2310
title: 'JPA Entity Lifecycle'
date: '2017-08-06T19:14:51+00:00'
author: 'Martin Farrell'
layout: post
guid: 'https://www.javabullets.com/?p=2310'
permalink: /jpa-entity-lifecycle/
thrive_post_fonts:
    - '[]'
thrive_share_count:
    - '{"facebook":2,"twitter":0,"plusone":0,"pinterest":0,"linkedin":0,"total":2,"last_fetch":1527080388,"url":"https://www.javabullets.com/jpa-entity-lifecycle/"}'
image: /wp-content/uploads/2017/08/entityManager_javabullets.png
categories:
    - Java
    - Java8
    - JEE
    - JPA
    - SQL
tags:
    - 'Entity Lifecycle'
    - EntityManager
    - JEE
    - JPA
---

The last blog post about the [JPA EntityManager](https://www.javabullets.com/entitymanager/) didn’t cover the JPA Entity Lifecycle. This post builds on the original one by covering the JPA Entity Lifecycle and associated Lifecycle events

# JPA Entity Lifecycle

![JPA Entity Lifecycle](https://i0.wp.com/www.javabullets.com/wp-content/uploads/2017/08/entityManager_javabullets.png?resize=661%2C249&ssl=1)

As a reminder the purpose of the EntityManager is to the relationship between the JPA entity and the underlying datasource.

The above diagram shows the 5 key stages of JPA entity management –

- Object Doesnt Exist – This is a null object

```
<pre style="text-align: center;">MyObject myObject = null;
```

- New Object – Not associated with the EntityManager, and doesnt exist on database

```
<pre style="text-align: center;">MyObject myObject = new MyObject();
```

- Managed – This is the stage were the object becomes persisted and managed by the EntityManager. To do this we need to call the persist method from within a transaction. The object is then persisted to the database when the commit method is called

```
                           entityManager.getTransaction().begin();
                           MyObject myObject = new MyObject();
                           entityManager.persist(myObject);
                           entityManager.getTransaction().commit();
```

- Detached – This state removes the object from the EntityManager, but the object <span style="color: #ff0000;">**still**</span> exists on the database. Some EntityManager methods on a detached object will result in an IllegalArgumentException. The object can be reattached to the EntityManager through the merge method

```
<pre style="text-align: center;">entityManager.detach(myObject);
```

- Removed – Deletes the object from the database. Like persist this also needs to take place inside a transaction.

```
                           entityManager.getTransaction().begin();
                           entityManager.removed(myObject);
                           entityManager.getTransaction().commit();
```


# Callback Methods on JPA Entities

The final part of the JPA lifecycle event is the optional callback methods, and their associated annotations –

- @PrePerist / @PostPersist
- @PreRemove / @PostRemove
- @PreUpdate / @PostUpdate
- @PostLoad

The pre-methods are executed before the associated method action is executed(ie @PrePersist is executed before em.persist).

The post-methods are executed after the associated method action is executed(ie @PostPerist is executed after em.persist)

If either the pre- or post- methods/annotations throw an exception then the transaction will be rolled back.

## What can you use these Callback events for?

The pre- and post- perist methods are useful for setting timestamps for auditing, or set default values.