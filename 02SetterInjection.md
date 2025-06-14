## Setter Injection:
- We need an container to create the object. We create the container using `ApplicationContext`. But to use the spring we need a `spring jar file`.
- So, 1st we need to **add a jar file**

![image](https://github.com/user-attachments/assets/446476b8-f212-40f7-aaf3-c2e0526bda5f)
![image](https://github.com/user-attachments/assets/9e20cbbc-eb3b-46da-ad6d-30f3f90f5b57)

- now we need to create a container and create the object there. And all the things present in the `container` is called as `bean`

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
	xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	xsi:schemaLocation="http://www.springframework.org/schema/beans
        http://www.springframework.org/schema/beans/spring-beans.xsd">
        <!-- id means object name  -->
<bean id="address" class="com.joydeb.Address">
<!-- name should be the exact the setter name -->
<property name="address" value="Kolkata"></property>	
</bean>
<bean id="employee" class="com.joydeb.Employee">
<property name="name" value="Joydeb"></property>	
<!-- pass the name of the reference object -->
<property name="address" ref="address"></property> <!-- dependency injection -->
</bean>
</beans>
```

## How the data is injecting
- Using the `setter Injection`, when we create a `bean` this methods get called

```java
public void setName(String name) {
	System.out.println("From Setter of name");
	this.name = name;
}
public void setAddress(Address address) {
	System.out.println("From Setter of Address");
	this.address = address;
}
```

- and this method will set the data in the particular fields `name, address`
- In `bean` creation we need to mention the exect method name without the `set` before the method name. The `setAdress()` will internally convert into `address`.

```xml
<bean id="employee" class="com.joydeb.Employee">
<property name="name" value="Joydeb"></property>	
<!-- pass the name of the reference object -->
<property name="address" ref="address"></property> <!-- dependency injection -->
</bean>
```

- Best exmple to understant that it is setter Injection
```java
public void setAddress2(Address address) {
	System.out.println("From Setter of Address");
	this.address = address;
}
```
```xml
<bean id="employee" class="com.joydeb.Employee">
<property name="name" value="Joydeb"></property>	
<!-- pass the name of the reference object -->
<property name="address2" ref="address"></property> <!-- dependency injection -->
</bean>
```

## Create multiple bean for different object
```xml
<bean id="employee1" class="com.accenture.lkm.Employee"><!-- non param - Constructor to create an object -->
<property name="employeeId" value="1001"></property>
<property name="employeeName" value="JAS"></property>
<property name="salary" value="56000.0"></property>
<property name="address" ref="address1"></property><!-- dependency injection -->
</bean>

<bean id="employee2" class="com.accenture.lkm.Employee"><!-- non param - Constructor to create an object -->
<property name="employeeId" value="1002"></property>
<property name="employeeName" value="SAN"></property>
<property name="salary" value="96000.0"></property>
<property name="address" ref="address2"></property><!-- dependency injection -->
</bean>
```

```java
ApplicationContext applicationContext = new 
	ClassPathXmlApplicationContext("com/accenture/lkm/resources/my_springbean.xml");
		
Employee employee1 = applicationContext.getBean("employee1",Employee.class);
employee1.display();
		
Employee employee2 = applicationContext.getBean("employee2",Employee.class);
employee2.display();

((ClassPathXmlApplicationContext)applicationContext).close();
```

## Using P schema created the Bean
- Easier way to create the bean

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
	xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	xsi:schemaLocation="http://www.springframework.org/schema/beans
    	http://www.springframework.org/schema/beans/spring-beans.xsd"
	xmlns:p="http://www.springframework.org/schema/p">
	
	<bean id="address" class="com.accenture.lkm.Address"
		p:addressLine1="Hyderabad"
		p:addressLine2="Telangana"></bean>
	
	<bean id="employee" class="com.accenture.lkm.Employee"
		p:employeeId="1001"
		p:employeeName="JAS"
		p:salary="56000.0"
		p:address-ref="address"></bean>
	
</beans>
```







