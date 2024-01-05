---
id: 292
title: 'JPA Entity Relationships'
date: '2014-10-27T20:03:08+00:00'
author: 'Martin Farrell'
layout: post
guid: 'http://glenware.wordpress.com/?p=292'
permalink: /jpa-entity-relationships/
geo_public:
    - '0'
thrive_post_fonts:
    - '[]'
categories:
    - Uncategorized
---

This section covers the more common entity mappings between Object Models and Database. Its often easier to use the tools to create these relationships, but its worth knowing the basics –

- One-to-One (Unidirectional)
- One-to-One (Bidirectional)
- Many-to-One (Unidirectional)
- Many-to-One (Bidirectional)
- Many-to-Many (Both Directions)

**One-to-One(Unidirectional)**

User Address

user\_id  
address\_id ———&gt; address\_id

*Tables*  
\[sourcecode lang=”sql”\] CREATE TABLE USER  
(  
 USER\_ID NUMBER(38,0),  
 ADDRESS\_ID NUMBER(38,0)  
);

CREATE TABLE ADDRESS  
(  
 ADDRESS\_ID NUMBER(38,0)  
);  
\[/sourcecode\]

*Classes*

\[sourcecode lang=”java”\] @Entity  
public class User {  @Id  
 @Column(name="USER\_ID")  
 private int userId;

 @OneToOne  
 @JoinColumn(name="ADDRESS\_ID")  
 private Address address;

}

@Entity  
public class Address {

 @Id  
 @Column(name="ADDRESS\_ID")  
 private int addressId;

}  
\[/sourcecode\]

**One-to-One (Bidirectional)**

We could also make the User-Address relationship bidirectional –

*Classes*

\[sourcecode lang=”java”\] @Entity  
public class User {  @Id  
 @Column(name="USER\_ID")  
 private int userId;

 @OneToOne  
 @JoinColumn(name="ADDRESS\_ID")  
 private Address address;

}

@Entity  
public class Address {

 @Id  
 @Column(name="ADDRESS\_ID")  
 private int addressId;

 @OneToOne(mappedBy="address")  
 private User user;

}  
\[/sourcecode\]

**Many-to-One (Unidirectional)**

 Role User

 user\_id  
role\_id ———&gt; role\_id

*Tables*

\[sourcecode lang=”sql”\] CREATE TABLE USER  
(  
 ROLE\_ID NUMBER(38,0),  
 USER\_ID NUMBER(38,0)  
); CREATE TABLE ROLE  
(  
 ROLE\_ID NUMBER(38,0)  
);  
\[/sourcecode\]

*Classes*

\[sourcecode lang=”java”\] @Entity  
public class User {  @Id  
 private int userId;

 @ManyToOne  
 private Role role;

}

@Entity  
public class Role {

 @Id  
 private int roleId;

}  
\[/sourcecode\]

**Many-to-One (Bidirectional)**

*Classes*

\[sourcecode lang=”java”\] @Entity  
public class User {  @Id  
 private int userId;

 @ManyToOne  
 private Role role;

}

@Entity  
public class Role {

 @Id  
 private int roleId;

 @OneToMany(mappedBy="role")  
 private Collection users;

}  
\[/sourcecode\]

**Many-to-Many (Both Directions)**

One user could also have many roles through a JOIN table –

 USER USER\_ROLE ROLE

user\_id ———&gt; user\_id  
 role\_id ———-&gt; role\_id

*Tables*

\[sourcecode lang=”sql”\] CREATE TABLE USER  
(  
 USER\_ID NUMBER(38,0)  
); CREATE TABLE USER\_ROLE  
(  
 USER\_ID NUMBER(38,0),  
 ROLE\_ID NUMBER(38,0)  
);

CREATE TABLE ROLE  
(  
 ROLE\_ID NUMBER(38,0)  
);  
\[/sourcecode\]

*Classes*

\[sourcecode lang=”java”\] @Entity  
public class User {  @Id  
 private int userId;

 @ManyToMany  
 @JoinTable(name="USER\_ROLE",  
 joinColumns=@JoinColumn(name="USER\_ID"),  
 inverseJoinColumns=@JoinColumn(name="ROLE\_ID"))  
 private Collection roles;

}

@Entity  
public class Role {

 @Id  
 private int roleId;

 @ManyToMany(mappedBy="roles")  
 private Collection user;

}  
\[/sourcecode\]

**References**

http://en.wikibooks.org/wiki/Java\_Persistence/