## What is JSR330 in Spring?
- JSR(Java Specification Request) 330, also known as the "Dependency Injection for Java" specification, is a standard developed under the `Java Community Process (JCP)`.
- JSR is like an API which is provided to the Java developer to use.
- To use this we need to add those jar in our system
- It defines annotations for dependency injection in Java applications. These annotations provide a standardized way to declare dependencies and inject them into `Java classes`, `promoting more modular` and `flexible code`.
- In the context of Spring Framework, JSR 330 annotations are supported for dependency injection alongside Spring's own annotations.
- Spring provides integration with JSR 330 annotations, allowing developers to use annotations like `@Inject, @Named, @Qualifier`, etc., for dependency injection in Spring-managed beans.
- **@Inject:** This annotation is used to indicate dependencies that need to be injected into a class. Spring interprets this annotation in a similar way to its `@Autowired` annotation
- **@Named:** This annotation can be used to create an object similar to the steriotype annotation `@Component`
- **@Qualifier:** This annotation is used in conjunction with `@Inject` or `@Autowired` to specify which bean should be injected when there are multiple beans of the same type.
- Using JSR 330 annotations in Spring can enhance code readability and maintainability by adhering to `standardized dependency` injection practices.

![image](https://github.com/user-attachments/assets/dbff47a6-80ab-475e-b6e1-db73e13d7b4b)

```java
package com.accenture.lkm;

import javax.inject.Inject;
import javax.inject.Named;

import org.springframework.beans.factory.annotation.Value;

@Named("employee")
public class Employee {

	@Value("1001")
	private Integer employeeId;
	@Value("JAS")
	private String employeeName;
	@Value("56000.0")
	private Double salary;
	
	@Inject
	//@Autowired(required = false)
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

- Setter injection

```java
@Inject
	public void setAddress(Address address) {
		this.address = address;
	}
```

- Constructor Injection

```java
@Inject
	public Employee(Address address) {
		System.out.println("From Constructor of Employee class....");
		this.address=address;
	}
```

## What is JSR250 in SpringCore?
- JSR-250, also known as the "Common Annotations for the Java Platform", is a specification that defines a set of common annotations for use in Java applications. These annotations provide metadata to Java classes, methods, and fields, allowing frameworks and tools to understand and process them more effectively.
- In the context of Spring Core, JSR-250 annotations are often used for declarative configuration of Spring beans. Spring Core provides support for these annotations, allowing developers to use them to define aspects of their Spring-managed components without having to rely on XML configuration files.
- **@Resource:** Used to declare a dependency on another bean by name similar to @Autowired, type, or qualifier. In `@Autowired` we can make the annotation required true or false but for `@Resource` it always true
- **@PostConstruct:** Specifies a method that should be executed after a bean has been initialized.
- **@PreDestroy:** Specifies a method that should be executed before a bean is removed from the container.

```java
package com.accenture.lkm;



import javax.annotation.Resource;

import org.springframework.beans.factory.annotation.Value;
import org.springframework.stereotype.Component;

@Component("employee")
public class Employee {

	@Value("1001")
	private Integer employeeId;
	@Value("JAS")
	private String employeeName;
	@Value("56000.0")
	private Double salary;
	
	@Resource
	//@Autowired(required = false)
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





