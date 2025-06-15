## Spring Annotation Autowiring
```java
package com.accenture.lkm;

import org.springframework.beans.factory.annotation.Autowired;
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
	
	@Autowired
	private Address address;

	/*
	//@Autowired
	public Employee(Address address) {
		System.out.println("Employee Class Constructor....");
		this.address=address;
	}
	*/
	
	public Employee() {
		System.out.println("Employee Class Constructor....");
	}
	
	public Integer getEmployeeId() {
		return employeeId;
	}

	public void setEmployeeId(Integer employeeId) {
		System.out.println("From Setter of employee id");
		this.employeeId = employeeId;
	}

	public Double getSalary() {
		return salary;
	}

	public void setSalary(Double salary) {
		System.out.println("From Setter of employee salary");
		this.salary = salary;
	}

	public String getEmployeeName() {
		return employeeName;
	}

	public void setEmployeeName(String employeeName) {
		System.out.println("From Setter of employee name");
		this.employeeName = employeeName;
	}

	public Address getAddress() {
		return address;
	}

	//@Autowired
	public void setAddress(Address address) {
		System.out.println("From Setter of employee Address");
		this.address = address;
	}

	public void display() {
		System.out.println("\nEmployee Details are:");
		System.out.println("Employee ID:" + this.employeeId);
		System.out.println("Employee Name:" + this.employeeName);
		System.out.println("Employee Salaray:" + this.salary);
		System.out.println("\nAddress line1:" + this.address.getAddressLine1());
		System.out.println("Address line2:" + this.address.getAddressLine2());
	}
}
```

- We also make the `@Autowired(required=false)` so that if the `dependency` is not ready when creation of the dependent bean then it will not throw any `NullPointerException`.

```java
package com.accenture.lkm;

import java.util.Optional;

import org.springframework.beans.factory.annotation.Autowired;
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
	
	@Autowired(required = false)
	private Address address;
	
	public Employee() {
		System.out.println("Employee Class Constructor....");
	}
	
	public Integer getEmployeeId() {
		return employeeId;
	}

	public void setEmployeeId(Integer employeeId) {
		System.out.println("From Setter of employee id");
		this.employeeId = employeeId;
	}

	public Double getSalary() {
		return salary;
	}

	public void setSalary(Double salary) {
		System.out.println("From Setter of employee salary");
		this.salary = salary;
	}

	public String getEmployeeName() {
		return employeeName;
	}

	public void setEmployeeName(String employeeName) {
		System.out.println("From Setter of employee name");
		this.employeeName = employeeName;
	}

	public Address getAddress() {
		return address;
	}

	public void setAddress(Address address) {
		System.out.println("From Setter of employee Address");
		this.address = address;
	}

	public void display() {
		System.out.println("\nEmployee Details are:");
		System.out.println("Employee ID:" + this.employeeId);
		System.out.println("Employee Name:" + this.employeeName);
		System.out.println("Employee Salary:" + this.salary);
		
		Optional<Address> optional = Optional.ofNullable(this.address);
		optional.ifPresent(address->{
			System.out.println("\nAddress line1:" + address.getAddressLine1());
			System.out.println("Address line2:" + address.getAddressLine2());
		});
		
	}
}
```

- Use `@Autowired` for static feild:

```java
@Autowired
	private static Address address;
```

- Use `@Autowired` for static setMethod:

```java
@Autowired
	public static void setAddress(Address address) {
		System.out.println("From Setter of employee Address");
		Employee.address = address;
	}
```

- Use `@Autowired` for static feild of non static method:

```java
private static Address address;
public Address getAddress() {
		return address;
	}

	@Autowired
	public void setAddress(Address address) {
		System.out.println("From Setter of employee Address");
		Employee.address = address;
	}
```

- If the 2 bean with same type came then a issue will be occure when we use `@Autowired` because it is `byType` by default.
- To Resolve we can use `@Qualifier`

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
	xmlns:context="http://www.springframework.org/schema/context"
	xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	xsi:schemaLocation="
        http://www.springframework.org/schema/beans     
        http://www.springframework.org/schema/beans/spring-beans.xsd
        http://www.springframework.org/schema/context 
        http://www.springframework.org/schema/context/spring-context.xsd">

	
	<context:component-scan base-package="com.accenture.lkm"/>
		
	<bean id="myAddress1" class="com.accenture.lkm.Address">
		<property name="addressLine1" value="HSR Layout, Sector1**" />
		<property name="addressLine2" value="Bangalore, Karnatka**" />
	</bean>

	<bean id="myAddress2" class="com.accenture.lkm.Address">
		<property name="addressLine1" value="Bellandur, Eco Space" />
		<property name="addressLine2" value="Bangalore, Karnatka" />
	</bean> 
	
</beans>
```

```java
  @Autowired
	@Qualifier("myAddress2")
	private Address address;
```
