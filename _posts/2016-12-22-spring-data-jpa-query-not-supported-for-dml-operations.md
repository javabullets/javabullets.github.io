---
id: 1129
title: 'Spring Data JPA &#8211; @Query &#8211; Not supported for DML operations'
date: '2016-12-22T10:25:15+00:00'
author: 'Martin Farrell'
layout: post
guid: 'https://glenware.wordpress.com/?p=1129'
permalink: /spring-data-jpa-query-not-supported-for-dml-operations/
publicize_twitter_user:
    - glenwareltd
thrive_post_fonts:
    - '[]'
categories:
    - Java
---

I was wanting to update a database table status with a list of ID’s so decided the simplest way was to use @Query and JPAQL –

```
public interface MyJPAObjectRepository extends BaseRepository<MyJPAObject, String> {
    @Query("update MyJPAObject m set m.status = ?1 where m.id in ?2")
    void updateMyJPAObjectStatus(Status status, List<String> idList);
}
```

When I ran the code I got this exception –

```
Caused by: org.hibernate.hql.internal.QueryExecutionRequestException: Not supported for DML operations [update com.glenware.MyJPAObject m set m.status = ?1 where m.id in (:x2_0_)]
```

The reason is the code is calling execute, and we need executeUpdate. The fix is fairly simple – you add the @Modifying annotation –

```
public interface MyJPAObjectRepository extends BaseRepository<MyJPAObject, String> {
    @Modifying
    @Query("update MyJPAObject m set m.status = ?1 where m.id in ?2")
    void updateMyJPAObjectStatus(Status status, List<String> idList);
}
```

The [@Modifying](http://docs.spring.io/spring-data/jpa/docs/current/api/org/springframework/data/jpa/repository/Modifying.html) annotaion is used to instruct spring-data that this is a “modifying-query”, and allow DML operations

##### Reference 

http://docs.spring.io/spring-data/jpa/docs/1.10.6.RELEASE/reference/html/#jpa.modifying-queries