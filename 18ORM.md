## In JDBC what we do?
- If we use `mySQL driver` we can connect with `mySQL` database
- If we use `JDBC` then migration to `another driver` is hard.
- We need to do lot of effort to make the connection. We need to write lot of repetative steps for every opperation.
- Step 1: Register the driver
- Step 2: Create a connection
- Step 3: write a query
- Step 4: Execute the query
- Step 5: Close the connection
- When we save the data in database through this process we store it in `cells` not in a `row` directly. We need to deal with each and every cell.
- We are store cell by cell data when we use JDBC.
- Now the solution of this is ORM.

## How we can intrigrate ORM in Spring
- ORM (Object Relational Mapping) --> Mapping Object to Table
- The object I have we can directly store it in the table.
- Store the Object directly in a row

### ORM Tools:
- Hibernate
- TopLink
- MyBatis
- Django

## JPA
- JPA (Java Persistance API)
- It is not a framework, its an `library` on the top of other `ORM framework`
- When we want to `migrate` from one orm tool to another then their will be some problem.
- Hibernate use `save` method to store the data and other tool can use different method for its.
- So for migration we need to change the code many time.
- So java create `an API implementation` for **all the ORM tool**. It will be present in the top of every ORM tools. And inside that API there will be certain rule which we need to use.
- So if we use `JPA` then for migration we need to change the jar only.

## Step 1:
Add jar files

![image](https://github.com/user-attachments/assets/3cc43d06-fe0d-4ab8-a15b-91ee5d919a4d)
![image](https://github.com/user-attachments/assets/ae9e75b0-3f83-4987-8864-9c42113923ac)

## Step 2:
- Create a EmployeeBean
![image](https://github.com/user-attachments/assets/c26ce04d-ca47-4f54-a9ce-2d6b7a9e1688)

here we are using `layered archetecture` 

## Step 3:
- Create a client `UITester` ---> intaract with `Service layer`
- main method:

![image](https://github.com/user-attachments/assets/e1b0daee-8e23-42b6-9433-b3bcab73da8f)

- add method:

![image](https://github.com/user-attachments/assets/66ac6c7a-de5c-45ce-a8ab-1de171915ad9)

## Step 4:
Create the service layer
- EmployeeService.java

![image](https://github.com/user-attachments/assets/bc12acab-d191-4086-bc02-429c0b80334b)

- EmployeeServiceImpl.java

![image](https://github.com/user-attachments/assets/ab4b6cdd-159b-4a3a-babe-c994edfe8ec3)

## Step 5:
- Service layer will communicate with DAO (Data Access Object) --> Here we convert the getting Bean into `Entity (Persistable Java class)` which we use to store the data in database.
- It will be looking same as Bean but some changes are there.
- EmployeeDAO.java

![image](https://github.com/user-attachments/assets/a5677559-415b-49a7-b085-d8fa73a51247)

- EmployeeDAOImpl.java

![image](https://github.com/user-attachments/assets/11fbff21-667e-4dcc-b93f-4ecb6d7672cf)

## Step 6:
Create entity:
![image](https://github.com/user-attachments/assets/b15a3f8c-89b9-4965-9dd2-26712a59a6cb)

## Step 7:
- We create the container to handle all the `object` creation.
### 1st configuration file:
```xml
<beans xmlns="http://www.springframework.org/schema/beans"
	xmlns:context="http://www.springframework.org/schema/context"
	xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	xsi:schemaLocation="
        http://www.springframework.org/schema/beans     
        http://www.springframework.org/schema/beans/spring-beans.xsd
        http://www.springframework.org/schema/context 
        http://www.springframework.org/schema/context/spring-context.xsd">

    <!-- scanning service and DAO -->
	<context:component-scan base-package="com.accenture.lkm.service, com.accenture.lkm.dao" />
	
	<!-- including the JPA spring integration configuration -->
 	<import resource="cst_jpa_spring_config.xml"/>
</beans>
```

### 2nd configuration file for database connection cst_jpa_spring_config.xml
```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
	xmlns:context="http://www.springframework.org/schema/context"
	xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	xmlns:aop="http://www.springframework.org/schema/aop"
	xsi:schemaLocation="
        http://www.springframework.org/schema/beans     
        http://www.springframework.org/schema/beans/spring-beans.xsd
        http://www.springframework.org/schema/context 
        http://www.springframework.org/schema/context/spring-context.xsd
        http://www.springframework.org/schema/aop http://www.springframework.org/schema/aop/spring-aop.xsd">
   
    <bean class="org.springframework.beans.factory.config.PropertyPlaceholderConfigurer">
		<property name="location">
			<value>classpath:com/accenture/lkm/resources/cst_conn.properties</value> <!-- class path is requried when runing from web app -->
		</property>
	</bean>
  
    <bean id="cst_DataSource" class="org.springframework.jdbc.datasource.DriverManagerDataSource" >
        <property name="driverClassName" value="${cst_db_driver}" />
        <property name="url" value="${cst_db_url}" />
        <property name="username" value="${cst_user}" />
        <property name="password" value="${cst_password}" />
    </bean>
 
 
	<!-- entity manager factory -->
    <bean id="cst_entityManagerFactory"	class="org.springframework.orm.jpa.LocalContainerEntityManagerFactoryBean">
		<property name="dataSource" ref="cst_DataSource" />
		<property name="jpaVendorAdapter">
			<bean class="org.springframework.orm.jpa.vendor.HibernateJpaVendorAdapter">
				<property name="showSql" value="true" />
				<property name="generateDdl" value="true" />
				<property name="databasePlatform" value="org.hibernate.dialect.MySQLDialect" />
			</bean>
		</property>
		<property name="packagesToScan" value="com.accenture.lkm.entity"></property>
	</bean>
	
</beans>
```

### 3rd the property file cst_conn.properties
```properties
cst_db_driver=com.mysql.cj.jdbc.Driver
cst_db_url=jdbc:mysql://localhost:3306/springormdemos
cst_user=root
cst_password=root
```
## The connection and convertion
- we can directly autowired the EntityManagerFactory because the bean has been already present in the xml file:
```java
@Autowired
	private EntityManagerFactory entityManagerFactory;
```
```xml
    <bean id="cst_entityManagerFactory"	class="org.springframework.orm.jpa.LocalContainerEntityManagerFactoryBean">
```

- convert bean to entity
```java
public static EmployeeEntity convertBeanToEntity(EmployeeBean bean) {
		EmployeeEntity employeeEntityBean = new EmployeeEntity();
		BeanUtils.copyProperties(bean, employeeEntityBean);
		return employeeEntityBean;
	}
```

- convert entity to bean
```java
public static EmployeeBean convertEntityToBean(EmployeeEntity entity) {
		EmployeeBean employee = new EmployeeBean();
		BeanUtils.copyProperties(entity, employee);
		return employee;
	}
```

## Instead of XML we can do java configuration:
- java main configuration

```java
package com.accenture.lkm.spring.mainconf;

import org.springframework.context.annotation.ComponentScan;
import org.springframework.context.annotation.Configuration;
import org.springframework.context.annotation.Import;

import com.accenture.lkm.db.config.SpringDBConfig;

@Configuration
@ComponentScan({"com.accenture.lkm.dao","com.accenture.lkm.service"})
@Import(SpringDBConfig.class)
public class SpringMainConfig {

}
```

- java db configuration

```java
package com.accenture.lkm.db.config;

import org.springframework.beans.factory.annotation.Value;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;
import org.springframework.context.annotation.PropertySource;
import org.springframework.context.support.PropertySourcesPlaceholderConfigurer;
import org.springframework.jdbc.datasource.DriverManagerDataSource;
import org.springframework.orm.jpa.LocalContainerEntityManagerFactoryBean;
import org.springframework.orm.jpa.vendor.HibernateJpaVendorAdapter;

@Configuration
@PropertySource("classpath:com/accenture/lkm/resources/cst_conn.properties")
public class SpringDBConfig {
	
	// reading value from properties file and giving to the datasource
	@Value("${cst_db_driver}")
	private String driverName;
	
	@Value("${cst_db_url}")
	private String url;
	
	@Value("${cst_user}")
	private String userName;
	
	@Value("${cst_password}")
	private String password;
	
	//To resolve ${} in @Value
	@Bean
	public static PropertySourcesPlaceholderConfigurer propertyConfigInDev() {
			return new PropertySourcesPlaceholderConfigurer();
	}

	//Data source can have any name
	@Bean(name = "cst_DataSource")
	public DriverManagerDataSource getDataSource() {
		DriverManagerDataSource dataSource = new DriverManagerDataSource();
		dataSource.setDriverClassName(driverName);
		dataSource.setUrl(url);
		dataSource.setUsername(userName);
		dataSource.setPassword(password);

		return dataSource;
	}

	public HibernateJpaVendorAdapter getVendorAdaptor(){
		HibernateJpaVendorAdapter adapter = new  HibernateJpaVendorAdapter();
		adapter.setShowSql(true);
		adapter.setGenerateDdl(false);
		adapter.setDatabasePlatform("org.hibernate.dialect.MySQLDialect");
		return adapter;
	}

	@Bean(name = "cst_entityManagerFactory") //can have any name
	public LocalContainerEntityManagerFactoryBean getEntityManagerFactory(DriverManagerDataSource dataSource) {
		LocalContainerEntityManagerFactoryBean factoryBuilder = new LocalContainerEntityManagerFactoryBean();
		factoryBuilder.setDataSource(dataSource);
		factoryBuilder.setJpaVendorAdapter(getVendorAdaptor());
		factoryBuilder.setPackagesToScan("com.accenture.lkm.entity");
		return factoryBuilder;
	}
	
}
```












