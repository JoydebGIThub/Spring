## Method Invocation
- Address class

```java
package com.accenture.lkm;

import org.springframework.beans.factory.annotation.Value;
import org.springframework.stereotype.Component;

@Component("address")
public class Address {
	
	@Value("HSR, Layout Sector1")
	private String addressLine1;

	@Value("Karnatka, Bangalore")
	private String addressLine2;

	public Address() {
		System.out.println("From the constructor of Address class\n");
	}

	public String getAddressLine1() {
		return addressLine1;
	}

	public void setAddressLine1(String addressLine1) {
		this.addressLine1 = addressLine1;
	}

	public String getAddressLine2() {	
		return addressLine2;
	}

	public void setAddressLine2(String addressLine2) {
		this.addressLine2 = addressLine2;
	}

}
```
- Employee class
```java
package com.accenture.lkm;

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
	
	
	@Value("#{address}")
	private Address address;

	@Value("#{address.addressLine1}")
	private String addressLine1;

	@Value("#{address.getAddressLine2().toUpperCase()}")
	private String addressLine2;

	
	public Employee() {
		System.out.println("From Constructor of Employee: Created the Employee Object and injected the Address Object\n");
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

	public String getAddressLine1() {
		return addressLine1;
	}

	public void setAddressLine1(String addressLine1) {
		this.addressLine1 = addressLine1;
	}

	public void display() {
		System.out.println("\nEmployee Details are:");
		System.out.println("Employee ID:" + this.employeeId);
		System.out.println("Emloyee Name: " + this.employeeName);
		System.out.println("Employee Salary:" + this.salary);
		
		System.out.println("Address Line1 in Employee:" + this.addressLine1);
		System.out.println("Address Line2 in Employee:" + this.addressLine2);
		
	
		System.out.println("\nAddress line1:" + this.address.getAddressLine1());
		System.out.println("Address line2:" + this.address.getAddressLine2());
	}
}
```

## Collection Injection
- FeedValuesBean class

```java
package com.accenture.lkm;

import java.util.ArrayList;
import java.util.HashMap;
import java.util.List;
import java.util.Map;

import org.springframework.stereotype.Component;

@Component("feedValuesBean")
public class FeedValuesBean {

	private Map<String, String> mapPropertyFromFeeder;
	private List<String> listPropertyFromFeeder;

	public FeedValuesBean() {
		mapPropertyFromFeeder = new HashMap<String, String>();
		mapPropertyFromFeeder.put("tom", "tom@gmail.com");
		mapPropertyFromFeeder.put("mike", "mike@gmail.com");
 
		listPropertyFromFeeder = new ArrayList<String>();
		listPropertyFromFeeder.add("apple");
		listPropertyFromFeeder.add("boy");
 
	}
}
```

- Employee class

```java
package com.accenture.lkm;

import java.util.List;
import java.util.Map;

import org.springframework.beans.factory.annotation.Value;
import org.springframework.stereotype.Component;

@Component("employee")
public class Employee {
	
	@Value("1001")
	private Integer employeeId;
	
	@Value("56000.0")
	private Double salary;
	
	@Value("#{address}")
	private Address address;

	

	/* Collection Properties */
	@Value("#{feedValuesBean.listPropertyFromFeeder}") // List: [apple, boy]
	private List<Object> listProperty;

	@Value("#{feedValuesBean.mapPropertyFromFeeder}") // Map: {tom=tom@gmail.com, mike=mike@gmail.com}
	private Map<String, String> mapProperty;

	@Value("#{feedValuesBean.listPropertyFromFeeder[0]}") // apple
	private String employeeName;

	@Value("#{feedValuesBean.mapPropertyFromFeeder[tom]}") // tom@gmail.com
	private String email;
	/* Collection Properties */

	public Employee() {
		System.out.println("From Constructor of Employee: Created the Employee Object and injected the Address Object\n");
	}


	public void display() {
		System.out.println("\nEmployee Details are:");
		System.out.println("Employee ID:" + this.employeeId);
		System.out.println("Employee Name : " + this.employeeName);
		System.out.println("Employee Salary:" + this.salary);
		System.out.println("Email : " + this.email);
		
		System.out.println("\nAddress line1:" + this.address.getAddressLine1());
		System.out.println("Address line2:" + this.address.getAddressLine2());
		
		System.out.println("\nList:"+listProperty);
		System.out.println("Map:"+mapProperty);
	}
}
```





