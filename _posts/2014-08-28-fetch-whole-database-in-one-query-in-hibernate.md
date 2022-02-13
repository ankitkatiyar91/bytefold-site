---
layout: post
title: Fetch Whole Database in one Query in hibernate
date: 2014-08-28 02:26:00.000000000 +05:30
type: post
parent_id: '0'
published: true
password: ''
status: publish
categories:
- Hibernate
- Java
tags: []
meta:
  blogger_blog: bytefold.com
  blogger_author: Ankit Katiyar
  blogger_permalink: "/2014/08/fetch-whole-database-in-one-query-in.html"
  blogger_internal: "/feeds/2635046121517897773/posts/default/8565859511335169250"
  _edit_last: '1'
  _yoast_wpseo_content_score: '30'
  _yoast_wpseo_primary_category: '57'
  _yoast_wpseo_focuskw_text_input: Awesome feature and tricks to manage Entities
  _yoast_wpseo_focuskw: Awesome feature and tricks to manage Entities
  _yoast_wpseo_linkdex: '25'
author:
  email: ankit@bytefold.com
  display_name: Ankit Katiyar
  first_name: Ankit
  last_name: Katiyar
permalink: "/fetch-whole-database-in-one-query-in-hibernate/"
---
Hibernate is most widely used ORM tool for Java (at least by the time I am writing this). It has lot's of Awesome feature and tricks to manage Entities,  
Implicit Inheritance is one of them. You can use a parent to fetch some entity and it will load it's some entities.

**Key:** Every entity is a subclass of Object class. If you create criteria for Object class you will get all the entities data.
```java
Session session = factory.openSession();   
List l=session.createCriteria(Object.class).list();   
for (Object object : l) {    
 System.out.println("Object: "+l);   
}
```

&nbsp;

