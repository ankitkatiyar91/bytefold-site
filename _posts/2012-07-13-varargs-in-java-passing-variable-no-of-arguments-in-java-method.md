---
layout: post
title: Varargs in java (Passing variable no of arguments in java method)
date: 2012-07-13 04:04:00.000000000 +05:30
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
  blogger_permalink: "/2012/07/varargs-in-java-passing-variable-no-of.html"
  blogger_internal: "/feeds/2635046121517897773/posts/default/1878297087044921893"
  _edit_last: '1'
  _yoast_wpseo_content_score: '60'
  _yoast_wpseo_primary_category: '56'
  _wp_old_slug: varargs-in-java-passing-variable-no-of-arguments-in-java-program
author: ankit_katiyar
permalink: "/varargs-in-java-passing-variable-no-of-arguments-in-java-method/"
---
After J2SE 5.0 java supports passing of variable arguments in a method that u don't need to fix the no of arguments at the time of defining the method.  
The compiler automatically converts the arguments in the form of an array so u can access all the arguments at runtime.

* * *

**Example:-**

```java
public class Varargs {

	public static void main(String[] args) {
		Varargs var=new Varargs();
		var.show("Ankit" , "Singh" , "Katiyar");
		var.show("facebook.com"","ankitkatiyar91");

	}

	public void show(String... arg)
	{
		for(int i=0;i<arg.length;i++)
		{
		System.out.println(arg[i]);
		}
	}
}
```

**&nbsp;**

* * *

&nbsp;

Note:- it works only&nbsp;for version J2SE 5.0 or above.

You can use any data type for variable arguments.

**Example:-**

_&nbsp;_

```java
public class Varargs {

	public static void main(String[] args) {

		Varargs var=new Varargs();
		var.show(1,2,3);
		var.show(9,7);
	 
	}
	public void show(int... arg)
	{
		for(int i=0;i<arg.length;i++)
		{
		System.out.println(arg[i]);
		}
	}
}
```

Restriction: For any method, it must be the last argument.

Explore more&nbsp;[here](https://docs.oracle.com/javase/1.5.0/docs/guide/language/varargs.html)

