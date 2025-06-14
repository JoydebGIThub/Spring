## @Profile Annotation:
The `@Profile` annotation in Spring Core is used to specify which beans should be loaded intio the Spring annotation context based on the active profiles.
Profiles in Spring allow you to define sets of beans and configurations that should be active only under certain circumstances, such as specific environments (development, testing, production) or runtime conditions

```java
package com.accenture.lkm;

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
	
	@Value("#{address}")//SpEL - Spring Expression Language Syntax
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
		System.out.println("\nAddress line1:" + this.address.getAddressLine1());
		System.out.println("Address line2:" + this.address.getAddressLine2());
	}
}
```

- main class

```java
package com.accenture.lkm.ui;

import org.springframework.context.ApplicationContext;
import org.springframework.context.support.ClassPathXmlApplicationContext;

import com.accenture.lkm.Employee;

public class UITester {

	public static void main(String[] args) {

		//To activate the profile, try executing by commenting the below line 
		System.setProperty("spring.profiles.active", "test");
		
		ApplicationContext applicationContext = new 
				ClassPathXmlApplicationContext("com/accenture/lkm/resources/my_springbean.xml");
		
		Employee employee = applicationContext.getBean("employee",Employee.class);
		employee.display();

		((ClassPathXmlApplicationContext)applicationContext).close();
	}

}

```
