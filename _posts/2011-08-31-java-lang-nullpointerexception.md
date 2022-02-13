---
layout: post
title: java.lang.NullPointerException in Java
date: 2011-08-31 04:53:00.000000000 +05:30
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
  blogger_permalink: "/2011/08/javalangnullpointerexception.html"
  blogger_internal: "/feeds/2635046121517897773/posts/default/6068943828919565542"
  _edit_last: '1'
  _yoast_wpseo_focuskw_text_input: java.lang.NullPointerException in Java
  _yoast_wpseo_content_score: '30'
  _yoast_wpseo_primary_category: '56'
  _yoast_wpseo_focuskw: java.lang.NullPointerException in Java
  _yoast_wpseo_linkdex: '28'
author:
  login: ankitkatiyar91
  email: ankitkatiyar67@gmail.com
  display_name: Ankit Katiyar
  first_name: Ankit
  last_name: Katiyar
permalink: "/java-lang-nullpointerexception/"
---
 **_java.lang.NullPointerException_** in Java

In Java "java.lang.NullPointerException" exception is the one that every beginner&nbsp;face once a while. The reason is variable at which the error&nbsp;occurred, does not contain any value or object reference.  
  
  
**1.** If u define any reference variable and don't create any object for that variable&nbsp;  
**2.** &nbsp;If u define any reference variable globally but when u create the object then again define it

**Example:-**

```
Object a; // declare an object

a.hashcode(); // trying to call a method on the object. 
//"mind it! I have not initialized it yet"

// above line will definitely give you an exception, 
//obviously NullPointerException as reference a is still null.
```

**Solution:-&nbsp;**

```
Object a; // declare an object

a=new Object(); //initialized, Yes! you can do it on above line as will

a.hashcode();
```

&nbsp;

Wanna deep dive? Go [here](https://stackoverflow.com/questions/218384/what-is-a-nullpointerexception-and-how-do-i-fix-it)

