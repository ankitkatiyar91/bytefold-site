---
layout: post
title: Jagged Array in Java
date: 2012-07-13 04:14:00.000000000 +05:30
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
  blogger_permalink: "/2012/07/jagged-array-in-java.html"
  blogger_internal: "/feeds/2635046121517897773/posts/default/4412194843321078844"
  _edit_last: '1'
  _yoast_wpseo_content_score: '60'
  _yoast_wpseo_primary_category: '56'
  _yoast_wpseo_focuskw_text_input: Jagged Arrays
  _yoast_wpseo_focuskw: Jagged Arrays
  _yoast_wpseo_linkdex: '32'
author:
  email: ankit@bytefold.com
  display_name: Ankit Katiyar
  first_name: Ankit
  last_name: Katiyar
permalink: "/jagged-array-in-java/"
---
Java support Jagged Arrays. You can have a different&nbsp;length for each column do not need to have same no of the column for all rows.

* * *

**Example:-**

```
public class JaggedArray {
public static void main(String ar[])
{
int[][] a=new int[2][];
a[0]=new int[2];
a[1]=new int[4];
a[0][0]=1;
a[0][1]=12;
a[1][0]=10;
a[1][1]=3;
a[1][2]=12;
a[1][3]=13;
System.out.println("length-"+a.length);
for (int i = 0; i < a.length; i++) {
for (int k = 0; k < a[i].length; k++) {
System.out.print(a[i][k]+" , ");
}
System.out.println("");
} 
}
}
```

* * *

**Output:-**

```
length-2
1 , 12 , 
10 , 3 , 12 , 13 ,
```

Curious why this happens? In Java, every 2D or 3D arrays is just an array having each element as an array. So if you create a 2D array with length 2,3 then it will create one array with length 2 and that array will have 2 different arrays with length 3 as it's element.

Wanna know more about Arrays in java? Go [here](https://docs.oracle.com/javase/tutorial/java/nutsandbolts/arrays.html)
