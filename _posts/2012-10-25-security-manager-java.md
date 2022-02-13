---
layout: post
title: Security Manager in Java
date: 2012-10-25 01:53:00.000000000 +05:30
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
  blogger_permalink: "/2012/10/the-security-manager.html"
  blogger_internal: "/feeds/2635046121517897773/posts/default/2640024370943280853"
  _edit_last: '1'
  _wp_old_slug: the-security-manager
author:
  email: ankit@bytefold.com
  display_name: Ankit Katiyar
  first_name: Ankit
  last_name: Katiyar
permalink: "/security-manager-java/"
---
A security manager is an object that is used to check whether the operation we want to perform is not&nbsp;violating the security policy of the operating system.  
Sometimes we want to perform any operation and get an exception

```
Exception in thread "AWT-EventQueue-1" java.security.AccessControlException: access denied (java.io.FilePermission characteroutput.txt write)
        at java.security.AccessControlContext.checkPermission(AccessControlContext.java:323)
        at java.security.AccessController.checkPermission(AccessController.java:546)
        at java.lang.SecurityManager.checkPermission(SecurityManager.java:532)
        at java.lang.SecurityManager.checkWrite(SecurityManager.java:962)
        at java.io.FileOutputStream.<init>(FileOutputStream.java:169)
        at java.io.FileOutputStream.<init>(FileOutputStream.java:70)
        at java.io.FileWriter.<init>(FileWriter.java:46)
```

To Prevent this exception we can use SecurityManager class to check for the permission before performing an operation.

The security manager is an object of the type[` SecurityManager`](http://docs.oracle.com/javase/7/docs/api/java/lang/SecurityManager.html) to obtain a reference to this object, invoke.`System.getSecurityManager`

```
SecurityManager appsm = System.getSecurityManager();
```

If there is no security manager, this method returns.`null`

The SecurityManager class defines many other methods used to verify other kinds of operations. For example,&nbsp;`SecurityManager.checkAccess`&nbsp;verifies thread accesses, and verifies`SecurityManager.checkPropertyAccess` access to the specified property. Each operation or group of operations has its own`checkXXX()`&nbsp;method.  
In addition, the set of methods`checkXXX()` represents the set of operations that are already subject to the protection of the security manager. Typically, an application does not have to directly invoke any methods`checkXXX()`.

