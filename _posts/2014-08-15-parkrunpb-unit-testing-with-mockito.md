---
id: 2111
title: 'parkrunPB &#8211; Unit Testing with Mockito'
date: '2014-08-15T21:17:32+00:00'
author: 'Martin Farrell'
layout: post
guid: 'http://glenware.wordpress.com/?p=196'
permalink: /parkrunpb-unit-testing-with-mockito/
geo_public:
    - '0'
thrive_post_fonts:
    - '[]'
categories:
    - Java
tags:
    - mockito
    - parkrunPB
    - Spring
    - 'spring MVC'
    - Spring3
    - Spring4
---

The final part of the spring jigsaw is unit testing. Actually this should be the first part, but I felt it was important to see the end result before focusing on how to test the application.

**Set up**

```

<!-- TESTING DEPENDENCIES -->
<dependency>
   <groupId>org.hamcrest</groupId>
   <artifactId>hamcrest-all</artifactId>
   <version>1.3</version>
</dependency>
<dependency>
   <groupId>junit</groupId>
   <artifactId>junit</artifactId>
   <version>4.10</version>
<exclusions>
   <exclusion>
      <artifactId>hamcrest-core</artifactId>
      <groupId>org.hamcrest</groupId>
   </exclusion>
</exclusions>
</dependency>
<dependency>
   <groupId>org.mockito</groupId>
   <artifactId>mockito-core</artifactId>
   <version>1.9.0</version>
</dependency>
```

**Example Class**

```

public class ParkrunDAOTest {
   private ParkrunDAOImpl parkrunDAOImpl;
   private SessionFactory sessionFactory;
   private Session session;
   private Query query;
   @Before
   public void setUp() {
      parkrunDAOImpl = new ParkrunDAOImpl();
      sessionFactory = mock(SessionFactory.class);
      session = mock(Session.class);
      query = mock(Query.class);
      ReflectionTestUtils.setField(parkrunDAOImpl, "sessionFactory", sessionFactory);
   }
   @Test
   public void addPRCourse() {
      PRRegion prRegion = new PRRegion();
      prRegion.setId(1);
      prRegion.setPrName("TestRegion");
      PRCourse prCourse = new PRCourse();
      prCourse.setId(1);
      prCourse.setPrName("Test");
      prCourse.setAverageTime(1200);
      prCourse.setUrl("http://test");
      prCourse.setPrregion(prRegion);
      when(sessionFactory.getCurrentSession()).thenReturn(session);
      // call the method we wish to test...
      parkrunDAOImpl.addPRCourse(prCourse);
      // verify the method was called...
      verify(sessionFactory.getCurrentSession()).save(prCourse);
      // because there's no state to examine, we're done
   }
   @Test
   public void getPRCourse() {
      PRRegion prRegion = new PRRegion();
      prRegion.setId(1);
      prRegion.setPrName("TestRegion");
      PRCourse prCourse = new PRCourse();
      prCourse.setId(1);
      prCourse.setPrName("Test");
      prCourse.setAverageTime(1200);
      prCourse.setUrl("http://test");
      prCourse.setPrregion(prRegion);
      when(sessionFactory.getCurrentSession()).thenReturn(session);
      when(sessionFactory.getCurrentSession().get(PRCourse.class, 1)).thenReturn(prCourse);
      PRCourse fromDAO = parkrunDAOImpl.getPRCourse(1);
      // THEN
      assertNotNull(fromDAO);
      assertSame(fromDAO, prCourse);
   }
}
```

**Key Points**

The key point with mockito is that we are creating mock objects to allow us to test specific functionality.

- setUp – create mock Session and Query objects, and inject them into the parkrunDAOImpl
- @Test – each test is marked with this annotation
- @Test – each test sets up its data as required, then tests how the method should respond
- Tests are undertaken using the assert methods