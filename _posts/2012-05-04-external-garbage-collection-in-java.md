---
layout: post
title: External garbage collection in java
date: 2012-05-04 10:28:00.000000000 +05:30
type: post
parent_id: '0'
published: true
password: ''
status: publish
categories:
- Java
tags: []
meta:
  blogger_blog: technohelpy.blogspot.com
  blogger_author: Ankit Katiyar
  blogger_permalink: "/2012/05/external-garbage-collection-in-java.html"
  blogger_internal: "/feeds/2635046121517897773/posts/default/6116397053749000836"
  _edit_last: '1'
  _yoast_wpseo_content_score: '30'
  _yoast_wpseo_primary_category: '56'
author:
  login: ankitkatiyar91
  email: ankitkatiyar67@gmail.com
  display_name: Ankit Katiyar
  first_name: Ankit
  last_name: Katiyar
permalink: "/external-garbage-collection-in-java/"
---
In java garbage collection is internally implemented. For it, JVM(Java Virtual Machine) uses a Class  
java.lang.Runtime.

The JVM's heap stores all objects created by an executing Java program. Objects are created by Java's "new" operator, and memory for new objects is allocated on the heap at runtime. Garbage collection is the process of automatically freeing objects that are no longer referenced by the program. This frees the programmer from having to keep track of when to free allocated memory, thereby preventing many potential bugs and headaches.

You can also externally &nbsp;set the max heap size for your JVM by the VM command  
_java -Xms_ .

If u &nbsp; want to implement garbage collection in your program u can use the method of _ **Runtime** _ class there is a native method defined in java that is used to call a garbage collector.

```
Runtime r = Runtime.getRuntime();
r.gc();

//One alternative to above code is until method from system class
System.gc();
// this does the same thing
```

