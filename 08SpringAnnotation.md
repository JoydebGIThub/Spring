## Java Annotations
We need `AOP library` for using Annotations and below are some annotations used
- @Component replacing the `<bean>` tag
- @Value replace the `<property>` tag
- @Profile
- @Autowired replace `autowire="byType/byName/constructor"`

To make this annotation work write `<context: component-scan base-package= "com.accenture.lkm">`
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


	<!-- used for activating jsr 250 annotations preconstruct,postconstruct 
		and @resources -->
	<!-- <context:annotation-config/> -->	
	
	<context:component-scan base-package="com.accenture.lkm"/>
</beans>
```
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
	
	@Value("#{address}")//SpEL - Spring Expression Language Syntax // @Component("address") in the Address class
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

## Spring Annotation with Operator
- OperatorsBean class
```java
package com.accenture.lkm;

import org.springframework.beans.factory.annotation.Value;
import org.springframework.stereotype.Component;

@Component("operatorsBean")
public class OperatorsBean {

	//Relational operators
	@Value("#{1 == 1}") //true
	private boolean testEqual;
	
	@Value("#{1 != 1}") //false
	private boolean testNotEqual;
	
	@Value("#{1 < 1}") //false
	private boolean testLessThan;
	
	@Value("#{1 <= 1}") //true
	private boolean testLessThanOrEqual;
	
	@Value("#{1 > 1}") //false
	private boolean testGreaterThan;
	
	@Value("#{1 >= 1}") //true
	private boolean testGreaterThanOrEqual;

	//Logical operators 
	
	@Value("#{numberBean.no == 999 and numberBean.no < 900}") //false
	private boolean testAnd;
	
	@Value("#{numberBean.no == 999 or numberBean.no < 900}") //true
	private boolean testOr;
	
	@Value("#{!(numberBean.no == 999)}") //false
	private boolean testNot;

	@Value("#{2 > 1 ? 'a' : 'b'}") // "a"
	private String ternary;
	
	@Value("#{null ?: 'b'}") // "b"
	private String elvis;
	
	
	//Mathematical operators
	
	@Value("#{1 + 1}") //2.0
	private double testAdd;
	
	@Value("#{'1' + '@' + '1'}") //1@1
	private String testAddString;
	
	@Value("#{1 - 1}") //0.0
	private double testSubtraction;

	@Value("#{1 * 1}") //1.0
	private double testMultiplication;
	
	@Value("#{10 / 2}") //5.0
	private double testDivision;
	
	@Value("#{10 % 10}") //0.0
	private double testModulus ;
	
	@Value("#{2 ^ 2}") //4.0
	private double testExponentialPower;

	public void testElvisTernary() {
		System.out.println(elvis + " " + ternary);
	}
	@Override
	public String toString() {
		return "OperatorBean [testEqual=" + testEqual + "\n, testNotEqual="
				+ testNotEqual + "\n, testLessThan=" + testLessThan
				+ "\n, testLessThanOrEqual=" + testLessThanOrEqual
				+ "\n, testGreaterThan=" + testGreaterThan
				+ "\n, testGreaterThanOrEqual=" + testGreaterThanOrEqual
				+ "\n, testAnd=" + testAnd + "\n, testOr=" + testOr + "\n, testNot="
				+ testNot + "\n, testAdd=" + testAdd + "\n, testAddString="
				+ testAddString + "\n, testSubtraction=" + testSubtraction
				+ "\n, testMultiplication=" + testMultiplication
				+ "\n, testDivision=" + testDivision + "\n, testModulus="
				+ testModulus + "\n, testExponentialPower="
				+ testExponentialPower + "]";
	}
	
}
```

- NumberBean class

```java
package com.accenture.lkm;

import org.springframework.beans.factory.annotation.Value;
import org.springframework.stereotype.Component;

@Component("numberBean")
public class NumberBean {

	@Value("999")
	private int no;

	public NumberBean() {
		// TODO Auto-generated constructor stub
	}

	public int getNo() {
		return no;
	}

	public void setNo(int no) {
		this.no = no;
	}

}
```

- Main method
```java
package com.accenture.lkm.ui;

import org.springframework.context.ApplicationContext;
import org.springframework.context.support.ClassPathXmlApplicationContext;

import com.accenture.lkm.OperatorsBean;

public class UITester {

	public static void main(String[] args) {

		ApplicationContext applicationContext = new ClassPathXmlApplicationContext(
				"com/accenture/lkm/resources/my_springbean.xml");
		OperatorsBean operatorsBean = applicationContext.getBean("operatorsBean",OperatorsBean.class);
		System.out.println(operatorsBean);
		operatorsBean.testElvisTernary();
		
		((ClassPathXmlApplicationContext)applicationContext).close();
	}

}
// EL Operators and SPEL
```









