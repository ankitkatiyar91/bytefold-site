---
layout: post
title: JVM Shutdown Hook
date: 2019-01-26 21:55:05.000000000 +05:30
type: post
parent_id: '0'
published: true
password: ''
status: publish
categories:
- Java
tags:
- JVM Shutdown Hook
meta: {}
author: ankit_katiyar
permalink: "/jvm-shutdown-hook/"
excerpt: JVM shutdown hook are like finalize methods for your JVM. It does the same
  job for JVM that finalize method does for the objects of a class.
---


Recently I got a problem where I had to perform some task while the application is shutting down. I worked with <g class="gr_ gr_18 gr-alert gr_gramm gr_inline_cards gr_run_anim Grammar multiReplace" id="18" data-gr-id="18">lot's</g> of problems where I have to write object unload tasks (_ **finalize** _ method). But this was something completely new to me.





After some googling I found that JAVA supports JVM shutdown hooks. These hooks are some threads that are executed when JVM is shutting down. Java's Runtime class provide an **_addShutdownHook(Thread hook)&nbsp;_**method. This method will add this **_hook_&nbsp;** thread to be executed when all the tasks are done and JVM this about to shutdown/close.



<!-- wp:enlighter/codeblock {"language":"java"} -->

```
public class JVMShutdownHook
{

    public static void main(String[] args)
    {
        System.out.println("Main start... ");
        Runtime.getRuntime().addShutdownHook(new Thread()
        {
            @Override
            public void run()
            {
                System.out.println("Finally shut down hook called");
            }
        });

        System.out.println("Main end.... ");
    }

}
```



<!-- wp:heading {"level":4} -->

#### Output



<!-- wp:code -->

```
Main start... 
Main end....   
Finally shut down hook called
```

<!-- /wp:code -->



You can see from the output that code inside hook is executed at the end. This way you can handle events, free resources or something else like updating some flags/logs/process file when you application is shutting down. We often need these while running some jobs on server.



<!-- wp:separator -->

* * *
<!-- /wp:separator -->

<!-- wp:paragraph {"align":"right"} -->

A moment's insight is sometimes worth a life's experience.  
 --Oliver Wendell Holmes (1809-1894)



