---
id: 449
title: 'Spring Data JPA &#8211; Custom Repository'
date: '2015-06-12T10:42:26+00:00'
author: 'Martin Farrell'
layout: post
guid: 'https://glenware.wordpress.com/?p=449'
permalink: /spring-data-jpa-custom-repository/
thrive_post_fonts:
    - '[]'
categories:
    - 'Spring Data'
tags:
    - JPA
    - Spring
    - 'Spring Data JPA'
---

Another useful feature of Spring Data is defining custom repositories. These involve defining two additional artifacts for your Spring data repository –

- Custom Repository interface
- Custom Repository implementation

**Example**

1\. Define custom repository interface –

```

public interface CustomExampleRepositoryCustom {
Integer customExampleMethod(String stringIn);
}
```

2\. Define implementation class –

```

public class CustomExampleRepositoryCustomImpl implements CustomExampleRepositoryCustom {
@PersistenceContext
private EntityManager em;
@Override
public Integer customExampleMethod(String stringIn) {
return new Integer(1);
}
}
```

3\. Extend your Spring Data Repository –

```

public interface CustomExampleRepository extends CrudRepository<Documents, Long>, CustomExampleRepositoryCustom {
}
```

4\. We can then inject and call the method as we normally would –

```

@Autowired
private CustomExampleRepository customExampleRepository;
Integer callCustomExampleMethodInteger = customExampleRepository.customExampleMethod("Hello World");
```