---
layout: post
title: Enable/Disable profile specific spring security debug flag
date: 2020-05-02 21:04:02.000000000 +05:30
type: post
parent_id: '0'
published: true
password: ''
status: publish
categories:
- Miscellaneous
tags:
- Spring Security Debugging
- Spring Security Logging
meta:
  _edit_last: '1'
author: ankit_katiyar
permalink: "/enable-disable-profile-specific-spring-security-debug-flag/"
---
Spring security comes with a handy feature to enable <em>debug</em>flag to see a nice debug log as shown below to see what is happening with your application.

```
2020-04-21 21:19:16.829  INFO 12332 ---
[nio-8080-exec-5] Spring Security Debugger : \*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\* Request received for GET '/images/favicon.ico': org.apache.catalina.connector.RequestFacade@20617566 servletPath:/images/favicon.ico pathInfo:null headers: host: localhost:8080 connection: keep-alive pragma: no-cache cache-control: no-cache user-agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/80.0.3987.163 Safari/537.36 sec-fetch-dest: image accept: image/webp,image/apng,image/\*,\*/\*;q=0.8 sec-fetch-site: same-origin sec-fetch-mode: no-cors referer: http://localhost:8080/ accept-encoding: gzip, deflate, br accept-language: en-US,en;q=0.9,hi;q=0.8 cookie: ext\_name=jaehkpjddfdgiiefcnhahapilbejohhj; JSESSIONID=B31522BDCCAA126406166C25DBE9DE96; sub\_pop=1587484276805 Security filter chain: [] empty (bypassed by security='none') \*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*
```



You can control this feature from your bean configuration code



```java
@Configuration
@EnableWebSecurity(debug = true)
public class SecurityConfig extends WebSecurityConfigurerAdapter
```


Still, you may want to control it from _application.properties_ files for some profiles. Like I want to keep it on for dev only.




```
org.springframework.security.config.annotation.web.builders.WebSecurity.debugEnabled=true
```



