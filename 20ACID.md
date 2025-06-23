# What is ACID properties of DB?
ACID stands for Atomicity, Consistency, Isolation, and Durability. These are four fundamental properties that most relational databases offer to ensure the integrity and reliability of data during transactions.
Here's a breakdown of each property:
## Atomicity:
All operations within a transaction are treated as a single unit.
Either all operations succeed completely, or the entire transaction is rolled back, leaving the database in its original state.
This prevents partial updates or inconsistencies from occurring.
## Consistency:
A transaction must bring the database from one valid state to another.
This means the data adheres to all defined constraints and rules within the database schema.
For example, foreign key relationships must remain valid after the transaction completes.
## Isolation:
Concurrent transactions are isolated from each other, meaning they cannot see or interfere with each other's modifications.
This ensures that the outcome of a transaction is independent of other transactions happening at the same time.

```java
@PersistenceContext
	private EntityManager entityManager;

	@Transactional(value = "txManager", isolation = Isolation.READ_COMMITTED)
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
```

```java
@Repository
public class EmployeeDAOImpl implements EmployeeDAO {

	@PersistenceContext
	private EntityManager entityManager;

	@Transactional(value = "txManager", isolation = Isolation.READ_UNCOMMITTED)
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
}
```

## Durability:
Once a transaction is committed, the changes are permanently persisted to the database storage.
Even if a system failure occurs after the commit, the changes are not lost and remain accessible.
These ACID properties work together to guarantee data integrity and prevent inconsistencies within the database.

# How to have AUTOMICITY implemented in Spring ORM?
Ans: Use Transaction Propagation attribute of Transactional Annotation
Req: Create a New Department and Create a New Employee for that Department if both are successful then commit the whole transaction if any one fail, then rollback the entire transaction.
Propagation types:
## @Transactional(propagation = Propagation.REQUIRED) : 
•	This is the default propagation behavior of the @Transactional annotation.
•	If there is already an active transaction when the annotated method is called, the method joins that existing transaction.
•	If no active transaction exists, Spring automatically creates a new transaction for the method's execution.
•	Any changes made within the method/methods are committed to the database if all the methods complete successfully.
•	If an exception occurs during the execution, the entire transaction (including changes made in other methods within the same transaction) is rolled back, ensuring data integrity.
Note: Transaction will rollback automatically only for UnChecked/Runtime exceptions. Transaction will not rollback by default for Checked Exceptions 

- If I use the `Transactional` in one method then I don't need to use it in another method beacuse it is `propogaterd` from service layer to dao layer. Beacuse both method are shareing the same transaction
- DAO transaction:

```java
@PersistenceContext
	private EntityManager entityManager;
	
	@Transactional(value = "txManager")
	@Transactional(propagation = Propagation.REQUIRED)
	public Integer addEmployee(EmployeeBean employeeBean) throws Exception{
		Integer employeeID = 0;
		EmployeeEntity employeeEntityBean =convertEmployeeBeanToEntity(employeeBean);
		try {
			entityManager.persist(employeeEntityBean);
			employeeID = employeeEntityBean.getId();
		} catch (Exception exception) {
			throw exception;
		}
		return employeeID;
	}
	
	//@Transactional(value = "txManager")
	//@Transactional(propagation = Propagation.REQUIRED)
	public Integer addDepartment(DepartmentBean departmentBean)throws Exception {
		Integer departmentCode = 0;
		DepartmentEntity departmentEntityBean =convertDepartmentBeanToEntity(departmentBean);
		try {
			entityManager.persist(departmentEntityBean);
			departmentCode = departmentEntityBean.getDepartmentCode();
		} catch (Exception exception) {
			throw exception;
		}
		return departmentCode;
	}
```

- Service layer Transaction

```java
package com.accenture.lkm.service;

import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Service;
import org.springframework.transaction.annotation.Transactional;

import com.accenture.lkm.business.bean.DepartmentBean;
import com.accenture.lkm.business.bean.EmployeeBean;
import com.accenture.lkm.dao.EmployeeDepartmentDAO;

@Service
public class EmployeeDepartmentServiceImpl implements EmployeeDepartmentService {

	@Autowired
	private EmployeeDepartmentDAO employeeDepartmentDAO;
	
	@Transactional(value = "txManager")
	public Integer addEmployeeAndDepartment(EmployeeBean employeeBean,DepartmentBean departmentBean) throws Exception {
		
		int result = 0;
		try{
		int deptId=employeeDepartmentDAO.addDepartment(departmentBean);
						employeeBean.setDepartmentCode(deptId);
		result = deptId+employeeDepartmentDAO.addEmployee(employeeBean);
		
		}catch(Exception e){
			throw e;
		}
		return result;
	}

}
```

- DAO class

```java
@PersistenceContext
	private EntityManager entityManager;

	public Integer addEmployee(EmployeeBean employeeBean) throws Exception {
		Integer employeeID = 0;
		EmployeeEntity employeeEntityBean = convertEmployeeBeanToEntity(employeeBean);
		try {
			entityManager.persist(employeeEntityBean);
			dummyCodeThrowingRunTimeException(); // code to throw runtime exception
			employeeID = employeeEntityBean.getId();
		} catch (Exception exception) {
			throw exception;
		}
		return employeeID;
	}

	public Integer addDepartment(DepartmentBean departmentBean) throws Exception {
		Integer departmentCode = 0;
		DepartmentEntity departmentEntityBean = convertDepartmentBeanToEntity(departmentBean);
		try {
			entityManager.persist(departmentEntityBean);
			departmentCode = departmentEntityBean.getDepartmentCode();
		} catch (Exception exception) {
			throw exception;
		}
		return departmentCode;
	}

	public void dummyCodeThrowingRunTimeException() {
		throw new RuntimeException("This Dummy Unchecked/RuntimeException");
	}
```

### Logical Transaction and Physical Transaction
- Which we manage on DAO layer are called **Physical Transcation**
- Which we manage on Service layer are called **Logical Transcation**

```java
@Service
public class EmployeeDepartmentServiceImpl implements EmployeeDepartmentService {

	@Autowired
	private EmployeeDepartmentDAO employeeDepartmentDAO;
	
	@Transactional(value = "txManager")
	public Integer addEmployeeAndDepartment(EmployeeBean employeeBean,DepartmentBean departmentBean) throws Exception {
		
		int result = 0;
		try{
		int deptId=employeeDepartmentDAO.addDepartment(departmentBean);
						employeeBean.setDepartmentCode(deptId);
		result = deptId+employeeDepartmentDAO.addEmployee(employeeBean);
		
		}catch(Exception e){
			throw e;
		}
		return result;
	}

}

//default propagation is Required @Transactional(value = "txManager",propagation=Propagation.REQUIRED)
//Physical transaction and logical transaction share the same transactional scope
```

```java
@PersistenceContext
	private EntityManager entityManager;
	
	public Integer addEmployee(EmployeeBean employeeBean) throws Exception{
		Integer employeeID = 0;
		
		EmployeeEntity employeeEntityBean =convertEmployeeBeanToEntity(employeeBean);
		try {
			entityManager.persist(employeeEntityBean);
			dummyCodeThrowingCheckedException();
			employeeID = employeeEntityBean.getId();
		} catch (Exception exception) {
			throw exception;
		}
		return employeeID;
	}
	

	
	public Integer addDepartment(DepartmentBean departmentBean)
			throws Exception {
		Integer departmentCode = 0;
		
		DepartmentEntity departmentEntityBean =convertDepartmentBeanToEntity(departmentBean);
		try {
				entityManager.persist(departmentEntityBean);
				
			departmentCode = departmentEntityBean.getDepartmentCode();
		} catch (Exception exception) {
			throw exception;
		}
		return departmentCode;
	}
	
	
	public void dummyCodeThrowingCheckedException() throws InvalidDummyException{
		throw new InvalidDummyException();
	}
```

```java
package com.accenture.lkm.exceptions;

@SuppressWarnings("serial")
public class InvalidDummyException extends Exception {
	public InvalidDummyException() {
		super("It is a Dummy checked exception");
	}
}
```
### Handle the rollback condition in Physical Transaction
```java
@PersistenceContext
	private EntityManager entityManager;
	
	//added just to give strategy as rollback for the InvalidDummyException
	@Transactional(value="txManager",rollbackFor=InvalidDummyException.class)
	public Integer addEmployee(EmployeeBean employeeBean) throws Exception{
		Integer employeeID = 0;
		
		EmployeeEntity employeeEntityBean =convertEmployeeBeanToEntity(employeeBean);
		try {
				entityManager.persist(employeeEntityBean);
				dummyCodeThrowingCheckedException();
			employeeID = employeeEntityBean.getId();
		} catch (Exception exception) {
			throw exception;
		}
		return employeeID;
	}
	
	public Integer addDepartment(DepartmentBean departmentBean)
			throws Exception {
		Integer departmentCode = 0;
		
		DepartmentEntity departmentEntityBean =convertDepartmentBeanToEntity(departmentBean);
		try {
				entityManager.persist(departmentEntityBean);
				
			departmentCode = departmentEntityBean.getDepartmentCode();
		} catch (Exception exception) {
			throw exception;
		}
		return departmentCode;
	}
	
	
	public void dummyCodeThrowingCheckedException() throws InvalidDummyException{
		throw new InvalidDummyException();
	}
```


## @Transactional(propagation = Propagation.REQUIRES_NEW)
The @Transactional(propagation = Propagation.REQUIRES_NEW) annotation in Spring ORM is used to specify a specific transaction propagation behavior for a method. Here's what it does:
### Behavior:
When applied to a method, it ensures a new, independent transaction is created regardless of whether an existing transaction is already active.
The method's operations are executed within this newly created transaction.
The outcome (commit or rollback) of the inner transaction does not affect the outcome of the outer transaction (if one exists).
- Create a indipendent Transaction. It will not depending on the previous transaction. If we get error on the addEmployee it will not stop it will store that data and complete the transcation

```java
@PersistenceContext
	private EntityManager entityManager;

	// added just to give strategy as rollback for the InvalidDummyException
	@Transactional(value = "txManager", rollbackFor = InvalidDummyException.class)
	public Integer addEmployee(EmployeeBean employeeBean) throws Exception {
		Integer employeeID = 0;

		EmployeeEntity employeeEntityBean = convertEmployeeBeanToEntity(employeeBean);
		try {
			entityManager.persist(employeeEntityBean);
			dummyCodeThrowingCheckedException();
			employeeID = employeeEntityBean.getId();
		} catch (Exception exception) {
			throw exception;
		}
		return employeeID;
	}

	// added to run the following in a independent logical transaction scope
	@Transactional(value = "txManager", propagation = Propagation.REQUIRES_NEW)
	public Integer addDepartment(DepartmentBean departmentBean) throws Exception {
		Integer departmentCode = 0;

		DepartmentEntity departmentEntityBean = convertDepartmentBeanToEntity(departmentBean);
		try {
			entityManager.persist(departmentEntityBean);

			departmentCode = departmentEntityBean.getDepartmentCode();
		} catch (Exception exception) {
			throw exception;
		}
		return departmentCode;
	}

	public void dummyCodeThrowingCheckedException() throws InvalidDummyException {
		throw new InvalidDummyException();
	}
```

```java
@Autowired
	private EmployeeDepartmentDAO employeeDepartmentDAO;
	@Transactional(value="txManager",rollbackFor=InvalidDummyException.class)
	public Integer addEmployeeAndDepartment(EmployeeBean employeeBean,DepartmentBean departmentBean) throws Exception {
		
		int result = 0;
		try{
		int deptId=employeeDepartmentDAO.addDepartment(departmentBean);
						employeeBean.setDepartmentCode(deptId);
		result = deptId+employeeDepartmentDAO.addEmployee(employeeBean);
		
		}catch(Exception e){
			throw e;
		}
		return result;
	}
```
