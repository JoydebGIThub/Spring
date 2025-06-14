## Enter the value in collection through IOC
```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
	xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	xsi:schemaLocation="http://www.springframework.org/schema/beans
        http://www.springframework.org/schema/beans/spring-beans.xsd">

	<bean id="addressObject" class="com.accenture.lkm.Address">
		<property name="addressLine1" value="HSR Layout, Sector1,Global" />
		<property name="addressLine2" value="Bangalore, Karnatka,Global" />
	</bean>

	<bean id="empObject" class="com.accenture.lkm.Employee">
		<property name="employeeId" value="1001"></property>
		<property name="employeeName" value="JAS"></property>
		<property name="salary" value="200000"></property>
		
		<!-- Setting List -->
		<property name="listProperty">
			<list>
				<value>Some value</value> 
				<ref bean="addressObject" /> 
				<bean class="com.accenture.lkm.Address">  <!-- Inner Bean -->
					<property name="addressLine1" value="Waverock, Gachibowli" />
					<property name="addressLine2" value="Hyderabad, Telangana" />
				</bean>
				<ref bean="addressObject" /> <!-- List allows duplicates -->
				<value>1001</value> 
				<null />
			</list>
		</property>

		<property name="setProperty">
			<set>
				<value>Some value</value> 
				<ref bean="addressObject" /> 
				<bean class="com.accenture.lkm.Address">  <!-- Inner Bean -->
					<property name="addressLine1" value="Waverock, Gachibowli" />
					<property name="addressLine2" value="Hyderabad, Telangana" />
				</bean>
				<ref bean="addressObject" /> <!-- Set will not allow duplicates -->
				<value>1001</value> 
				<null />
			</set>

		</property>

		<property name="mapProperty">
			<map>
				<entry>
					<key><value>1001</value></key>
					<value>John</value>
				</entry>

				<entry>
					<key><value>1002</value></key>
					<ref bean="addressObject" />
				</entry>

				<entry key="1003" value="Some value" />
				<entry key="1004" value-ref="addressObject" />
				<entry key="1005">
					<bean class="com.accenture.lkm.Address"> <!-- Inner Bean -->
						<property name="addressLine1" value="Waverock, Gachibowli" />
						<property name="addressLine2" value="Hyderabad, Telangana" />
					</bean>
				</entry>
				<entry key="null" value="null" />
			</map>
		</property>

		<property name="propertiesProperty">
			<props>
				<prop key="1">Value1</prop>
				<prop key="2">Value2</prop>
			</props>
		</property>
	</bean>
</beans>
```

```java
package com.accenture.lkm;

import java.util.List;
import java.util.Map;
import java.util.Properties;
import java.util.Set;

public class Employee {

	private Integer employeeId;
	private Double salary;
	private String employeeName;

	/* Collection Properties */
	private List<Object> listProperty;
	private Set<Object> setProperty;
	private Map<Object, Object> mapProperty;
	private Properties propertiesProperty;

	public List<Object> getListProperty() {
		return listProperty;
	}

	public void setListProperty(List<Object> listProperty) {
		this.listProperty = listProperty;
	}

	public Set<Object> getSetProperty() {
		return setProperty;
	}

	public void setSetProperty(Set<Object> setProperty) {
		this.setProperty = setProperty;
	}

	public Map<Object, Object> getMapProperty() {
		return mapProperty;
	}

	public void setMapProperty(Map<Object, Object> mapProperty) {
		this.mapProperty = mapProperty;
	}

	public Properties getPropertiesProperty() {
		return propertiesProperty;
	}

	public void setPropertiesProperty(Properties propertiesProperty) {
		this.propertiesProperty = propertiesProperty;
	}

	/* Collection Properties */

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
		System.out.println("Employee ID : " + this.employeeId);
		System.out.println("Employee Name : "+this.employeeName);
		System.out.println("Employee Salary : " + this.salary);
	}

}
```
