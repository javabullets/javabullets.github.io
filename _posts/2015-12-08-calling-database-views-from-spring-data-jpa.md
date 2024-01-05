---
id: 492
title: 'Calling Database Views From Spring Data JPA'
date: '2015-12-08T16:11:29+00:00'
author: 'Martin Farrell'
layout: post
guid: 'https://glenware.wordpress.com/?p=492'
permalink: /calling-database-views-from-spring-data-jpa/
geo_public:
    - '0'
thrive_post_fonts:
    - '[]'
thrive_share_count:
    - '{"facebook":0,"twitter":0,"plusone":0,"pinterest":null,"linkedin":0,"total":0,"last_fetch":1581921808,"url":"https://www.javabullets.com/calling-database-views-from-spring-data-jpa/"}'
image: /wp-content/uploads/2017/05/espresso-1.jpg
categories:
    - 'Spring Data'
tags:
    - 'database views'
    - 'Spring Data'
---

Im not going to go into pro’s and con’s as to whether database views are the correct approach with a JPA subsystem, but will say that a lot of the time there isnt a discussion as the view already exists and you will just need to use it to complete your task

Lets consider this view –

```

CREATE VIEW my_view AS
SELECT my_view_id, my_view_name FROM my_table;
```

We map this to a JPA object as –

```

@Entity
@Table(name = "my_view")
public class MyView implements Serializable {
private static final long serialVersionUID = 1L;
@Id
@Column(name = "my_view_id")
private Long myViewId;
@NotNull
@Column(name = "my_view_name")
private String myViewName;
}
```

We then create a repository –

```

import org.springframework.data.repository.CrudRepository;
public interface MyViewRepository extends CrudRepository<MyView, Long> {
}
```

We can then call this –

```

@Autowired
private MyViewRepository myViewRepository;
// ...
long count = myViewRepository.count();
```