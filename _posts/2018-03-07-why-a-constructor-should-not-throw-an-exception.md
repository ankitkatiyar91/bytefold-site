---
layout: post
title: Why a constructor should not throw an exception?
date: 2018-03-07 21:31:00.000000000 +05:30
type: post
parent_id: '0'
published: true
password: ''
status: publish
categories:
- Java
tags: []
meta:
  _yoast_wpseo_focuskw: constructor should not throw exception
  _yoast_wpseo_linkdex: '52'
  _edit_last: '1'
  _yoast_wpseo_content_score: '30'
  _yoast_wpseo_primary_category: '56'
  _yoast_wpseo_focuskw_text_input: constructor should not throw exception
author:
  email: ankit@bytefold.com
  display_name: Ankit Katiyar
  first_name: Ankit
  last_name: Katiyar
permalink: "/why-a-constructor-should-not-throw-an-exception/"
---
Just imagine if somebody asks you to do some task and when you have done 80% of it they come and ask to stop. Because you have not met preconditions for it. How will you feel about it? Something similar happens when you throw an exception from a constructor.

### What are constructors? (If you already understand this, let’s revisit or just skip to next para)

A constructor is a special method that is responsible for initialization of the variable of a class and returns a newly created instance of that class. We do not define a return type of constructs as they always return an object of the class they are defined in. Their names will always be same as class name.

&nbsp;

```java
class Car {

	private int cylinder;

	public Car(int cylinder) {
		super();
		this.cylinder = cylinder;
	}

}
```

Next thing that we need to understand for this is constructor chaining.

## Constructor Chaining

If you have never heard this thing before then let me introduce to constructor chaining.

In Java, A class can extend exactly one class and if it does not extend any class then the compiler will make some change and your class will extend java.lang.Object class.

In your class you define some constructors and these constructors will either have their first statement as a this(\<Parameter if any\>) or a super(\<Parameter if any\>). Most of the time you don’t give these statements. In that case, the compiler makes some changes and add super() to your constructor. Now you wonder why? Well, the answer is, when your object will be created then there are some tasks that need to be performed before your object starts initializing and these steps are defined in Object class. If you already extend some class then your parent class may need to do some tasks or initialize it’s variable to get going with your child.

Here we can say that _"A child cannot be born before its father".&nbsp;_Your parent

After all, that’s why we have inheritance so that you can reuse the functionality of your parent and for that parent needs to be initialized before use.

This whole concept of calling one constructor from another is called constructor chaining.

```java
class Car {

	private int cylinder;

	public Car(int cylinder) {
		super();
		this.cylinder = cylinder;
	}

}

class Audi extends Car {

	public Audi(int cylinder) {
		// calling super class constructor as it needs to initialize cylinder of parent
		super(cylinder);
	}

	public Audi() {
		// calling above constructor 
		this(4); // here we can also have a call to super, but not both
	}

}
```

Now just think if your constructor is throwing an exception at the third link or first maybe while reading from some CSV what will happen?

Your construction will break but before that, contractors of all its parents (including Object class) will be called and all those efforts will be a waste of time and memory.

```
class Car {

	private int cylinder;

	public Car(int cylinder) {
		super();
		this.cylinder = cylinder;
	}

}

class Audi extends Car {

	public Audi(int cylinder) {
		// calling super class constructor as it needs to initialize cylinder of parent
		super(cylinder);
	}

	public Audi() {
		// calling above constructor 
		this(4); // here we can also have a call to super, but not both
		
		// here I got some weird situation and want to throw an exception
		throw new IllegalArgumentException("You input is not correct");
		// OOPS! we have already done bunch of tasks before throwing from here.
		// Also don't forgot the memory allocations and you may have a long hierarchy of classes
	}

}
```

One possible solution can be using a method that does the tasks that may throw an exception and then call the constructor to you avoid unnecessary steps. Yes, that is using a factory design pattern.

Now you know why it is always disgraced to throw an exception from a constructor.

