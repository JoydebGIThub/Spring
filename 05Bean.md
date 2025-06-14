## Inner Bean
- Bean inside a bean

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
	xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	xsi:schemaLocation="http://www.springframework.org/schema/beans
        http://www.springframework.org/schema/beans/spring-beans.xsd">
	
	<bean id="address" class="com.accenture.lkm.Address"><!--2 param - Constructor to create an object -->
		<constructor-arg><value>Hyderabad</value></constructor-arg>
		<constructor-arg><value>Telangana</value></constructor-arg>
	</bean>
	
	<bean id="employee" class="com.accenture.lkm.Employee"><!--2 param - Constructor to create an object -->
		<!-- Try by changing the order of constructor-arg tag -->
		<constructor-arg ref="address"></constructor-arg>
		<constructor-arg>
			<!-- Inner Bean will generally not have id -->
			<bean id="contact" class="com.accenture.lkm.Contact"><!--2 param - Constructor to create an object -->
				<constructor-arg><value>jas@accenture.com</value></constructor-arg>
				<constructor-arg><value>9988776644</value></constructor-arg>
			</bean>
		</constructor-arg>
		
		<property name="employeeId"><value>1001</value></property>
		<property name="employeeName"><value>JAS</value></property>
		<property name="salary"><value>56000.0</value></property>
	</bean>
	
</beans>
```

## Bean Scopes:
In Spring Framework, the scope of a bean defines the `lifecycle` and `visibility` of `instances managed` by the the Spring container. The scope determines how and when a new instance of bean should be created and when it should be destroyed. Spring provides several bean scopes, each serving different purposes:

### Singleton Scope:
This is the default scope in Spring. In singleton scope, the Spring container creates `only one instance of the bean` per container. This single instance is shared across the entire application context.

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
	xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	xsi:schemaLocation="http://www.springframework.org/schema/beans
        http://www.springframework.org/schema/beans/spring-beans.xsd">

	<!-- default scope is singleton try removing scope="singleton" -->
	<bean id="employee" class="com.accenture.lkm.Employee" scope="singleton">
		<property name="employeeId" value="1001"></property> 
		<property name="employeeName" value="JAS"></property>
		<property name="salary" value="200000"></property>	
	</bean>

</beans>
```

```java
package com.accenture.lkm.ui;

import org.springframework.context.ApplicationContext;
import org.springframework.context.support.ClassPathXmlApplicationContext;

import com.accenture.lkm.Employee;

public class UITester1 {

	public static void main(String[] args) {

		ApplicationContext applicationContext = new 
				ClassPathXmlApplicationContext("com/accenture/lkm/resources/my_springbean1.xml");
		
		Employee employee1 = applicationContext.getBean("employee",Employee.class);
		Employee employee2 = applicationContext.getBean("employee",Employee.class);
		Employee employee3 = applicationContext.getBean("employee",Employee.class);
		Employee employee4 = applicationContext.getBean("employee",Employee.class);
		
		System.out.println(employee1.hashCode());
		System.out.println(employee2.hashCode());
		System.out.println(employee3.hashCode());
		System.out.println(employee4.hashCode());
		
		((ClassPathXmlApplicationContext)applicationContext).close();
	}

}

```

### Prototype Scope:
In `prototype scope`, the Spring container creates a `new instance of the bean every time it is requested`. This means that every time you ask the container for a bean of that type, you get a new instance.
```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
	xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	xsi:schemaLocation="http://www.springframework.org/schema/beans
        http://www.springframework.org/schema/beans/spring-beans.xsd">

	<bean id="employee" class="com.accenture.lkm.Employee" scope="prototype">
		<property name="employeeId" value="1001"></property> 
		<property name="employeeName" value="JAS"></property>
		<property name="salary" value="200000"></property>	
	</bean>

</beans>
```

```java
package com.accenture.lkm.ui;

import org.springframework.context.ApplicationContext;
import org.springframework.context.support.ClassPathXmlApplicationContext;

import com.accenture.lkm.Employee;

public class UITester2 {

	public static void main(String[] args) {

		ApplicationContext applicationContext = new 
				ClassPathXmlApplicationContext("com/accenture/lkm/resources/my_springbean2.xml");
		
		Employee employee1 = applicationContext.getBean("employee",Employee.class);
		Employee employee2 = applicationContext.getBean("employee",Employee.class);
		Employee employee3 = applicationContext.getBean("employee",Employee.class);
		Employee employee4 = applicationContext.getBean("employee",Employee.class);
		
		System.out.println(employee1.hashCode());
		System.out.println(employee2.hashCode());
		System.out.println(employee3.hashCode());
		System.out.println(employee4.hashCode());
		
		((ClassPathXmlApplicationContext)applicationContext).close();
	}

}

```

### Request Scope:
Beans in `request scope` are `created once per HTTP request`. This means that a `new instance of the bean is created for every HTTP request`, and it's available only within that request. If we load the HTTP request an new instance will created.

### Session Scope:
Beans in `session scope` are created once per HTTP session. Similar to request scope, a new instance of the bean is created for each HTTP session and is available throughout the session's lifecycle. After loading also the same instance will be remained.

### Application Scope:
Also knows as `singleton per context` scope, beans in application scope are created once per Spring container. This means that there is only one instance of the bean per Spring application context.

### WebSocket Scope:
Introduced in Spring 4.2, this scope is similar to `session scope` but specific to WebSocket-based application. Beans in WebSocket scope are created once per WebSocket session.


