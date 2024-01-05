---
id: 451
title: 'Calling Stored Procedures From Spring Data JPA'
date: '2015-06-12T10:47:51+00:00'
author: 'Martin Farrell'
layout: post
guid: 'https://glenware.wordpress.com/?p=451'
permalink: /calling-stored-procedures-from-spring-data-jpa/
thrive_post_fonts:
    - '[]'
thrive_share_count:
    - '{"facebook":0,"twitter":0,"plusone":0,"pinterest":0,"linkedin":0,"total":0,"last_fetch":1530012231,"url":"https://www.javabullets.com/calling-stored-procedures-from-spring-data-jpa/"}'
categories:
    - 'Spring Boot'
    - 'Spring Data'
tags:
    - 'Spring Boot'
    - 'Spring Data'
    - 'Stored Procedures'
---

Ok Ive struggled on Calling Stored Procedures From Spring Data JPA for a while. My issue is that my SP’s dont return any values, so the normal JPA and Spring Data approaches outlined in my previous posts Spring Data JPA – Custom Repository.

# Calling Stored Procedures From Spring Data JPA

Consider the following procedure –

```

CREATE OR REPLACE PACKAGE test_pkg AS
PROCEDURE in_only_test (inParam1 IN VARCHAR2);
PROCEDURE in_and_out_test (inParam1 IN VARCHAR2, outParam1 OUT VARCHAR2);
END test_pkg;
/
CREATE OR REPLACE PACKAGE BODY test_pkg AS
PROCEDURE in_only_test(inParam1 IN VARCHAR2) AS
BEGIN
DBMS_OUTPUT.PUT_LINE('in_only_test');
END in_only_test;
PROCEDURE in_and_out_test(inParam1 IN VARCHAR2, outParam1 OUT VARCHAR2) AS
BEGIN
outParam1 := 'Woohoo Im an outparam, and this is my inparam ' || inParam1;
END in_and_out_test;
END test_pkg;
```

Ensure that you have granted the appropriate permissions to this class – eg –

```

GRANT EXECUTE ON TEST_PKG TO 'MYDB';
```

# Implementation

```

@Entity
@Table(name = "MYTABLE")
@NamedStoredProcedureQueries({
@NamedStoredProcedureQuery(name = "in_only_test", procedureName = "test_pkg.in_only_test",
parameters = {
@StoredProcedureParameter(mode = ParameterMode.IN, name = "inParam1", type = String.class)
}
),
@NamedStoredProcedureQuery(name = "in_and_out_test", procedureName = "test_pkg.in_and_out_test",
parameters = {
@StoredProcedureParameter(mode = ParameterMode.IN, name = "inParam1", type = String.class),
@StoredProcedureParameter(mode = ParameterMode.OUT, name = "outParam1", type = String.class)
}
)
})
public class MyTable implements Serializable {
}
```

The key points are –

- procedureName – This is the name of the stored procedure on the database
- name – This is the name of the StoredProcedure in the JPA ecosystem
- We then define the IN/OUT parameters

We then create the Spring Data JPA repository –

```

public interface MyTableRepository extends CrudRepository<MyTable, Long> {
@Procedure(name = "in_only_test")
void inOnlyTest(@Param("inParam1") String inParam1);
@Procedure(name = "in_and_out_test")
String inAndOutTest(@Param("inParam1") String inParam1);
}
```

The key points –

- @Procedure – the name parameter must match the name on @NamedStoredProcedureQuery
- @Param – Must match @StoredProcedureParameter name parameter

We can then call them –

```

// This version shows how a param can go in an be returned from a stored procedure
String inParam = "Hi Im an inputParam";
String outParam = myTableRepository.inAndOutTest(inParam);
Assert.assertEquals(outParam, "Woohoo Im an outparam, and this is my inparam Hi Im an inputParam");
// This version shows how to call a Stored Procedure which doesnt return any parameter -
myTableRepository.inOnlyTest(inParam);
```

# Other Tricks

Ive occasionally struggled with the above approach, and resorted to accessing the stored procedure as a native query through a custom repository.

This is done by defining a custom repository –

```

public interface MyTableRepositoryCustom {
void inOnlyTest(String inParam1);
}
```

We then make sure our main repository extends this interface –

```

public interface MyTableRepository extends CrudRepository<MyTable, Long>, MyTableRepositoryCustom {
}
```

We then define our custom repository –

```

public class MyTableRepositoryImpl implements MyTableRepositoryCustom {
@PersistenceContext
private EntityManager em;
@Override
public void inOnlyTest(String inParam1) {
this.em.createNativeQuery("BEGIN in_only_test(:inParam1); END;")
.setParameter("inParam1", inParam1)
.executeUpdate();
}
}
```

This can then be called in the normal way –

```

@Autowired
MyTableRepository myTableRepository;
// And to call the method -
myTableRepository.inOnlyTest(inParam1);
```

# Conclusions

Calling Stored Procedures From Spring Data JPA can be delivered using custom Spring Repositories.