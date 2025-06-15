## What does @DirtiesContext(methodMode = MethodMode.BEFORE_METHOD) annotation do in Spring test?
- For the perticular method the `Context` will load again, beacause if somehow the data or value changed for the Context then we don't get the correct result.
- In Spring Framework's testing support, the `@DirtiesContext` annotation is used to indicate that the ApplicationContext associated with a test is dirty and should be recreated before running a specific test method or all test methods in a test class.
- When methodMode is set to BEFORE_METHOD, it indicates that the context should be marked as dirty and recreated before the execution of each test method.

### Here's a breakdown:
- `@DirtiesContext:` Marks the context as dirty, indicating that it should be refreshed.
- `methodMode = MethodMode.BEFORE_METHOD:` Specifies that the context should be dirtied and recreated before each test method (BEFORE_METHOD mode).
- This annotation is useful in scenarios where the state of the ApplicationContext is modified by a test method in a way that subsequent test methods rely on the original state. By marking the context as dirty before each method, you ensure that each test method starts with a fresh ApplicationContext, preventing interference between tests due to shared context state modifications.

```java
package com.accenture.lkm.test;

import org.junit.jupiter.api.Assertions;
import org.junit.jupiter.api.Test;
import org.junit.jupiter.api.extension.ExtendWith;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.test.annotation.DirtiesContext;
import org.springframework.test.annotation.DirtiesContext.MethodMode;
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
	/*
	 * In Spring Framework's testing support, the @DirtiesContext annotation is used to indicate that the ApplicationContext 
	 * associated with a test is dirty and should be recreated before running a specific test method. 
	 * When methodMode is set to BEFORE_METHOD, it indicates that the context should be marked as dirty and recreated before 
	 * the execution of the test method.
	 */
	@DirtiesContext(methodMode = MethodMode.BEFORE_METHOD)
	public void testEmployeeSalary() {
		System.out.println("*** testEmployeeSalary ***");
		Assertions.assertEquals(200000,employee.getSalary());
	}
}

/*
methodMode
@DirtiesContext(methodMode = MethodMode.BEFORE_METHOD)
@DirtiesContext(methodMode = MethodMode.AFTER_METHOD)

classMode
@DirtiesContext(classMode = ClassMode.BEFORE_CLASS)
@DirtiesContext(classMode = ClassMode.AFTER_CLASS)
*/
```

## Java Configuration
```java
package com.accenture.lkm.test;

import org.junit.jupiter.api.Assertions;
import org.junit.jupiter.api.Test;
import org.junit.jupiter.api.extension.ExtendWith;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.test.context.ContextConfiguration;
import org.springframework.test.context.junit.jupiter.SpringExtension;

import com.accenture.lkm.Employee;
import com.accenture.lkm.resources.MyConfiguration;

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
@ContextConfiguration(classes = MyConfiguration.class)
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

```

## What does @ActiveProfiles({"test"}) annotation do in Spring test ?
- In Spring Framework's testing support, `@ActiveProfiles({"test"})` is an annotation used to activate one or more profiles when running tests. Profiles in Spring allow you to define sets of configuration options that can be activated under certain conditions.
- When you annotate a test **class or method** with `@ActiveProfiles({"test"})`, you're essentially telling Spring to activate the "test" profile for that particular test. This can be useful when you have different sets of configurations for different environments (e.g., development, testing, production) and you want to ensure that the correct configuration is loaded during testing.
- For example, if you have a test profile configured with specific properties or beans that are meant to be used only during testing, using @ActiveProfiles({"test"}) ensures that those configurations are applied when running the test annotated with it.

```java
package com.accenture.lkm;

import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.beans.factory.annotation.Value;
import org.springframework.context.annotation.Profile;
import org.springframework.stereotype.Component;

@Component("employee")
@Profile("test")
public class Employee {

	@Value("1001")
	private Integer employeeId;
	@Value("JAS")
	private String employeeName;
	@Value("56000.0")
	private Double salary;
	
	@Autowired
	private Address address;
	
	public Employee() {
		System.out.println("From Constructor of Employee class....");
	}

	public Address getAddress() {
		return address;
	}

	public void setAddress(Address address) {
		this.address = address;
	}

	public Integer getEmployeeId() {
		return employeeId;
	}

	public void setEmployeeId(Integer employeeId) {
		this.employeeId = employeeId;
	}

	public Double getSalary() {
		return salary;
	}

	public void setSalary(Double salary) {
		this.salary = salary;
	}

	public String getEmployeeName() {
		return employeeName;
	}

	public void setEmployeeName(String employeeName) {
		this.employeeName = employeeName;
	}

	public void display() {
		System.out.println("\nEmployee Details are:");
		System.out.println("Employee ID:" + this.employeeId);
		System.out.println("Employee Name:" + this.employeeName);
		System.out.println("Employee Salary:" + this.salary);
		System.out.println("\nAddress line1:" + this.address.getAddressLine1());
		System.out.println("Address line2:" + this.address.getAddressLine2());
	}
}
```

```java
package com.accenture.lkm.test;

import org.junit.jupiter.api.Assertions;
import org.junit.jupiter.api.Test;
import org.junit.jupiter.api.extension.ExtendWith;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.test.context.ActiveProfiles;
import org.springframework.test.context.ContextConfiguration;
import org.springframework.test.context.junit.jupiter.SpringExtension;

import com.accenture.lkm.Employee;
import com.accenture.lkm.resources.MyConfiguration;

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
@ContextConfiguration(classes = MyConfiguration.class)

/*
 * In Spring Framework's testing support, @ActiveProfiles({"test"}) is an annotation used to 
 * activate one or more profiles when running tests. 
 */
@ActiveProfiles({"test"})
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
```
