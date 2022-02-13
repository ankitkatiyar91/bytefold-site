---
layout: post
title: System Properties in java
date: 2012-10-25 01:32:00.000000000 +05:30
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
  blogger_permalink: "/2012/10/system-properties-in-java.html"
  blogger_internal: "/feeds/2635046121517897773/posts/default/6294914253339918777"
  _edit_last: '1'
author: ankit_katiyar
permalink: "/system-properties-in-java/"
---
There are various system properties that we can use in our programs to perform some action or access some file.  
There are some&nbsp;commonly&nbsp;used properties.

&nbsp;

| Key | Meaning |
| "file.separator" | Character that separates components of a file path. This is "/" on UNIX and "" on Windows. |
| "java.class.path" | Path used to find directories and JAR archives containing class files. Elements of the class path are separated by a platform-specific character specified in the&nbsp;path.separator&nbsp;property. |
| "java.home" | Installation directory for Java Runtime Environment (JRE) |
| "java.vendor" | JRE vendor name |
| "java.vendor.url" | JRE vendor URL |
| "java.version" | JRE version number |
| "line.separator" | Sequence used by operating system to separate lines in text files |
| "os.arch" | Operating system architecture |
| "os.name" | Operating system name |
| "os.version" | Operating system version |
| "path.separator" | Path separator character used in&nbsp;java.class.path |
| "user.dir" | User working directory |
| "user.home" | User home directory |
| "user.name" | User account name |

**Example:-** These are following keys that can be used to&nbsp;find out&nbsp;the property.  
_"System"_ &nbsp;class has a properties Object associated with it that is used to&nbsp;find out&nbsp;those properties.

For More Refer:-  
[http://docs.oracle.com/javase/tutorial/essential/environment/sysprop.html](http://docs.oracle.com/javase/tutorial/essential/environment/sysprop.html)

**Here is a small snippet that can show all you available properties**

<!-- wp:enlighter/codeblock {"language":"java"} -->

```
public class SystemProperties {

	public static void main(String[] args) {
		System.getProperties().entrySet().forEach(e -> {
			System.out.println(e.getKey() + " : " + e.getValue());
		});
	}
}
```



