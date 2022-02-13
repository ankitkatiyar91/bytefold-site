---
layout: post
title: Not In query In Liferay Dynamic Query
date: 2013-08-19 07:33:00.000000000 +05:30
type: post
parent_id: '0'
published: true
password: ''
status: publish
categories:
- Miscellaneous
tags: []
meta:
  blogger_blog: bytefold.com
  blogger_author: Ankit Katiyar
  blogger_permalink: "/2013/08/not-in-query-in-liferay-dynamic-query.html"
  blogger_internal: "/feeds/2635046121517897773/posts/default/3569513229157446587"
  _edit_last: '1'
author: ankit_katiyar
permalink: "/not-in-query-in-liferay-dynamic-query/"
---
 In Liferay dynamic query you can create a query for not in condition like

&nbsp;

&nbsp;

```sql
SELECT * FROM traders WHERE traderId NOT IN(455,466)
```

```java
dynamicQuery.add(PropertyFactoryUtil.forName("primaryKey.traderId")
.notIn(StatutoryLocalServiceUtil.dynamicQuery()      
.add(PropertyFactoryUtil.forName("primaryKey.traderId").eq(455))      
.add(PropertyFactoryUtil.forName("primaryKey.traderId").eq(456))      
.setProjection(ProjectionFactoryUtil.property("primaryKey.traderId"))));
```

&nbsp;

