---
id: 273
title: EntityManager
date: '2014-10-20T20:48:11+00:00'
author: 'Martin Farrell'
layout: post
guid: 'http://glenware.wordpress.com/?p=273'
permalink: /entitymanager/
geo_public:
    - '0'
thrive_post_fonts:
    - '[]'
thrive_share_count:
    - '{"facebook":0,"twitter":0,"plusone":0,"pinterest":0,"linkedin":0,"total":0,"last_fetch":1505142972,"url":"https://www.javabullets.com/entitymanager/"}'
categories:
    - Uncategorized
---

This section is a walkthru of the methods in [EntityManager](http://docs.oracle.com/javaee/6/api/javax/persistence/EntityManager.html)

All of these methods require –

- Persistence context
- Action only completed when complete or flushed
- Requires Transaction Context
- Only work on @Entity
- Doesnt work on @Embeddable

[EntityManager.close()](http://docs.oracle.com/javaee/6/api/javax/persistence/EntityManager.html#close%28%29)

- Close EntityManager

[EntityManager.flush()](http://docs.oracle.com/javaee/6/api/javax/persistence/EntityManager.html#flush%28%29)

- Write changes to DB before Transaction Committed
- Uses – populate Ids, clear batches during batch processing

[EntityManager.getDelegate()](http://docs.oracle.com/javaee/6/api/javax/persistence/EntityManager.html#getDelegate%28%29)

- Access Underlying EntityManager implementation

[EntityManager.getReference()](http://docs.oracle.com/javaee/6/api/javax/persistence/EntityManager.html#getReference%28java.lang.Class,%20java.lang.Object%29)

- Get object placeholder without loading object – gives performance advantages
- Stand-in for related object
- Does not verify existence of object on datastore – so could result in foreign key constraint

AgreementState agreementState = entityManager.getReference(AgreementState.class, agreementStateId);  
Agreement agreement = new Agreement();  
…  
entityManager.persist(agreement);  
agreement.setAgreementState(agreementState);  
…  
entityManager.commit();

[EntityManager.merge()](http://docs.oracle.com/javaee/6/api/javax/persistence/EntityManager.html#merge%28T%29)

- merges into persistence context
- Normally called for existing objects
- Only Entity objects, not Embeddable

Agreement managementAgreement = em.merge(detachAgreement);

[EntityManager.persist()](http://docs.oracle.com/javaee/6/api/javax/persistence/EntityManager.html#persist%28java.lang.Object%29)

- Called within transaction
- Inserts new @Entity object into datastore
- Registers with EntityManager
- Id assigned when persist() called

Agreement agreement = new Agreement();  
agreement.setAgreement(“Test name”);  
entityManager.persist(agreement);

[EntityManager.refresh()](http://docs.oracle.com/javaee/6/api/javax/persistence/EntityManager.html#refresh%28java.lang.Object%29)

- Refresh Managed Object with whats currently on database
- Not for detached objects

entityManager.refresh(refreshAgreement);

[EntityManager.remove()](http://docs.oracle.com/javaee/6/api/javax/persistence/EntityManager.html#remove%28java.lang.Object%29)

- Delete object

Agreement agreementToBeDeleted = entityManager.find(Agreement.class, agreementId);  
entityManager.remove(agreementToBeDeleted);