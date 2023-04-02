# Transactions

### Locking: 
- Locks are used by the server to control the simultaneous use of data resources. 
- If data is locked by one user, others will have to wait to use it. 

### Strategy 1: 
- Writers receive a write lock to modify data
- Readers get a read lock to read the data. 

### Strategy 2: 
- Writers get a write lock to modify data
- No read lock 
- This is called Versioning

## Lock Granularities: 
### Table Locks: 
- Keep users from modifying data in the same table at the same time. 
### Page Locks: 
- Keep multiple users from modifying data on the same page of a table the same tiem. 
### Row Locks: 
- Keeps multiple users from modifying the same row of a table at the same time.

## Transactions: 
-  A transaction is a mechanism that groups multiple statements.
- Either all Transaction statements succeed or none of them do.
EX:  
```sql 
START TRANSACTION;
/* withdraw money from first account, making sure balance is sufficient */
UPDATE account SET avail_balance = avail_balance – 500
WHERE account_id = 9988
AND avail_balance > 500
IF <exactly one row was updated by the previous statement> THEN
/* deposit money into second account */
UPDATE account SET avail_balance = avail_balance + 500
WHERE account_id = 9989
IF <exactly one row was updated by the previous statement> THEN
/* everything workd, make the changes permanent */
COMMIT;
ELSE
/* something went wrong, undo all changes in this transaction */
ROLLBACK;
END IF;
ELSE
/* insufficient funds, or error encountered during update */
ROLLBACK;
END IF;
```

- Once a transaction is either committed or rolled back, the write locks are released, making data available for other transactions
- If the server goes down mid update, the transaction will be rolled back by default. 
- There are two different ways to start transaction, different servers use different approaches. 

- An active transaction is always associated with a database session. Because of this we do not need to explicitly start a transaction. Once a transaction is complete, the server will automatically begin for a the session. (Use with Oracle Database.)
- Unless we explicitly start a transaction, individual SQL statements will be processed
independently. If we want a transaction, we must send a command to the server. (Use
with SQL and MySQL.)
An advantage of the first approach is that we can roll back changes. With the second
approach, changes are permanent.
- Use either "START TRANSACTION" or "BEGIN TRANSACTION" 
- Autocommit mode 
- Starting a transaction before one ends will cause the previous transaction to end. 
