## Constructor Injection:
```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
	xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	xsi:schemaLocation="http://www.springframework.org/schema/beans
        http://www.springframework.org/schema/beans/spring-beans.xsd">
	
	<bean id="address" class="com.accenture.lkm.Address"><!--2 param - Constructor to create an object -->
		<constructor-arg value="Hyderabad"></constructor-arg>
		<constructor-arg value="Telangana"></constructor-arg>
	</bean>
	
	<bean id="employee" class="com.accenture.lkm.Employee"><!--4 param - Constructor to create an object -->
		<constructor-arg value="1001"></constructor-arg>
		<constructor-arg value="JAS"></constructor-arg>
		<constructor-arg value="56000.0"></constructor-arg>
		<constructor-arg ref="address"></constructor-arg>
	</bean>
	
</beans>
```
- In constructor we don't need to mention the fields it will automatically understand
- We need to `mentain the order`
- Here the `dependencies` are injected based on the `order of the data type present in the constructor`

### Short cut using C namespace
```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
	xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	xsi:schemaLocation="http://www.springframework.org/schema/beans
        http://www.springframework.org/schema/beans/spring-beans.xsd"
    xmlns:c="http://www.springframework.org/schema/c">
	
	<bean id="address" class="com.accenture.lkm.Address"
		c:addressLine1="Hyderabad"
		c:addressLine2="Telangana"><!--2 param - Constructor to create an object -->
	</bean>
	
	<bean id="employee" class="com.accenture.lkm.Employee"
		c:employeeId="1001"
		c:employeeName="JAS"
		c:salary="56000.0"
		c:address-ref="address"><!--4 param - Constructor to create an object -->
	</bean>
	
</beans>
```
- all the fields are **mandatory**

## Both Setter and Constructor injection
```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
	xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	xsi:schemaLocation="http://www.springframework.org/schema/beans
        http://www.springframework.org/schema/beans/spring-beans.xsd">
	
	<bean id="address" class="com.accenture.lkm.Address"><!--2 param - Constructor to create an object -->
		<constructor-arg value="Hyderabad"></constructor-arg>
		<constructor-arg value="Telangana"></constructor-arg>
	</bean>
	
	<bean id="contact" class="com.accenture.lkm.Contact"><!--2 param - Constructor to create an object -->
		<constructor-arg value="jas@accenture.com"></constructor-arg>
		<constructor-arg value="9988776644"></constructor-arg>
	</bean>
	
	<bean id="employee" class="com.accenture.lkm.Employee"><!--2 param - Constructor to create an object -->
		<!-- Try by changing the order of constructor-arg tag -->
		<constructor-arg ref="address"></constructor-arg>
		<constructor-arg ref="contact"></constructor-arg>
		
		<property name="employeeId" value="1001"></property>
		<property name="employeeName" value="jas"></property>
		<property name="salary" value="56000.0"></property>
	</bean>
	
</beans>

```

## If we don't want to follow the order of the argument
- If parameter composed of non implicit data type only then the `order of aurgument is not matter`
- Here I have 2 data type `address` and `contact` we if i don't follow the order of injecting them in the constructor then there will be no issue

### What if the order of argument define in a configuration file is different from the order of augumnet defined in a constructor:
- For this type of problem we get an `exception` called `unsatisfiedDependencyInjection`
- To solve this we can use `type attribute`

#### Mention type attribute in constructor to define the type argument
```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
	xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	xsi:schemaLocation="http://www.springframework.org/schema/beans
        http://www.springframework.org/schema/beans/spring-beans.xsd">
	
	<bean id="address" class="com.accenture.lkm.Address"><!--2 param - Constructor to create an object -->
		<!-- When more than one parameter is of same type then use index/name to map -->
		<constructor-arg value="Telangana" type="java.lang.String" index="1"></constructor-arg>
		<constructor-arg value="Hyderabad" type="java.lang.String" index="0"></constructor-arg>
	</bean>
	
	<bean id="employee" class="com.accenture.lkm.Employee"><!--4 param - Constructor to create an object -->
		<constructor-arg value="JAS" type="java.lang.String"></constructor-arg>
		<constructor-arg value="1001" type="java.lang.Integer"></constructor-arg>
		<constructor-arg ref="address" type="com.accenture.lkm.Address"></constructor-arg>
		<constructor-arg value="56000.0" type="java.lang.Double"></constructor-arg>
	</bean>
	
</beans>
```

#### If we have same type more than one
- So, then we can use `index` for it to metion the order

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
	xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	xsi:schemaLocation="http://www.springframework.org/schema/beans
        http://www.springframework.org/schema/beans/spring-beans.xsd">
	
	<bean id="address" class="com.accenture.lkm.Address"><!--2 param - Constructor to create an object -->
		<constructor-arg value="Hyderabad"></constructor-arg>
		<constructor-arg value="Telangana"></constructor-arg>
	</bean>
	
	<bean id="employee" class="com.accenture.lkm.Employee"><!--4 param - Constructor to create an object -->
		<constructor-arg value="JAS" index="1"></constructor-arg>
		<constructor-arg value="1001" index="0"></constructor-arg>
		<constructor-arg ref="address" index="3"></constructor-arg>
		<constructor-arg value="56000.0" index="2"></constructor-arg>
	</bean>
	
</beans>
```

#### using the exact same name mentioned in the constructor
```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
	xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	xsi:schemaLocation="http://www.springframework.org/schema/beans
        http://www.springframework.org/schema/beans/spring-beans.xsd">
	
	<bean id="address" class="com.accenture.lkm.Address"><!--2 param - Constructor to create an object -->
		<constructor-arg value="Hyderabad"></constructor-arg>
		<constructor-arg value="Telangana"></constructor-arg>
	</bean>
	
	<bean id="employee" class="com.accenture.lkm.Employee"><!--4 param - Constructor to create an object -->
		<constructor-arg value="JAS" name="employeeName"></constructor-arg>
		<constructor-arg value="1001" name="employeeId"></constructor-arg>
		<constructor-arg ref="address" name="address"></constructor-arg>
		<constructor-arg value="56000.0" name="salary"></constructor-arg>
	</bean>
	
</beans>
```
