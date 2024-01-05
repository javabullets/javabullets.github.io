---
id: 308
title: 'JPQL vs Criteria API'
date: '2014-10-27T21:20:32+00:00'
author: 'Martin Farrell'
layout: post
guid: 'http://glenware.wordpress.com/?p=308'
permalink: /jpql-vs-criteria-api/
geo_public:
    - '0'
thrive_post_fonts:
    - '[]'
categories:
    - Java
    - JEE
    - JPA
---

Ok this is ongoing and I’ll fill in the blanks as I get time

**Simple Queries**

**SELECT ag FROM Agreement ag**

\[sourcecode lang=”java”\] CriteriaBuilder cb = em.getCriteriaBuilder();  
CriteriaQuery&lt;Agreement&gt; cq = cb.createQuery(Agreement.class);  
Root&lt;Agreement&gt; agreement = cq.from(Agreement.class);  
cq.select(agreement);  
TypedQuery&lt;Agreement&gt; q = em.createQuery(cq);  
List&lt;Agreement&gt; allAgreements = q.getResultList();  
\[/sourcecode\] **SELECT DISTINCT ag.agreementYear FROM Agreement ag**

\[sourcecode lang=”java”\] CriteriaBuilder cb = em.getCriteriaBuilder();  
CriteriaQuery&lt;Agreement&gt; cq = cb.createQuery(Agreement.class);  
Root&lt;Agreement&gt; agreement = cq.from(Agreement.class);  
cq.select(agreement).distinct(true); // distinct == true  
TypedQuery&lt;Agreement&gt; q = em.createQuery(cq);  
List&lt;Agreement&gt; allAgreements = q.getResultList();  
\[/sourcecode\] **SELECT ag FROM Agreement ag WHERE ag.agreementYear = ‘2014’**

\[sourcecode lang=”java”\] // Without MetaModel  
cq = cb.createQuery(Agreement.class);  
Root&lt;Agreement&gt; agreement = cq.from(Agreement.class);  
cq.select(agreement).where(  
 cb.equal(agreement.get("agreementYear"), "2014")  
); // Without MetaModel – typesafety thru metamodel  
cq = cb.createQuery(Agreement.class);  
Root&lt;Agreement&gt; agreement = cq.from(Agreement.class);  
cq.select(agreement).where(cb.equal(  
 agreement.get(Agreement\_.agreementYear), 2014)  
);  
\[/sourcecode\]

The rest of the examples will use the metamodel.

**SELECT ag FROM Agreement ag WHERE ag.agreementName LIKE ‘%test%’**

\[sourcecode lang=”java”\] cq = cb.createQuery(Agreement.class);  
Root&lt;Agreement&gt; agreement = cq.from(Agreement.class);  
cq.select(agreement).where( cb.like(  
 agreement.get(Agreement\_.agreementName), "%test%" )  
 )  
);  
\[/sourcecode\] **SELECT ag FROM Agreement ag WHERE ag.agreementName IS NULL**

\[sourcecode lang=”java”\] cq = cb.createQuery(Agreement.class);  
Root&lt;Agreement&gt; agreement = cq.from(Agreement.class);  
cq.select(agreement).where(  
 agreement.get(Agreement\_.agreementName).isNull()  
);  
\[/sourcecode\] **SELECT ag FROM Agreement ag WHERE ag.claims IS EMPTY**

\[sourcecode lang=”java”\] cq = cb.createQuery(Agreement.class);  
Root&lt;Agreement&gt; agreement = cq.from(Agreement.class);  
cq.select(agreement).where(  
 cb.isEmpty(agreement.&lt;Collection&gt;get("claims"))  
);  
\[/sourcecode\] **Joins**

The above queries are nice – but the real power of a query language is JOIN’s

**JOIN == INNER JOIN**

**SELECT ag FROM Agreement ag JOIN ag.claims cl**

\[sourcecode lang=”java”\] CriteriaBuilder cb = em.getCriteriaBuilder();  
CriteriaQuery&lt;Agreement&gt; query = cb.createQuery(Agreement.class);  
Root&lt;Agreement&gt; agreement = query.from(Agreement.class);  
Join&lt;Claim, Agreement&gt; claims = agreement.join("claims");  
query.select(claims);  
List&lt;Claim&gt; claims = em.createQuery(query).getResultList();  
\[/sourcecode\] **SELECT ag FROM Agreement ag JOIN ag.claims cl WHERE cl.amount = 1000**

\[sourcecode lang=”java”\] CriteriaBuilder cb = em.getCriteriaBuilder();  
CriteriaQuery&lt;Agreement&gt; query = cb.createQuery(Agreement.class);  
Root&lt;Agreement&gt; agreement = query.from(Agreement.class);  
Join&lt;Claim, Agreement&gt; claims = agreement.join("claims");  
query.select(claims).where(cb.equal(agreement .get("amount"), 1000));  
List&lt;Claim&gt; claims = em.createQuery(query).getResultList();  
\[/sourcecode\] **LEFT JOIN == LEFT OUTER JOIN**

**SELECT ag FROM Agreement ag LEFT JOIN ag.claims cl WHERE cl.amount &gt; 1000**

\[sourcecode lang=”java”\] CriteriaQuery cq = cb.createQuery(Agreement.class);  
Root&lt;Agreement&gt; = cq.from(Agreement.class);  
SetJoin claims = agreement.join(Agreement\_.claims, JoinType.LEFT);  
cq.select(agreement).where( cb.equal(claims.get(Claim\_.amount), 1000) );  
List result = em.createQuery(cq).getResultList();  
\[/sourcecode\] **FETCH JOIN**

A FETCH JOIN enables the fetching of an association as a side effect of the execution of a query

The effect of the FETCH is to not return the associated claims

SELECT ag FROM agreement ag LEFT JOIN FETCH ag.claims cl WHERE cl.amount = 1000

\[sourcecode lang=”java”\] CriteriaQuery cq = cb.createQuery(Agreement.class);  
Root&lt;Agreement&gt; agreement = cq.from(Agreement.class);  
SetJoin&lt;Agreement, Claims&gt; claims = agreement.join(Agreement\_.claims, JoinType.LEFT);  
cq.select(agreement).where( cb.equal(claims.get(Claim\_.amount), 1000) );  
agreement.fetch("claims");  
List result = em.createQuery(cq).getResultList();  
\[/sourcecode\] **References**

[http://en.wikibooks.org/wiki/Java\_Persistence/Criteria](http://en.wikibooks.org/wiki/Java_Persistence/Criteria)  
<http://docs.oracle.com/javaee/6/tutorial/doc/gjrij.html>