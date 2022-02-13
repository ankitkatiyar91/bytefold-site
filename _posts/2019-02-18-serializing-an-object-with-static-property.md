---
layout: post
title: Serializing an object with static property
date: 2019-02-18 16:02:58.000000000 +05:30
type: post
parent_id: '0'
published: true
password: ''
status: publish
categories:
- Java
tags:
- Serialization
meta: {}
author: ankit_katiyar
permalink: "/serializing-an-object-with-static-property/"
---


Serialization is often used to convert the <g class="gr_ gr_4 gr-alert gr_spell gr_inline_cards gr_run_anim ContextualSpelling ins-del multiReplace" id="4" data-gr-id="4">in memory</g> objects to stream and vice-versa. But **_static_** members are not part the object states so they are not serialized in this process.





You need to serialize an object that has some static properties. The answer is, you need to control the process of serialization and De-Serialization for those objects. To do this in java you need to implement the interface **_Externalizable._&nbsp;** This is similar to the Serializable interface but with more control over the process of the serialization.





_java.io.Externalizable_ interface provides two methods _writeExternal()_ and _readExternal()_. These methods are invoked whenever an object is being serialized or De-serialized. Syntax of both are shown below.



<!-- wp:enlighter/codeblock {"language":"java"} -->

```
void writeExternal(ObjectOutput out) throws IOException;
```



<!-- wp:enlighter/codeblock {"language":"java"} -->

```
void readExternal(ObjectInput in) throws IOException, ClassNotFoundException;
```





For demonstration, I have created a class _ExternalizableEmployee_ which has only two Strong fields, name and a static field company.



<!-- wp:heading {"level":3} -->

### Approach





As we know that serialization will not save a property marked as static. we can create a proxy object that has the same properties but none of them is marked as static. So when you save the proxy object it will save all the properties and vice-versa when reading. In this example, I created a static _Employee_ class with the _ExternalizableEmployee_ class. Within because we do not need this outside the scope of E_xternalizableEmployee_ class_._



<!-- wp:heading {"level":3} -->

### E_xternalizableEmployee&nbsp;_class



<!-- wp:enlighter/codeblock {"language":"java"} -->

```
import java.io.Externalizable;
import java.io.IOException;
import java.io.ObjectInput;
import java.io.ObjectOutput;
import java.io.Serializable;

public class ExternalizableEmployee implements Externalizable {

	private String name;
	private static String company;

	// Inner Proxy class for Serialization.
	private static class Employee implements Serializable {
		private static final long serialVersionUID = 610993458650534361L;

		String name;
		String company;

		public Employee(String name, String company) {
			super();
			this.name = name;
			this.company = company;
		}

		public String getName() {
			return name;
		}

		public String getCompany() {
			return company;
		}

	}

	@Override
	public void writeExternal(ObjectOutput out) throws IOException {
		/*
		 * Write the object of proxy class Employee with the values of
		 * ExternalizableEmployee
		 */
		Employee emp = new Employee(name, company);
		out.writeObject(emp);
	}

	@Override
	public void readExternal(ObjectInput in) throws IOException, ClassNotFoundException {
		/* Read the proxy object and initialize ExternalizableEmployee */
		Employee employee = (Employee) in.readObject();
		this.name = employee.getName();
		company = employee.getCompany();
	}

	public String getName() {
		return name;
	}

	public void setName(String name) {
		this.name = name;
	}

	public static String getCompany() {
		return company;
	}

	public static void setCompany(String company) {
		ExternalizableEmployee.company = company;
	}

}
```



<!-- wp:heading {"level":3} -->

### Write _ExternalizableEmployee_ object to File



<!-- wp:enlighter/codeblock {"language":"java"} -->

```
import java.io.FileOutputStream;
import java.io.IOException;
import java.io.ObjectOutputStream;

public class WriteExternalizableEmployeeTest {
	public static void main(String[] args) {
		try (ObjectOutputStream objectOutputStream = new ObjectOutputStream(new FileOutputStream("employee.data"))) {
			// create an object of ExternalizableEmployee
			ExternalizableEmployee emp=new ExternalizableEmployee();
			emp.setName("Test");
			ExternalizableEmployee.setCompany("ByteFold");
			
			// write object to file
			objectOutputStream.writeObject(emp);
		} catch (IOException e) {
			e.printStackTrace();
		}
	}
}
```



<!-- wp:heading {"level":3} -->

### Read _ExternalizableEmployee_ object



<!-- wp:enlighter/codeblock {"language":"java"} -->

```
import java.io.FileInputStream;
import java.io.IOException;
import java.io.ObjectInputStream;

public class ReadExternalizableEmployeeTest {
	public static void main(String[] args) {
		try (ObjectInputStream objectOutputStream = new ObjectInputStream(new FileInputStream("employee.data"))) {
			// Read an object of ExternalizableEmployee
			ExternalizableEmployee employee = (ExternalizableEmployee) objectOutputStream.readObject();

			System.out.println(employee.getName());
			System.out.println(employee.getCompany());
		} catch (IOException | ClassNotFoundException e) {
			e.printStackTrace();
		}
	}
}
```





Find the complete code at [GitHub](https://github.com/ankitkatiyar91/java-practise/tree/master/learning/src/main/java/IO/serialization)





More about [Serialization](https://dzone.com/articles/serialization-amp-de-serialization-in-java)



