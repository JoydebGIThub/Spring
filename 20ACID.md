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
## @Transactional(propagation = Propagation.REQUIRES_NEW)
The @Transactional(propagation = Propagation.REQUIRES_NEW) annotation in Spring ORM is used to specify a specific transaction propagation behavior for a method. Here's what it does:
### Behavior:
When applied to a method, it ensures a new, independent transaction is created regardless of whether an existing transaction is already active.
The method's operations are executed within this newly created transaction.
The outcome (commit or rollback) of the inner transaction does not affect the outcome of the outer transaction (if one exists).
