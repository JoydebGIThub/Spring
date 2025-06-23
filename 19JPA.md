# JPA Transaction management

So, for previous process we need to start the `transaction again and close again` for every process
```java
public Integer addEmployee(EmployeeBean employeeBean) throws Exception {
		Integer employeeID = 0;
		EntityManager entityManager = null;

		EmployeeEntity employeeEntityBean = convertBeanToEntity(employeeBean);
		try {
			entityManager = entityManagerFactory.createEntityManager();
			entityManager.getTransaction().begin();
			entityManager.persist(employeeEntityBean);
			entityManager.getTransaction().commit();
			employeeID = employeeEntityBean.getId();
		} catch (Exception exception) {
			throw exception;
		} finally {
			if (entityManager != null) {
				entityManager.close();
			}
		}

		return employeeID;
	}

public EmployeeBean updateEmployeeDetails(EmployeeBean employeeBean) throws Exception {
		EmployeeBean employeeBean2 = null;
		EntityManager entityManager = null;
		try {
			entityManager = entityManagerFactory.createEntityManager();
			EmployeeEntity employeeEntityBean2 = entityManager.find(EmployeeEntity.class, employeeBean.getId());
			if (employeeEntityBean2 != null){
				entityManager.getTransaction().begin();
				employeeEntityBean2.setSalary(employeeBean.getSalary());
				employeeBean2 = convertEntityToBean(employeeEntityBean2);
				entityManager.getTransaction().commit();
			}

		} catch (Exception exception) {
			throw exception;
		} finally {
			if (entityManager != null) {
				entityManager.close();
			}
		}

		return employeeBean2;
	}
```

## To start and stop the transaction manually we can add some part in container
- cst_jpa_spring_config.xml
- add a new schema and annotation-driven

```xml
        http://www.springframework.org/schema/tx http://www.springframework.org/schema/tx/spring-tx.xsd">

<tx:annotation-driven transaction-manager="txManager"/>	
   
    <bean id="txManager" class="org.springframework.orm.jpa.JpaTransactionManager"> <!--  standard name for transactionManager is transactionManager-->
		<property name="entityManagerFactory" ref="cst_entityManagerFactory" />
	</bean>
```

- EmployeeDAOImpl.java
- We need to add `@Transactional(value = "txManager")` to autowired the transaction
- The @PersistenceContext annotation is used to inject a JPA EntityManager into a Spring-managed bean (usually in the repository or service layer).
- It tells Spring to automatically inject an EntityManager that is:
  - Managed by the container
  - Bound to the current transaction context
This allows you to interact with the database in a transactional and thread-safe way.

```java
package com.accenture.lkm.dao;

import java.util.ArrayList;
import java.util.List;

import javax.persistence.EntityManager;
import javax.persistence.PersistenceContext;

import org.springframework.beans.BeanUtils;
import org.springframework.stereotype.Repository;
import org.springframework.transaction.annotation.Transactional;

import com.accenture.lkm.business.bean.EmployeeBean;
import com.accenture.lkm.entity.EmployeeEntity;

@Repository
@SuppressWarnings("unchecked")
@Transactional(value = "txManager")
public class EmployeeDAOImpl implements EmployeeDAO {

	@PersistenceContext
	private EntityManager entityManager;

	public Integer addEmployee(EmployeeBean employeeBean) throws Exception {
		Integer employeeID = 0;

		EmployeeEntity employeeEntityBean = convertBeanToEntity(employeeBean);
		try {

			entityManager.persist(employeeEntityBean);

			employeeID = employeeEntityBean.getId();
		} catch (Exception exception) {
			throw exception;
		}
		return employeeID;
	}
````

## For Java configuration
```java
package com.accenture.lkm.db.config;

import javax.persistence.EntityManagerFactory;

import org.springframework.beans.factory.annotation.Value;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;
import org.springframework.context.annotation.PropertySource;
import org.springframework.context.support.PropertySourcesPlaceholderConfigurer;
import org.springframework.jdbc.datasource.DriverManagerDataSource;
import org.springframework.orm.jpa.JpaTransactionManager;
import org.springframework.orm.jpa.LocalContainerEntityManagerFactoryBean;
import org.springframework.orm.jpa.vendor.HibernateJpaVendorAdapter;
import org.springframework.transaction.annotation.EnableTransactionManagement;

@Configuration
@PropertySource("classpath:com/accenture/lkm/resources/cst_conn.properties")
@EnableTransactionManagement// equivalent to <tx:annotation-driven>
public class SpringDBConfig {
@Bean(name = "txManager")
	public JpaTransactionManager getTransactionManager(EntityManagerFactory entityManagerFactory) {
		JpaTransactionManager transactionManager = new JpaTransactionManager(entityManagerFactory);
		return transactionManager;
	}
}
```

# Transaction Management Context:
## Context area
whenever we are performing any operation, any object or entity will be stored in a `Persistence Context area` will direct connection with the database. Means whatever thing i am access from the database which will already avaiable with in `context area`, either we access the data from database of the context area both will be the same thing. But the change is when we try to access the data from database we will make a `transaction on database`. If we access different thing everytime then its ok to access it from database but if we perform any frequent opperation for same data so instade of the database we can access the data from `context area`. It is kind of small cache. We don't have control over this cache its a small memory area. If we want to access an object frequently we can directly access from that perticular context area.
So, for this we can use `@PersistenceContext`
It has 2 scope:
## Type of Context:
- Transaction (By default scope)
- Extended

EntityManager is responsible for managing the entity. We don't need to create the object of EntityManagerFactory to access the entity we can use `@PersistenceContext` for that. When we use `TRANSACTION` scope it will hit the `database again and again`. Its by default. When we use `EXTENDED` scope it will hit the database for `one` time for same data. **It will reduce the hit to the database**
```java
@Repository
@Transactional(value = "txManager")
public class EmployeeDAOImpl implements EmployeeDAO {

	@PersistenceContext
	// @PersistenceContext(type = PersistenceContextType.TRANSACTION)
	// @PersistenceContext(type = PersistenceContextType.EXTENDED)
	private EntityManager entityManager;

	public EmployeeBean getEmployeeDetails(Integer id) throws Exception {
		EmployeeBean employeeBean = null;

		try {

			EmployeeEntity employeeEntity = entityManager.find(EmployeeEntity.class, id);

			if (employeeEntity != null) {
				employeeBean = convertEntityToBean(employeeEntity);
			}

		} catch (Exception exception) {

			throw exception;
		}

		return employeeBean;
	}

public static EmployeeBean convertEntityToBean(EmployeeEntity entity) {
		EmployeeBean employee = new EmployeeBean();
		BeanUtils.copyProperties(entity, employee);
		return employee;
	}
}
```

# Transaction mode
There in some conditions where we don't want to provide `read` or `write` access to any of the methods. Like if we want to give the access to see the data not to change it that can ge given by `read only` access. We can manage the transaction on `class level or method level`. 
## Method level transaction
- `readOnly=true` for this we are unable to add the employee


```java
@Repository
public class EmployeeDAOImpl implements EmployeeDAO {
	
	@PersistenceContext
	private EntityManager entityManager;
	
	@Transactional(value="txManager",readOnly=true)
	public Integer addEmployee(EmployeeBean employeeBean) throws Exception{
		Integer employeeID = 0;
		
		EmployeeEntity employeeEntityBean =convertBeanToEntity(employeeBean);
		try {
			entityManager.persist(employeeEntityBean);	
			employeeID = employeeEntityBean.getId();
		} catch (Exception exception) {
			throw exception;
		}
		return employeeID;
	}
	
	public static EmployeeEntity convertBeanToEntity(EmployeeBean bean){
		EmployeeEntity employeeEntityBean = new EmployeeEntity();
		BeanUtils.copyProperties(bean,employeeEntityBean);
		return employeeEntityBean;
	}
}
```

## @PersistenceContext(type = PersistenceContextType.TRANSACTION) VS @PersistenceContext(type = PersistenceContextType.EXTENDED) ?
### @PersistenceContext(type = PersistenceContextType.TRANSACTION) (Default):
This is the default behavior and the most commonly used type.
The persistence context is tied to the lifecycle of a single transaction.
It's created when the transaction starts and is closed/cleared when the transaction ends (commit or rollback).
#### Benefits:
More memory efficient as entities are cleared after each transaction.
Ensures data consistency within a single transaction.

### @PersistenceContext(type = PersistenceContextType.EXTENDED):
The persistence context exists across multiple transactions until the bean itself is destroyed.
#### Benefits:
Enables building a "conversation" with the client where data can be accumulated across method calls within the bean's lifecycle.
Useful for scenarios where data needs to be persisted at the end of a user interaction rather than within each individual method call.

![image](https://github.com/user-attachments/assets/b64baf21-365d-4600-995f-82dd7964bec7)


# Transaction Time out
It will help us in the case of `OTP`. We want the transaction should be stop within 10 min if the OTP is not conformed. Timeout always be in `secounds`

```java
@Repository
public class EmployeeDAOImpl implements EmployeeDAO {
	
	@PersistenceContext
	private EntityManager entityManager;
	
	
	@Transactional(value="txManager",timeout=3)
	public Integer addEmployee(EmployeeBean employeeBean) throws Exception{
		Integer employeeID = 0;
		EmployeeEntity employeeEntityBean =convertBeanToEntity(employeeBean);
		try {	
			// following wait simulates the wait time
			// to hit the database, it is five seconds, more than the value fixed
			// for timeout parameter of @Transactional
			Thread.sleep(5000);
			entityManager.persist(employeeEntityBean);	
			employeeID = employeeEntityBean.getId();
		} catch (Exception exception) {
			throw exception;
		}
		return employeeID;
	}
	
	public static EmployeeEntity convertBeanToEntity(EmployeeBean bean){
		EmployeeEntity employeeEntityBean = new EmployeeEntity();
		BeanUtils.copyProperties(bean,employeeEntityBean);
		return employeeEntityBean;
	}
}
```




