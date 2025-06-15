## Eager and Lazy bean creation:
In Spring Core, `eager` and `lazy` bean creation refer to the timing at which `Spring instantiates` and `initializes beans` within the application context.

### Eager Initialization:
- In eager initialization, beans are created and initialized `as soon as the Spring Container starts up`, regardless of whether they are immediately needed or not.
- Eagerly initialized beans are typically `singletons`, and their initialization happens during the application context initialization phase.
- Eager initialization is the `default behavior` for singleton-scoped beans in spring.

### Lazy Initialization:
- In lazy initialization, beans are created and initialized only when they are `first requested or injected into another bean`. Lazy initialization defers the `bean's creation until it is actually needed`.
- Lazy initialization is useful for beans that are resource-intensive or have dependencies that are not always needed during application startup.
- By default, beans in Spring are lazily initialized if they are `prototypes` or if the `lazy-init` attribute is set to true.

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
	xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	xsi:schemaLocation="http://www.springframework.org/schema/beans
        http://www.springframework.org/schema/beans/spring-beans.xsd">


	<!-- One object -->
	<bean id="employee" class="com.accenture.lkm.Employee" lazy-init="true">
		<property name="employeeId" value="1001"></property>
		<property name="employeeName" value="JAS"></property>
		<property name="salary" value="56000.0"></property>
	</bean>
</beans>
```

```java
package com.accenture.lkm.ui;

import org.springframework.context.ApplicationContext;
import org.springframework.context.support.ClassPathXmlApplicationContext;

public class UITester2 {

	public static void main(String[] args) {

		ApplicationContext applicationContext = 
				new ClassPathXmlApplicationContext("com/accenture/lkm/resources/my_springbean2.xml");

		((ClassPathXmlApplicationContext)applicationContext).close();
	}
}
/*
 * Employee Object is not created as it is configured as LAZY
 */

```

## Java Configuration:

```java
package com.accenture.lkm.resources;

import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;
import org.springframework.context.annotation.Lazy;

import com.accenture.lkm.Employee;

@Configuration

public class MyConfiguration2 {

	@Bean(name="employee")
	@Lazy(value=true)
	public Employee createEmployee() {
		Employee employee = new Employee();
		employee.setEmployeeId(1001);
		employee.setEmployeeName("JAS");
		employee.setSalary(1900.0);
		return employee;
	}

	
}

```

### Spring Bean life cycle

![image](https://github.com/user-attachments/assets/ff278c96-f7bb-4e49-95e2-26072dc9dda8)

## What is BeanPostProcessor is Spring Core?
In Spring Core, `BeanPostProcessor` is an interface that allows for customizing bean instantiation and initialization in the Spring Container. It Provides hooks that enable you to perform custom processing before and after bean initialiation, as well as before and after bean initialization callbacks.
**The BeanPostProcessor will execute for every bean in the application**.
The BeanPostProcessor interface defines two methods:
`postProcessBeforeInitialization(Object bean, String beanName)`: This method is invoked before the initialization phase of a bean. It allows you to perform custom processing on the bean instance before any initialization callbacks (like InitializingBean's afterPropertiesSet() method or custom @PostConstruct methods) are called.
`postProcessAfterInitialization(Object bean, String beanName)`: This method is invoked after the initialization phase of a bean. It allows you to perform custom processing on the bean instance after all initialization callbacks have been called.

```java
package com.accenture.lkm.business.bean;

import org.springframework.beans.BeansException;
import org.springframework.beans.factory.config.BeanPostProcessor;

public class MyBeanPostProcessor implements BeanPostProcessor {

	@Override
	public Object postProcessBeforeInitialization(Object bean, String beanName) throws BeansException {
		System.out.println("3. postProcessBeforeInitialization");
		return bean;
	}
	
	@Override
	public Object postProcessAfterInitialization(Object bean, String beanName) throws BeansException {
		System.out.println("7. postProcessAfterInitialization");
		if (bean instanceof Employee) {
			Employee employee = (Employee) bean;
			if (employee.getLastName() == null)
				employee.setLastName(" SAN");
		}
		return bean;
	}

}

```

```java
package com.accenture.lkm.business.bean;

import javax.annotation.PostConstruct;

import org.springframework.beans.factory.InitializingBean;


public class Employee implements InitializingBean {
	
	private String firstName;	
	private String lastName;

	public Employee() {
		System.out.println("1. constructor");
	}

	public void setFirstName(String firstName) {
		this.firstName = firstName;
		System.out.println("2. setter");
	}

	@PostConstruct
	private void postConstruct() {
		System.out.println("4. postConstruct()");
	}

	@Override
	public void afterPropertiesSet() throws Exception {
		System.out.println("5. afterPropertiesSet()");
	}

	public void myInit() {
		System.out.println("6. init()");
	}

	public String getFirstName() {
		return firstName;
	}

	public String getLastName() {
		return lastName;
	}
	public void setLastName(String lastName) {
		this.lastName = lastName;
	}

	@Override
	public String toString() {
		return "Employee [firstName=" + firstName + ", lastName=" + lastName + "]";
	}

}
```
