## Setter injection
- if Bean is not given the bean is created by the name of the method

```java
package com.accenture.lkm.resources;

import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;

import com.accenture.lkm.Address;
import com.accenture.lkm.Employee;

@Configuration
public class MyConfiguration {
	
	@Bean
	//if Bean name is not given then the bean is created by the name of the method
	//here it is createEmployee
	public Employee createEmployee(){
		Employee  employee = new Employee();
		employee.setEmployeeId(1001);    
		employee.setEmployeeName("JAS");
		employee.setSalary(56000.0);
		employee.setAddress(createAddress());
		return employee;
	}
	
	@Bean(name="address")
	public Address createAddress(){
		Address address = new Address();
		address.setAddressLine1("AddressLine1");
		address.setAddressLine2("AddressLine2");
		return address;
	}
}

```

```java
ApplicationContext applicationContext = 
				new AnnotationConfigApplicationContext(MyConfiguration.class);
		Employee employee = applicationContext.getBean("createEmployee",Employee.class);
		employee.display();
```

- Give a particular name to the `bean`

```java
package com.accenture.lkm.resources;

import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;

import com.accenture.lkm.Address;
import com.accenture.lkm.Employee;

@Configuration
public class MyConfiguration {
	@Bean(name="employee")
	public Employee createEmployee(Address address){//Managed Parameter
		Employee  employee = new Employee();
		employee.setEmployeeId(1001);    
		employee.setEmployeeName("JAS");
		employee.setSalary(56000.0);
		employee.setAddress(address);
		return  employee;
	}
	@Bean(name="address")
	public Address createAddress(){
		Address address = new Address();
		address.setAddressLine1("AddressLine1");
		address.setAddressLine2("AddressLine2");
		return address;
	}
}

```

- Using the `@ComponentScan` to scan the package for `Bean` instead of configue the bean by ownself

```java
package com.accenture.lkm.resources;

import org.springframework.context.annotation.ComponentScan;
import org.springframework.context.annotation.Configuration;

@Configuration
@ComponentScan(basePackages = "com.accenture.lkm")
public class MyConfiguration {

}
```

```java
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
```

- We can declare the values in properties and then we can use it in any component
``employee.properties``
```properties
employeeId: 1001
employeeName: JAS
salary: 56000.0
```

- Properties Source Configuration

```java
package com.accenture.lkm.resources;

import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;
import org.springframework.context.annotation.PropertySource;
import org.springframework.context.support.PropertySourcesPlaceholderConfigurer;

@Configuration
@PropertySource({"classpath:properties/employee.properties", "classpath:properties/address.properties"})
public class PropertiesFileConfiguration {
	
	@Bean
	public PropertySourcesPlaceholderConfigurer propertyConfigIn() {
		return new PropertySourcesPlaceholderConfigurer();
	}
}

```

- Java configuration for component scan

```java
package com.accenture.lkm.resources;

import org.springframework.context.annotation.ComponentScan;
import org.springframework.context.annotation.Configuration;
import org.springframework.context.annotation.Import;

@Configuration
@ComponentScan(basePackages="com.accenture.lkm")
@Import(PropertiesFileConfiguration.class)//Linking the configuration class
public class MyConfiguration {
	
}

```

- Employee component to use properties

```java
@Component("employee")
public class Employee {

	@Value("${employeeId}")//$ - Property Place Holder
	private Integer employeeId;
	@Value("${employeeName}")
	private String employeeName;
	@Value("${salary}")
	private Double salary;
	
	@Autowired
	private Address address;
```

### XML properties configuration
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
	
	<bean id="propConfig"
		class="org.springframework.context.support.PropertySourcesPlaceholderConfigurer">
		<property name="locations">
			 <list>
                <value>classpath:properties/employee.properties</value>
                <value>classpath:properties/address.properties</value>
            </list>
		</property>
	</bean>
	
	<bean id="employee" class="com.accenture.lkm.Employee">
	    <property  name="employeeId" value="${employeeId}" />
	    <property  name="employeeName" value="${employeeName}"/>   
	    <property  name="salary" value="${salary}"/>
	    <property  name="address" ref="address"/>           
	</bean>
	
	<bean id="address" class="com.accenture.lkm.Address">
		<property name="addressLine1" value="${addressLine1}"></property>
		<property name="addressLine2" value="${addressLine2}"></property>
	</bean>
</beans>
<!-- https://stackoverflow.com/questions/3403773/using-multiple-property-files-via-propertyplaceholderconfigurer-in-multiple-pr-->
```

- Using context

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
	
	<context:property-placeholder 
	    location="properties/employee.properties,
	    properties/address.properties"/>
	    
	    
	<bean id="employee" class="com.accenture.lkm.Employee">
	    <property  name="employeeId" value="${employeeId}" />
	    <property  name="employeeName" value="${employeeName}"/>   
	    <property  name="salary" value="${salary}"/>
	    <property  name="address" ref="address"/>           
	</bean>
	
	<bean id="address" class="com.accenture.lkm.Address">
		<property name="addressLine1" value="${addressLine1}"></property>
		<property name="addressLine2" value="${addressLine2}"></property>
	</bean>
</beans>
```
