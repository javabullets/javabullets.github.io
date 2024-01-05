---
id: 437
title: 'Spring Configuration &#8211; Version Numbers in Schema Locations'
date: '2015-05-19T13:27:21+00:00'
author: 'Martin Farrell'
layout: post
guid: 'https://glenware.wordpress.com/?p=437'
permalink: /spring-configuration-version-numbers-in-schema-locations/
thrive_post_fonts:
    - '[]'
categories:
    - Uncategorized
---

I recently had the following compilation issue –

`The matching wildcard is strict, but no declaration can be found for element 'tx:annotation-driven'.`

The issue was due to conflicting spring schema locations due to the use of version numbers in the schema’s. The suggested best practice is to remove the version numbers –

`xsi:schemaLocation=http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans-3.2.xsd`

Would become –

`xsi:schemaLocation=http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd`

The reasoning being that spring will seek the highest versions of the schema that it can find in the Jars, and you don’t have to maintain the versioning in your configuration files. This can then be controlled using your maven pom, or build process