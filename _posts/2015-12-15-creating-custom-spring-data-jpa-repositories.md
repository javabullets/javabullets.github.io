---
id: 497
title: 'Custom Spring Data JPA Repositories for Database Views'
date: '2015-12-15T13:28:56+00:00'
author: 'Martin Farrell'
layout: post
guid: 'https://glenware.wordpress.com/?p=497'
permalink: /creating-custom-spring-data-jpa-repositories/
thrive_post_fonts:
    - '[]'
image: /wp-content/uploads/2017/05/steve-taylor-110050.jpg
categories:
    - 'Spring Data'
tags:
    - 'custom repository'
    - 'Spring Data'
    - views
---

This post looks at how to define a Custom Spring Data JPA Repositories for Database Views. In this case I am creating a Repository for database views so I dont expose the Create, Remove or Update methods in the CrudRepository.

Here is my simple starter table and view –

```

CREATE TABLE my_table (
my_table_id NUMBER,
my_table_type VARCHAR2(100)
);
CREATE VIEW my_view AS
SELECT my_table_id, my_table_type FROM my_table;
```

Populate with sample data –

```

INSERT INTO my_table(MY_TABLE_ID, MY_TABLE_TYPE) VALUES (1, 'Value 1');
INSERT INTO my_table(MY_TABLE_ID, MY_TABLE_TYPE) VALUES (2, 'Value 2');
```

Now create a JPA object for this –

```

@Entity
@Table(name = "my_view")
public class MyView implements Serializable {
private static final long serialVersionUID = 1L;
@Id
@Column(name = "my_table_id")
private Long myTableId;
@Column(name = "my_table_type")
private String myTableType;
// add getters and setters
}
```

Now the interesting bit – define our ViewRepository –

```

import org.springframework.data.repository.*;
@NoRepositoryBean
public interface ViewRepository<T, ID extends Serializable> extends Repository<T, ID> {
T findOne(ID id);
boolean exists(ID id);
Iterable<T> findAll();
long count();
}
```

The key points are –

Use @NoRepositoryBean to prevent the Repository from being created. This is similar to creating an abstract class

We add the method definitions as defined in CrudRepository, but exclude delete and save.

Spring Data JPA’s SimpleJpaRepository will fill in the missing information for our repository and create the implementation class.

We can now create an instance of the repository –

```

public interface MyViewRepository extends ViewRepository<MyView, Long> {}
```

We can then call these methods –

```

@Autowired
private MyViewRepository myViewRepository;
// ...
@Override
public long getMyViewCount() {
return myViewRepository.count();
}
}
```