## Why we do testing?
- To improve the quality of code / product we perform the testing
- To improve the quality of code to find out all kind of visuals at early stage.
- Testing is the important part of software development life cycle.

### What is white box testing?
- The java developer knows about the internal functionality of the application, has all the technical knowledge of the application and when the developer do the testing that is called **white box texting**

### What is black box testing?
- Tester who works in an opperations they perform the **black box testing** because they don't have the `internal` knowledge of the application, how the functionality works. 
- They don't need to technical knowledge of the application to perform the testing.

## Techincal / Functional Testing
- We are going to text all the `unites` of the code.
- The smallest part of any application was `method / unit`.
- So here we perform the `unit` testing so we are going to use the `JUnit` framework.

## Process
- add the jar file called `spring-test-5.3.22`
- Go to build path and add the library and add JUnit 5
- create a Test Class in the test folder
- Create a method inside the class
- Add `@Test` annotation to mark the method test otherwise it won't be work. The annotation must be imported from `org.junit.jupiter.api.Test` package

```java
package com.accenture.lkm.test;

import org.junit.jupiter.api.Assertions;
import org.junit.jupiter.api.Test;
import org.springframework.context.ApplicationContext;
import org.springframework.context.support.ClassPathXmlApplicationContext;

import com.accenture.lkm.Employee;

public class TestEmployeeClass {

	@Test
	public void testEmployee() {
		System.out.println("*** testEmployee ***");
		ApplicationContext applicationContext = new ClassPathXmlApplicationContext(
				"com/accenture/lkm/resources/my_springbean.xml");
		Employee employee = applicationContext.getBean("employee",Employee.class);
		Assertions.assertNotNull(employee);
		((ClassPathXmlApplicationContext)applicationContext).close();
	}
	
	@Test
	public void testEmployeeAddress() {
		System.out.println("*** testEmployeeAddress ***");
		ApplicationContext applicationContext = new ClassPathXmlApplicationContext(
				"com/accenture/lkm/resources/my_springbean.xml");
		Employee employee = applicationContext.getBean("employee",Employee.class);
		Assertions.assertNotNull(employee.getAddress());
		((ClassPathXmlApplicationContext)applicationContext).close();
	}

	@Test
	public void testEmployeeSalary() {
		System.out.println("*** testEmployeeSalary ***");
		ApplicationContext applicationContext = new ClassPathXmlApplicationContext(
				"com/accenture/lkm/resources/my_springbean.xml");
		Employee employee = applicationContext.getBean("employee",Employee.class);
		Assertions.assertEquals(200000,employee.getSalary());
		((ClassPathXmlApplicationContext)applicationContext).close();
	}
}
// draw backs:
//1.For every methods Application context is reloaded, and support for context management 
// and caching is not there

```

- Here is the problem that we need to load the context again and again, loading the context for every single method
- Spring Context is loading for every methods which is a heavy operation and we don't have any support for `roleback`.
- If for testing purpose we need to update or delete anything then we are unable to do that.
- We can take the help of `spring based framework`. It provide support for testing in spring test model. There are some important annotation it has provided
  - @ContextConfiguration
  - @ExtendWith
 
### What does @ContextConfiguration(locations = "/com/accenture/lkm/resources/my_springbean.xml") annotation do in Spring test ?
- Load the complete context once for the file. Context will be available for entire class
- The `@ContextConfiguration` annotation in Spring test framework is used to specify the locations of the configuration files that define the application context for the test.
- In our example, locations = "/com/accenture/lkm/resources/my_springbean.xml" indicates that the configuration file my_springbean.xml is located in the com.accenture.lkm.resources package. This XML file likely contains bean definitions and other configuration settings required for the test to run.
- The Application context is loaded only once, and cached for all the test methods
- When the test runs, Spring loads the application context based on the configuration specified in the XML file, allowing the test to access and use beans defined within that context.
- Overall, @ContextConfiguration helps set up the environment for testing by providing the necessary configuration files to initialize the Spring application context.

### What does @ExtendWith(SpringExtension.class) this annotation do in Spring test ?
- The `@ExtendWith` annotation is used in `JUnit 5` to register extensions (also known as "test instance post-processors"). When you use `@ExtendWith(SpringExtension.class)`, you're essentially telling JUnit to enable Spring support for the test class.
- In the case of SpringExtension, it provides integration between JUnit 5 and the Spring TestContext Framework. This means that the test class can leverage various Spring testing features such as dependency injection (@Autowired), transaction management, and more.

```java
package com.accenture.lkm.test;

import org.junit.jupiter.api.Assertions;
import org.junit.jupiter.api.Test;
import org.junit.jupiter.api.extension.ExtendWith;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.test.context.ContextConfiguration;
import org.springframework.test.context.junit.jupiter.SpringExtension;

import com.accenture.lkm.Employee;

/*
 * The @ExtendWith annotation is used in JUnit 5 to register extensions (also known as "test instance post-processors"). 
 * When you use @ExtendWith(SpringExtension.class), you're essentially telling JUnit to enable Spring support for the test 
 * class.
 */
@ExtendWith(SpringExtension.class)

/*
 * The @ContextConfiguration annotation in Spring test framework is used to specify the locations of the configuration files 
 * that define the application context for the test.
 * The Application context is loaded only once, and cached for all the test methods 
 */
@ContextConfiguration(locations = "/com/accenture/lkm/resources/my_springbean.xml")
public class TestEmployeeClass {

	@Autowired
	private Employee employee;
	
	@Test
	public void testEmployee() {
		System.out.println("*** testEmployee ***");
		Assertions.assertNotNull(employee);
	}
	
	@Test
	public void testEmployeeAddress() {
		System.out.println("*** testEmployeeAddress ***");
		Assertions.assertNotNull(employee.getAddress());
	}

	@Test
	public void testEmployeeSalary() {
		System.out.println("*** testEmployeeSalary ***");
		Assertions.assertEquals(200000,employee.getSalary());
	}
}
// draw backs fixed:
//1.For every methods Application context is reloaded, and support for context management 
// and caching is not there

//https://docs.spring.io/spring-batch/trunk/reference/html/testing.html
//https://docs.spring.io/spring/docs/4.2.4.RELEASE/spring-framework-reference/htmlsingle/#testing-introduction
```













