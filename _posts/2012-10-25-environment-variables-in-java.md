---
layout: post
title: Environment Variables In Java
date: 2012-10-25 02:20:00.000000000 +05:30
type: post
parent_id: '0'
published: true
password: ''
status: publish
categories:
- Java
tags: []
meta:
  blogger_blog: bytefold.com
  blogger_author: Ankit Katiyar
  blogger_permalink: "/2012/10/environment-variables.html"
  blogger_internal: "/feeds/2635046121517897773/posts/default/4544628446130917337"
  _edit_last: '1'
  _yoast_wpseo_content_score: '30'
  _yoast_wpseo_primary_category: '56'
author:
  email: ankit@bytefold.com
  display_name: Ankit Katiyar
  first_name: Ankit
  last_name: Katiyar
permalink: "/environment-variables-in-java/"
---
&nbsp;

Environment Variables &nbsp;are variables that are added in any Operating system that can be accessed from anywhere in OS without using the complete(Real) location.

&nbsp;

In a java Application System.getenv(); is used to find out the system environment variables

### **Example:-**

```java
import java.util.Map;
```

public class EnvMap {  
public static void main (String[] args) {  
Map\<String, String\> env = System.getenv();  
for (String envName : env.keySet()) {  
System.out.format("%s=%s%n",  
envName,  
env.get(envName));  
}  
}  
}

&nbsp;

