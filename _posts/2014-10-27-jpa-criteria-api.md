---
id: 302
title: 'JPA Criteria API'
date: '2014-10-27T21:10:38+00:00'
author: 'Martin Farrell'
layout: post
guid: 'http://glenware.wordpress.com/?p=302'
permalink: /jpa-criteria-api/
geo_public:
    - '0'
thrive_post_fonts:
    - '[]'
categories:
    - Uncategorized
---

Ive got to say I don’t like the [JPA Criteria API](http://docs.oracle.com/javaee/6/api/javax/persistence/criteria/CriteriaBuilder.html) – some love it, but I much prefer JPQL. Consider the following JPQL –

**SELECT ag FROM agreement ag**

This would be expressed in the [Criteria API](http://docs.oracle.com/javaee/6/api/javax/persistence/criteria/CriteriaBuilder.html) by –

\[sourcecode lang=”java”\] CriteriaBuilder cb = em.getCriteriaBuilder();  
CriteriaQuery&lt;Agreement&gt; cq = cb.createQuery(Agreement.class); // Table  
Root&lt;Agreement&gt; agreement = cq.from(Agreement.class); // FROM Agreement ag  
cq.select(agreement); // SELECT ag  
TypedQuery&lt;Agreement&gt; q = em.createQuery(cq); // Note the preference of TypedQuery  
List&lt;Agreements&gt; allAgreements = q.getResultList();  
\[/sourcecode\] But the choice isn’t purely preference, and the Criteria API has its place when it comes to stricter Type safety. The reason for this is that the Criteria API is backed by a data metamodel of the underlying tables, so less room for the developer to muck things up by passing a letter into a field expecting a number.

\[sourcecode lang=”java”\] @Entity  
class Agreement {  
 private Long id;  
 private String agreementName;  
 private Integer agreementYear;  
 private List claims;  
 private User createdBy;  
} // JPA generates a metamodel –

@Static Metamodel(Agreement.class)  
public class Agreement \_ {  
 public static volatile SingularAttribute&lt;Agreement , Long&gt; id;  
 public static volatile SingularAttribute&lt;Agreement , String&gt; agreementName;  
 public static volatile SingularAttribute&lt;Agreement , Integer &gt; agreementYear;  
 public static volatile ListAttribute&lt;Agreement, Claims&gt; claims;  
 public static volatile SingularAttribute&lt;Agreement, User&gt; createdBy;  
}

// Example Query  
CriteriaBuilder cb = em.getCriteriaBuilder();  
CriteriaQuery cq = cb.createQuery(Agreement.class);  
Root&lt;Pet&gt; pet = cq.from(Agreement.class);  
EntityType&lt;Agreement&gt; Agreement\_ = agreement.getModel();  
\[/sourcecode\]

You can also access the metamodel directly

\[sourcecode lang=”java”\] Metamodel m = em.getMetamodel();  
EntityType&lt;Agreement&gt; Agreement\_ = m.entity(Agreement.class);  
\[/sourcecode\] 