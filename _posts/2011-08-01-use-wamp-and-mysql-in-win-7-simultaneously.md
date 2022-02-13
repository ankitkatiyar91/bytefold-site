---
layout: post
title: Use wamp and MySql in win 7 simultaneously
date: 2011-08-01 08:50:00.000000000 +05:30
type: post
parent_id: '0'
published: true
password: ''
status: publish
categories:
- Miscellaneous
tags: []
meta:
  blogger_blog: technohelpy.blogspot.com
  blogger_author: Ankit Katiyar
  blogger_permalink: "/2011/08/use-wamp-and-mysql-in-win-7.html"
  blogger_internal: "/feeds/2635046121517897773/posts/default/8288147280827228923"
author:
  login: ankitkatiyar91
  email: ankitkatiyar67@gmail.com
  display_name: Ankit Katiyar
  first_name: Ankit
  last_name: Katiyar
permalink: "/use-wamp-and-mysql-in-win-7-simultaneously/"
---
In win7 there is a problem in using wamp and MySql at the same time due to using the same port no. 3306 by the both &nbsp;wamp and MySql.&nbsp;So for resolving this problem change the port no. of MySql during installation (ex. 3307). Now you can use both at same time. If u want to use Mysql as DB in php then change the port no. in file "config.inc.php" .  
For windows the directory of this file is "C:wampappsphpmyadmin2.11.5"
[![]({{ site.baseurl }}/assets/images/2011/08/Capture.JPG)](http://1.bp.blogspot.com/-U5Dyg7lXNt0/TjbROx08CuI/AAAAAAAAAD8/mAGEEQE4glI/s1600/Capture.JPG)

&nbsp;In the image in second row give the new port no. of your DB.  
**_I hope it can help u.!!_**

