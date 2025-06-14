## Spring Auto Wiring?
`Autowiring` in Spring Core is a feature that allows Spring to automatically inject dependencies into `String-managed beans`. Instead of explicitly configuring each dependency in XML or Java configuration, `autowiring` allows spring to automatically wire beans on certain rules.
With autowiring, you don't need to specify **<constructor-arg> or <property>** elements in your bean configuration file to inject dependencies. Instead, spring automatically looks for beans match the type of property or constructor argument and injects them.
There are several modes of **autowiring in Spring**

### No Autowiring:
This is the default mode. In this mode, we have to `explicitly wire` beans using **<constructor-arg> or <property>** elements with `ref/innerbean` in **XML configuration**
```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
	xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	xsi:schemaLocation="http://www.springframework.org/schema/beans
        http://www.springframework.org/schema/beans/spring-beans.xsd">
	
	<bean id="address" class="com.accenture.lkm.Address"><!-- non param - Constructor to create an object -->
		<property name="addressLine1" value="Hyderabad"></property><!-- appropiate setter -->
		<property name="addressLine2" value="Telangana"></property>
	</bean>
	
	<bean id="employee" class="com.accenture.lkm.Employee"><!-- non param - Constructor to create an object -->
		<property name="employeeId" value="1001"></property>
		<property name="employeeName" value="JAS"></property>
		<property name="salary" value="56000.0"></property>
		<property name="address" ref="address"></property><!-- dependency injection -->
	</bean>
	
</beans>
```

### By Name:
Spring tries to find a bean with the same name as the property or constructor argument and injects it. You need to make sure that the bean name matches the property name.
Means the bean name `address` must matches the `setter method` name or the `constructor parameter` name
```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
	xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	xsi:schemaLocation="http://www.springframework.org/schema/beans
        http://www.springframework.org/schema/beans/spring-beans.xsd">
	
	<bean id="address" class="com.accenture.lkm.Address">
		<property name="addressLine1" value="Hyderabad"></property>
		<property name="addressLine2" value="Telangana"></property>
	</bean>
	
	<bean id="employee" class="com.accenture.lkm.Employee" autowire="byName">
		<property name="employeeId" value="1001"></property>
		<property name="employeeName" value="JAS"></property>
		<property name="salary" value="56000.0"></property>
	</bean>
	
</beans>
```

### By Type:
Spring tries to find a bean of the same type as the property or constructor argument and injects it. If there are multiple beans of the same type, it throws an exception unless you specify further qualification.
Means the type mentioned in the class of the bean match with the type of the setMethod or Constructor parameter
```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
	xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	xsi:schemaLocation="http://www.springframework.org/schema/beans
        http://www.springframework.org/schema/beans/spring-beans.xsd">
	
	<bean id="localAddress1" class="com.accenture.lkm.Address">
		<property name="addressLine1" value="Hyderabad"></property>
		<property name="addressLine2" value="Telangana"></property>
	</bean>
	
	<!-- Try uncommenting the below and execute and observe -->
	<!-- 
	<bean id="localAddress2" class="com.accenture.lkm.Address">
		<property name="addressLine1" value="Banglore"></property>
		<property name="addressLine2" value="karnataka"></property>
	</bean>
	 -->
	
	<bean id="employee" class="com.accenture.lkm.Employee" autowire="byType">
		<property name="employeeId" value="1001"></property>
		<property name="employeeName" value="JAS"></property>
		<property name="salary" value="56000.0"></property>
	</bean>
	
</beans>
```
- when we using `type auto wiring` and there 2 bean has same type then a exception occure called `NoUniqueBeanDefinationException`

#### Solve the NoUniqueBeanDefinationException
- By making any one of the bean `primary`

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
	xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	xsi:schemaLocation="http://www.springframework.org/schema/beans
        http://www.springframework.org/schema/beans/spring-beans.xsd">
	
	<bean id="localAddress1" class="com.accenture.lkm.Address">
		<property name="addressLine1" value="Hyderabad"></property>
		<property name="addressLine2" value="Telangana"></property>
	</bean>
	
	<bean id="localAddress2" class="com.accenture.lkm.Address" primary="true">
		<property name="addressLine1" value="Banglore"></property>
		<property name="addressLine2" value="karnataka"></property>
	</bean>
	
	<bean id="employee" class="com.accenture.lkm.Employee" autowire="byType">
		<property name="employeeId" value="1001"></property>
		<property name="employeeName" value="JAS"></property>
		<property name="salary" value="56000.0"></property>
	</bean>	
</beans>
```

### Constructor Autowiring
Spring automatically wires constructor arguments with beans by type/by name. This is particularly useful when a bean has multiple dependencies and you want to wire them via the constructor.
```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
	xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	xsi:schemaLocation="http://www.springframework.org/schema/beans
        http://www.springframework.org/schema/beans/spring-beans.xsd">
	
	<bean id="localAddress1" class="com.accenture.lkm.Address">
		<property name="addressLine1" value="Hyderabad"></property>
		<property name="addressLine2" value="Telangana"></property>
	</bean>
	
	<!-- Try uncommenting the below and execute and observe -->
	<!-- 
	<bean id="localAddress2" class="com.accenture.lkm.Address">
		<property name="addressLine1" value="Banglore"></property>
		<property name="addressLine2" value="karnataka"></property>
	</bean>
	 -->
	<bean id="employee" class="com.accenture.lkm.Employee" autowire="constructor">
		<property name="employeeId" value="1001"></property>
		<property name="employeeName" value="JAS"></property>
		<property name="salary" value="56000.0"></property>
	</bean>
	
</beans>
```

## Autowire in XML will work on static fields and method
```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
	xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	xsi:schemaLocation="http://www.springframework.org/schema/beans
        http://www.springframework.org/schema/beans/spring-beans.xsd">
	
	<bean id="localAddress1" class="com.accenture.lkm.Address">
		<property name="addressLine1" value="Hyderabad"></property>
		<property name="addressLine2" value="Telangana"></property>
	</bean>
	
	<bean id="employee" class="com.accenture.lkm.Employee" autowire="byType">
		<property name="employeeId" value="1001"></property>
		<property name="employeeName" value="JAS"></property>
		<property name="salary" value="56000.0"></property>
	</bean>
	
</beans>
```

```java
package com.accenture.lkm;

public class Employee {

	private Integer employeeId;
	private String employeeName;
	private Double salary;
	
	private static Address address;

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

	public static Address getAddress() {
		return address;
	}

	public static void setAddress(Address address) {
		System.out.println("From Setter of employee Address");
		Employee.address = address;
	}

	public void display() {
		System.out.println("\nEmployee Details are:");
		System.out.println("Employee ID:" + this.employeeId);
		System.out.println("Employee Name:" + this.employeeName);
		System.out.println("Employee Salary:" + this.salary);
		System.out.println("\nAddress line1:" + Employee.address.getAddressLine1());
		System.out.println("Address line2:" + Employee.address.getAddressLine2());
	}
}
```
