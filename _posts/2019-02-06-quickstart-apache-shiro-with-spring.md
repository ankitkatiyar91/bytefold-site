---
layout: post
title: Quickstart Apache Shiro with Spring
date: 2019-02-06 16:36:09.000000000 +05:30
type: post
parent_id: '0'
published: true
password: ''
status: publish
categories:
- Java
tags:
- Apache Shiro
- Authentication
meta: {}
author:
  email: ankit@bytefold.com
  display_name: Ankit Katiyar
  first_name: Ankit
  last_name: Katiyar
permalink: "/quickstart-apache-shiro-with-spring/"
excerpt: 'Apache Shiro is an Authentication Authorization framework with support for
  cryptography and session management. You can quickly create a layer of security
  around your application. This is Quick Demo of the Apche Shiro with Spring framework
  in java. '
---

<p>Apache Shiro is an Authentication Authorization framework with support for cryptography and session management. You can quickly create a layer of security around your application.</p>
<p></p>

<p>I used this framework with couple of project and now it's my first go for authentication and authorization mechanism around any application, even over Spring Security. A lot of may like Spring Security because it comes with your spring and lot of community support and documentations.</p>
<p></p>

<p>You need to create a realm that provides all the logic of Authenticating a User and Authorizing it for any access.  Below is a simple realm class. (Not doing any verification, just for demonstration)</p>
<p></p>

<p><strong>Shiro&nbsp;Dependency</strong></p>
<p></p>
<p><!-- wp:enlighter/codeblock {"language":"xml"} --></p>
<pre class="highlight" data-enlighter-language="xml" data-enlighter-theme="" data-enlighter-highlight="" data-enlighter-linenumbers="" data-enlighter-lineoffset="" data-enlighter-title="" data-enlighter-group="">&lt;!-- shiro dependency -->
		&lt;dependency>
			&lt;groupId>org.apache.shiro&lt;/groupId>
			&lt;artifactId>shiro-all&lt;/artifactId>
			&lt;version>1.1.0&lt;/version>
		&lt;/dependency></pre>


<p><strong>Realm</strong></p>
<p></p>
<p><!-- wp:enlighter/codeblock {"language":"java"} --></p>
<pre class="highlight" data-enlighter-language="java" data-enlighter-theme="" data-enlighter-highlight="" data-enlighter-linenumbers="" data-enlighter-lineoffset="" data-enlighter-title="" data-enlighter-group="">import org.apache.shiro.authc.AuthenticationException;
import org.apache.shiro.authc.AuthenticationInfo;
import org.apache.shiro.authc.AuthenticationToken;
import org.apache.shiro.authc.SimpleAuthenticationInfo;
import org.apache.shiro.authc.UsernamePasswordToken;
import org.apache.shiro.authc.credential.CredentialsMatcher;
import org.apache.shiro.authz.AuthorizationInfo;
import org.apache.shiro.authz.SimpleAuthorizationInfo;
import org.apache.shiro.realm.AuthorizingRealm;
import org.apache.shiro.subject.PrincipalCollection;

public class MyRealm extends AuthorizingRealm {

	public MyRealm() {
		super();
		setCredentialsMatcher(new CredentialsMatcher() {
			
			@Override
			public boolean doCredentialsMatch(AuthenticationToken arg0,
					AuthenticationInfo arg1) {
				System.out
						.println("MyRealm.MyRealm().new CredentialsMatcher() {...}.doCredentialsMatch()");
				return true;
			}
		});
		
	}
	
	@Override
	protected AuthorizationInfo doGetAuthorizationInfo(PrincipalCollection arg0) {
		System.out.println("MyRealm.doGetAuthorizationInfo()");
		AuthorizationInfo info=new SimpleAuthorizationInfo();
		return info;
	}

	

	@Override
	protected AuthenticationInfo doGetAuthenticationInfo(
			AuthenticationToken arg0) throws AuthenticationException {
		System.out.println("MyRealm.doGetAuthenticationInfo()");
		UsernamePasswordToken token=(UsernamePasswordToken) arg0;
		
		AuthenticationInfo info=new SimpleAuthenticationInfo(1,token.getCredentials(), getName());
		return info;
	}

}</pre>


<p><strong>Spring Configuration for Shiro</strong></p>
<p></p>
<p><!-- wp:enlighter/codeblock {"language":"xml"} --></p>
<pre class="highlight" data-enlighter-language="xml" data-enlighter-theme="" data-enlighter-highlight="" data-enlighter-linenumbers="" data-enlighter-lineoffset="" data-enlighter-title="" data-enlighter-group="">&lt;?xml version="1.0" encoding="UTF-8"?>
&lt;beans xmlns="http://www.springframework.org/schema/beans"
	xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">


&lt;!-- Define the realm you want to use to connect to your back-end security datasource: -->
&lt;bean id="myRealm" class="com.realm.MyRealm">&lt;/bean>

&lt;bean id="securityManager" class="org.apache.shiro.mgt.DefaultSecurityManager">
    &lt;!-- Single realm app.  If you have multiple realms, use the 'realms' property instead. -->
    &lt;property name="realm" ref="myRealm"/>
    &lt;property name="sessionManager.sessionListeners">
    	&lt;list>
    	&lt;ref bean="mySessionListener" />
    	&lt;/list>
    &lt;/property>
&lt;/bean>
&lt;bean id="lifecycleBeanPostProcessor" class="org.apache.shiro.spring.LifecycleBeanPostProcessor"/>

&lt;!-- For simplest integration, so that all SecurityUtils.* methods work in all cases, -->
&lt;!-- make the securityManager bean a static singleton.  DO NOT do this in web         -->
&lt;!-- applications - see the 'Web Applications' section below instead.                 -->
&lt;bean class="org.springframework.beans.factory.config.MethodInvokingFactoryBean">
    &lt;property name="staticMethod" value="org.apache.shiro.SecurityUtils.setSecurityManager"/>
    &lt;property name="arguments" ref="securityManager"/>
&lt;/bean>

&lt;bean id="mySessionListener" class="com.listner.MySessionListener" >&lt;/bean>
&lt;/beans>
</pre>
<p><strong>Demo</strong></p>
<pre class="highlight" data-enlighter-language="java" data-enlighter-theme="" data-enlighter-highlight="" data-enlighter-linenumbers="" data-enlighter-lineoffset="" data-enlighter-title="" data-enlighter-group="">
public class ShiroTest {
	public static void main(String[] args) {
		AbstractApplicationContext context=new ClassPathXmlApplicationContext("spring.xml");
		context.registerShutdownHook();
		org.apache.shiro.subject.Subject subject=SecurityUtils.getSubject();
		AuthenticationToken token=new UsernamePasswordToken("username", "password");
		System.out.println("Login a user--");
 		subject.login(token);
 		System.out.println("User logged in---"); subject.logout(); System.out.println("User logged out"); 
    } 
}
</pre>



A fully functional demo available on GitHub [https://github.com/ankitkatiyar91/java-framework-examples/tree/master/spring-examples/SpringShiro](https://github.com/ankitkatiyar91/java-framework-examples/tree/master/spring-examples/SpringShiro)





Check CMS application that usages Shiro for security [https://github.com/ankitkatiyar91/cms-java](https://github.com/ankitkatiyar91/cms-java)



