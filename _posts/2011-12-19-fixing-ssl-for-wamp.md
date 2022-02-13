---
layout: post
title: FIXING SSL FOR WAMP
date: 2011-12-19 08:15:00.000000000 +05:30
type: post
parent_id: '0'
published: true
password: ''
status: publish
categories:
- Miscellaneous
tags: []
meta:
  blogger_blog: bytefold.com
  blogger_author: Ankit Katiyar
  blogger_permalink: "/2011/12/fixing-ssl-for-wamp.html"
  blogger_internal: "/feeds/2635046121517897773/posts/default/776750940913000981"
author: ankit_katiyar
permalink: "/fixing-ssl-for-wamp/"
---
ERROR:  
There was a problem opening a secure connection to Google. This is what went wrong:  
  
  
Unable to find the socket transport "ssl" - did you forget to enable it when you configured PHP? (1)  
  
  
Recently I had to fix a bug/oversight in the WAMP install that prevented ssl from being a registered stream socket transport, it turns out there are quite a lot of requests for help on this subject out there but few or no answers. I stumbled into this problem when I tried to setup SSL encrypted email by way of google’s SMTP servers and if by putting my solution here I help just one person then it is worth it.  
  
  
The common error message that you will get when trying to do an SSL transport on a default WAMP install is: “Unable to find the socket transport “ssl” – did you forget to enable it when you configured PHP?”  
  
  
The solution it turns out is quite obvious, but maybe not so much when you are sleep deprived and multitasking, what you need to do is:  
  
  
&nbsp; &nbsp; 1. Stop the Server  
&nbsp; &nbsp; 3. Edit the&nbsp;C:wampbinapacheapache2.2.8binphp.ini and uncomment “;extension=php\_openssl.dll” (remove the semicolon at the start of the line)  
&nbsp; &nbsp; 4.Start the Apache service  
  
  
If you now check your phpinfo() page you will see Registered Stream Socket Transports: tcp, udp, ssl, sslv3, sslv2, tls and everything will now be working.  
After updating to the latest version of xampp (1.7.7) it appears the problem still exists, the php.ini file to edit now exists in the ../xampp/php directory, you need to add “extension=php\_openssl.dll” in there. No other steps are required (apart from the obvious restart of apache).
