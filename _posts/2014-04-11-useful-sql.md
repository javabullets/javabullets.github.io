---
id: 97
title: 'Useful SQL'
date: '2014-04-11T10:18:19+00:00'
author: 'Martin Farrell'
layout: post
guid: 'http://glenware.wordpress.com/?p=97'
permalink: /useful-sql/
geo_public:
    - '0'
thrive_post_fonts:
    - '[]'
categories:
    - SQL
---

Im using this section to store useful tidbits of SQL or PLSQL

**Looping through an Array in PLSQL**

```
<pre class="lang:default decode:true ">
DECLARE
   TYPE array_t IS VARRAY(6) OF NUMBER(38,0);
   ARRAY array_t := array_t(305, 405, 505, 605, 1005, 1105);
BEGIN
   FOR i IN 1..ARRAY.count loop
       dbms_output.put_line(ARRAY(i));
   END loop;
END;
```

**Reset a sequence – Oracle**

```
<pre class="lang:default decode:true ">
-- Test
SELECT MY_SEQUENCE.nextval from dual;

-- Decrement by 1
ALTER SEQUENCE MY_SEQUENCE INCREMENT BY -1 MINVALUE 0;

-- Decrement as required
SELECT MY_SEQUENCE.nextval from dual;

-- Reset Sequence
ALTER SEQUENCE MY_SEQUENCE INCREMENT BY 1 MINVALUE 0;
```