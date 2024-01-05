---
id: 276
title: 'JPA Lifecycle'
date: '2014-10-20T21:10:41+00:00'
author: 'Martin Farrell'
layout: post
guid: 'http://glenware.wordpress.com/?p=276'
permalink: /jpa-lifecycle/
geo_public:
    - '0'
thrive_post_fonts:
    - '[]'
categories:
    - Uncategorized
---

In JPA an objects can either be *managed* or *detached*. Managed changes will be reflected to the datastore, and detached changes wont be reflected in the datastore

This is best illustrated through the Persistence Manager Lifecycle –

TBD

**Managed**

- Read in current persistence context – tracks changes and object identity
- Changes reflected to datastore
- If the same object read again in same context – then == applies
- Not managed by more than one Persistence Context
- Managed should only reference managed objects

An object becomes managed by –

- EntityManager.persist() on new object
- EntityManager.merge() on detached object

**Detached**

- Not managed by current persistence context
- Changes not reflected to datastore
- Should only reference detached objects

An object becomes detacted by –

– new object detached until persist called

- EntityManager.detach()
- EntityManager.close()
- EntityManager.remove()
- EntityManager.flush()

In coding terms –

\[sourcecode lang=”java”\] MyEntity e = new MyEntity(); // scenario 1  
// tran starts  
em.persist(e);  
e.setSomeField(someValue);  
// tran ends, and the row for someField is updated in the database

// scenario 2  
// tran starts  
e = new MyEntity();  
em.merge(e);  
e.setSomeField(anotherValue);  
// tran ends but the row for someField is not updated in the database  
// (you made the changes \*after\* merging)

// scenario 3  
// tran starts  
e = new MyEntity();  
MyEntity e2 = em.merge(e);  
e2.setSomeField(anotherValue);  
// tran ends and the row for someField is updated  
// (the changes were made to e2, not e)  
\[/sourcecode\]

Thanks to [stackoverflow](http://stackoverflow.com/questions/1069992/jpa-entitymanager-why-use-persist-over-merge) for this example