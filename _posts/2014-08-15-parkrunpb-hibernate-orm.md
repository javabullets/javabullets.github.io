---
id: 2110
title: 'parkrunPB &#8211; Hibernate ORM'
date: '2014-08-15T20:53:18+00:00'
author: 'Martin Farrell'
layout: post
guid: 'http://glenware.wordpress.com/?p=192'
permalink: /parkrunpb-hibernate-orm/
geo_public:
    - '0'
thrive_post_fonts:
    - '[]'
categories:
    - Java
    - Spring
tags:
    - hibernate
    - parkrunPB
    - 'spring MVC'
    - Spring3
    - Spring4
---

There are a number of different approaches to connect a Java application to a database. These include –

- JDBC
- JPA
- Spring SQL Templates
- Object Relational Mappers – Toplink, Hibernate

I have opted for Hibernate for this application as its one of the most common ORM’s.

The advantages of an ORM are –

- Reduce development time and costs
- Vendor independence

The disadvantages are –

- SQL can be poor quality
- Poor preformance on complex queries
- Developers can have poor understanding of SQL

**Spring/Hibernate Configuration – spring-servlet.xml**

```

 	<bean id="propertyConfigurer"
 		class="org.springframework.beans.factory.config.PropertyPlaceholderConfigurer"
 		p:location="/WEB-INF/jdbc.properties" /> 
 	<bean id="dataSource"
 	      class="org.apache.commons.dbcp.BasicDataSource"
 		  destroy-method="close"
 		  p:driverClassName="com.mysql.jdbc.Driver"
 		  p:url="jdbc:mysql://127.0.0.1:3306/test"
 		  p:username=""
 		  p:password="" /> 
 	<bean id="sessionFactory"
 		  class="org.springframework.orm.hibernate3.annotation.AnnotationSessionFactoryBean">
 		<property name="dataSource" ref="dataSource" />
         <property name="annotatedClasses">
            <list>
               <value>com.glenware.parkrunpb.form.PRCourse</value>
               <value>com.glenware.parkrunpb.form.PRRegion</value>
            </list>
         </property>
 	 <property name="configurationClass">
 		<value>org.hibernate.cfg.AnnotationConfiguration</value>
	 </property>
 	 <property name="hibernateProperties">
 		<props>
 			<prop key="hibernate.dialect">org.hibernate.dialect.MySQLDialect</prop>
 			<prop key="hibernate.show_sql">true</prop>
			<prop key="hibernate.lazy">false</prop>
		</props>
 	</property>
 	</bean> 
 	<bean id="transactionManager"
 		class="org.springframework.orm.hibernate3.HibernateTransactionManager">
 		<property name="sessionFactory" ref="sessionFactory" />
 	</bean>
```

Key points –

- dataSource – this is configured with the url/username/password for the mysql instance being used. Another option is the jdbc.properties file
- sessionFactory – The configuration states we are only looking at annotations in – 
    - AnnotationSessionFactoryBean
    - PRCourse and PRRegion
    - MySQLDialect – this is the SQL dialect getting used in Hibernate – other options include
- transactionManager – We also need a TransactionManager, in this case HibernateTransactionManager

**Table Mapping**

One of the advantages is the ease with which tables can be mapped from the SQL DDL to the Java Object. Consider –

```

CREATE TABLE `prcourse` (
`prcourse_id` int(11) NOT NULL AUTO_INCREMENT,
`prregion_id` int(11),
`prname` varchar(256) DEFAULT NULL,
`url` varchar(256) DEFAULT NULL,
`averagetime` int(11) DEFAULT NULL,
`created` timestamp NOT NULL DEFAULT CURRENT_TIMESTAMP,
PRIMARY KEY (`prcourse_id`)
INDEX par_ind (`prregion_id`),
FOREIGN KEY (`prregion_id`)
)
```

Here the mapping is fairly simple –

- @Table(name = “PRCOURSE”) – maps to table
- @Id maps the primary key
- @Column maps the column names
- 

```

@Entity
@Table(name = "PRCOURSE")
public class PRCourse {
@Id
@Column(name = "PRCOURSE_ID")
@GeneratedValue
private Integer id;
@Column(name = "PRNAME")
@NotEmpty
private String prName;
@Column(name = "URL")
@NotEmpty
private String url;
@Column(name = "AVERAGETIME")
@Range(min = 1)
private int averageTime;
@ManyToOne
@JoinColumn(name="PRREGION_ID")
private PRRegion prregion;
}
```

The most complex part of this configuration is the one to many relationship between PRCourse and PRRegion, where we use the @ManyToOne annotation. The corresponding mapping ties prregion to PRCourse –

```

@OneToMany(mappedBy="prregion")
 private Set prCourses;
```

**Accessing Data**

This would be best explained using a UML diagram to show ParkrunDAOImpl implementing ParkrunDAO, and I’ll try and get round to adding one.

```

@Repository
@Transactional
public class ParkrunDAOImpl implements ParkrunDAO {
   @Autowired
   private SessionFactory sessionFactory;
   @Override
   public void addPRCourse(PRCourse prCourse) {
      sessionFactory.getCurrentSession().save(prCourse);
   }
   @Override
   public PRCourse getPRCourse(Integer id) {
      return (PRCourse) sessionFactory.getCurrentSession().get(PRCourse.class, id);
   }
   @Override
   public List<PRCourse> listPRCourse() {
      return sessionFactory.getCurrentSession().createQuery("from PRCourse order by prName").list();
   }
   @Override
   public void removePRCourse(Integer id) {
      PRCourse prCourse = (PRCourse) sessionFactory.getCurrentSession().load(PRCourse.class, id);
      if (prCourse != null) {
         sessionFactory.getCurrentSession().delete(prCourse);
      }
   }
   @Override
   public PRRegion getPRRegion(Integer id) {
      return (PRRegion) sessionFactory.getCurrentSession().get(PRRegion.class, id);
   }
   @Override
   public List<PRRegion> listPRRegion() {
      return sessionFactory.getCurrentSession().createQuery("from PRRegion order by regionName").list();
   }
}
```

The key point are –

- SessionFactory – this creates the connection to the database
- get method – return a specific instance
- createQuery – HQL – Hibernate specific query
- save – Add to database
- delete – Delete from database

**SQL Inserts**

This section contains the SQL should you wish to populate your database instance –

```

CREATE TABLE `prcourse` (
`prcourse_id` int(11) NOT NULL AUTO_INCREMENT,
`prregion_id` int(11),
`prname` varchar(256) DEFAULT NULL,
`url` varchar(256) DEFAULT NULL,
`averagetime` int(11) DEFAULT NULL,
`created` timestamp NOT NULL DEFAULT CURRENT_TIMESTAMP,
PRIMARY KEY (`prcourse_id`)
INDEX par_ind (`prregion_id`),
FOREIGN KEY (`prregion_id`)
)
CREATE TABLE `prregion` (
 `prregion_id` int(11) NOT NULL AUTO_INCREMENT,
 `regionname` varchar(256) DEFAULT NULL,
 `created` timestamp NOT NULL DEFAULT CURRENT_TIMESTAMP,
 PRIMARY KEY (`prregion_id`)
)
```

**Inserts**

```

 INSERT INTO PRREGION(REGIONNAME) VALUES ('East Midlands');
 INSERT INTO PRREGION(REGIONNAME) VALUES ('East of England');
 INSERT INTO PRREGION(REGIONNAME) VALUES ('Greater London');
 INSERT INTO PRREGION(REGIONNAME) VALUES ('North East England');
 INSERT INTO PRREGION(REGIONNAME) VALUES ('North West England');
 INSERT INTO PRREGION(REGIONNAME) VALUES ('Northern Ireland');
 INSERT INTO PRREGION(REGIONNAME) VALUES ('Overseas Military Bases');
 INSERT INTO PRREGION(REGIONNAME) VALUES ('Scotland');
 INSERT INTO PRREGION(REGIONNAME) VALUES ('South East England');
 INSERT INTO PRREGION(REGIONNAME) VALUES ('South West England');
 INSERT INTO PRREGION(REGIONNAME) VALUES ('Wales');
 INSERT INTO PRREGION(REGIONNAME) VALUES ('West Midlands');
 INSERT INTO PRREGION(REGIONNAME) VALUES ('Yorkshire and Humberside');
  INSERT INTO prcourse(prregion_id, prname, url, averagetime) VALUES (11, 'Inverness', 'http://www.parkrun.org.uk/inverness/', 1582);
 INSERT INTO prcourse(prregion_id, prname, url, averagetime) VALUES (11, 'Aberdeen', 'http://www.parkrun.org.uk/aberdeen/', 1586);
 INSERT INTO prcourse(prregion_id, prname, url, averagetime) VALUES (11, 'Dundee(Camperdown)', 'http://www.parkrun.org.uk/camperdown/', 1752);
 INSERT INTO prcourse(prregion_id, prname, url, averagetime) VALUES (11, 'St Andrews', 'http://www.parkrun.org.uk/standrews/', 1669);
 INSERT INTO prcourse(prregion_id, prname, url, averagetime) VALUES (11, 'Perth', 'http://www.parkrun.org.uk/perth/', 1620);
 INSERT INTO prcourse(prregion_id, prname, url, averagetime) VALUES (11, 'Edinburgh', 'http://www.parkrun.org.uk/edinburgh/', 1523);
 INSERT INTO prcourse(prregion_id, prname, url, averagetime) VALUES (11, 'Falkirk', 'http://www.parkrun.org.uk/falkirk/', 1612);
 INSERT INTO prcourse(prregion_id, prname, url, averagetime) VALUES (11, 'Tollcross', 'http://www.parkrun.org.uk/tollcross/', 1623);
 INSERT INTO prcourse(prregion_id, prname, url, averagetime) VALUES (11, 'Strathclyde', 'http://www.parkrun.org.uk/strathclyde/', 1586);
 INSERT INTO prcourse(prregion_id, prname, url, averagetime) VALUES (11, 'Victoria', 'http://www.parkrun.org.uk/victoria/', 1526);
 INSERT INTO prcourse(prregion_id, prname, url, averagetime) VALUES (11, 'Glasgow', 'http://www.parkrun.org.uk/glasgow/', 1585);
 INSERT INTO prcourse(prregion_id, prname, url, averagetime) VALUES (11, 'Eglington', 'http://www.parkrun.org.uk/eglinton/', 1681);
```