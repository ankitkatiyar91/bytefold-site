---
layout: post
title: Starting Apache Velocity with struts
date: 2014-03-10 08:00:00.000000000 +05:30
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
  blogger_permalink: "/2014/03/starting-apache-velocity-with-struts.html"
  blogger_internal: "/feeds/2635046121517897773/posts/default/7349269182651263975"
  _thumbnail_id: '334'
  _edit_last: '1'
  _yoast_wpseo_content_score: '30'
  _yoast_wpseo_primary_category: '56'
author:
  email: ankit@bytefold.com
  display_name: Ankit Katiyar
  first_name: Ankit
  last_name: Katiyar
permalink: "/starting-apache-velocity-with-struts/"
---
 **What is Velocity?**

**Ans:&nbsp;** Velocity is a Java-based template engine. It permits anyone to use a simple yet powerful template language to reference objects defined in Java code.

When Velocity is used for web development, Web designers can work in parallel with Java programmers to develop web sites according to the Model-View-Controller (MVC) model, meaning that web page designers can focus solely on creating a site that looks good, and programmers can focus solely on writing top-notch code. Velocity separates Java code from the web pages, making the web site more maintainable over its lifespan and providing a viable alternative to&nbsp;[Java Server Pages](http://java.sun.com/products/jsp/)&nbsp;(JSPs) or&nbsp;[PHP](http://www.php.net/).

Check ([http://velocity.apache.org/](http://velocity.apache.org/))&nbsp;for details.

For first App you need jars required for velocity.

Find jars at **([http://velocity.apache.org/download.cgi](http://velocity.apache.org/download.cgi))**

<!-- [if !supportLists]-->&nbsp;&nbsp;1.&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; <!--[endif]-->Create a Dynamic web application.

<!-- [if !supportLists]-->&nbsp;&nbsp;2.&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; <!--[endif]-->Put all the required jars in /WEB-INF/lib&nbsp; directory of your application

antlr-2.7.5.jar

avalon-logkit-2.1.jar

commons-collections-3.2.1.jar

commons-lang-2.4.jar

commons-logging-1.1.jar

jdom-1.0.jar

log4j-1.2.12.jar

maven-ant-tasks-2.0.9.jar

oro-2.0.8.jar

servletapi-2.3.jar

werken-xpath-0.9.4.jar

Note: you can find all these jars in lib folder of downloaded zip of Velocity (Ex. velocity-1.7.zip).

<!-- [if !supportLists]-->&nbsp;&nbsp;3.&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; <!--[endif]-->Map a servlet in your web.xml

```xml
<servlet>
    <servlet-name>velocity</servlet-name>
    <servlet-class>org.apache.velocity.tools.view.VelocityViewServlet</servlet-class>
  </servlet>
  <servlet-mapping>
       <servlet-name>velocity</servlet-name>
       <url-pattern>*.vm</url-pattern>
  </servlet-mapping>
```

&nbsp;

&nbsp;

<!-- [if !supportLists]-->&nbsp;&nbsp;4.&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; <!--[endif]-->In case of Struts 2.x map struts filter

```xml
<filter>
  <filter-name>struts2</filter-name>
  <filter-class>org.apache.struts2.dispatcher.ng.filter.StrutsPrepareAndExecuteFilter</filter-class>
  </filter>
  <filter-mapping>
  <filter-name>struts2</filter-name>
  <url-pattern>*</url-pattern> 
</filter-mapping>
```

&nbsp;

<!-- [if !supportLists]-->&nbsp;&nbsp;5.&nbsp; <!--[endif]-->Create a UserAction

&nbsp;

```java
package com.ankit.web;

import com.opensymphony.xwork2.ActionSupport;

public class UserLogin extends ActionSupport {

       private String name;
       private String email;
       private int random;
      
       public String getName() {
              return name;
       }

       public void setName(String name) {
              this.name = name;
       }

       public String getEmail() {
              return email;
       }

       public void setEmail(String email) {
              this.email = email;
       }

       public int getRandom() {
              return random;
       }

       public void setRandom(int random) {
              this.random = random;
       }

       public  String login() {
             
              random=(int)( Math.random()*10);
              System.out.println("Name->"+name+" random->"+random);
              return SUCCESS;
       }
}
```

&nbsp;

&nbsp;

&nbsp;

&nbsp;

&nbsp;&nbsp;6.&nbsp; <!--[endif]-->Struts.xml

```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPEstruts PUBLIC "-//Apache Software Foundation//DTD Struts Configuration 2.1//EN" "http://struts.apache.org/dtds/struts-2.1.dtd">
<struts>
<packagename="default" extends="struts-default">
       <action name="login"class="com.ankit.web.UserLogin" method="login" >
              <result>index.vm</result>
       </action>
</package>
</struts>
```

&nbsp;

&nbsp;

<!-- [if !supportLists]-->&nbsp;&nbsp;7.&nbsp; <!--[endif]-->index.jsp

```xml
<%@page language="java"contentType="text/html; charset=ISO-8859-1"
    pageEncoding="ISO-8859-1"%>
    <%@taglib uri="/struts-tags"  prefix="s"%>
    <%@taglib uri="http://velocity.apache.org/velocity-view"  prefix="velocity"%>
<!DOCTYPEhtml PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN" "http://www.w3.org/TR/html4/loose.dtd">
<html>
<head>
<meta http-equiv="Content-Type" content="text/html; charset=ISO-8859-1">
<title>Insert title here</title>
</head>
<body>
<s:formaction="login" >
<s:textfield  name="name"label="Name" labelSeparator=":-"></s:textfield>
<s:textfield  name="Email"label="Email" labelSeparator=":-"></s:textfield>
<s:submit></s:submit>
</s:form>
</body>
</html>
```

&nbsp;

<!-- [if !supportLists]-->&nbsp;&nbsp;8.&nbsp; <!--[endif]-->index.vm

```bash
#set($a="Welcome")
$a
<b>$name</b> <br>
Your Email is <b>$email</b>
<br>
Generated Random Number is in RED
#set($i=10)
#foreach($i in [1..10])
<b
#if($i==$random)
style="color:red;"
#else
style="color:green;"
#end >$i</b>&nbsp;
#set($i=$i+1)
#end
<br>Check if user is  Admin
#if($name=="Admin")
<b>User is Admin</b>
#end
```

&nbsp;

&nbsp;

**Output:-&nbsp;**

**Page1**

&nbsp; &nbsp;[![]({{ site.baseurl }}/assets/images/2014/03/velocity-output1.jpg)](http://bytefold.com/wp-content/uploads/2014/03/velocity-output1.jpg)

&nbsp; &nbsp; &nbsp; &nbsp; Page2

&nbsp;

&nbsp;[![]({{ site.baseurl }}/assets/images/2014/03/velocity-output2.jpg)](http://bytefold.com/wp-content/uploads/2014/03/velocity-output2.jpg)

**Look at [GitHub](https://github.com/ankitkatiyar91/bytefold/tree/master/VelocityWithStrutsTest)**

