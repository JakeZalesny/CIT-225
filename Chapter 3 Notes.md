# Notes for Chapter 3 Reading
```sql
SELECT * 
FROM language; 
```
Select indicates which columns from indicates the table 
4 Types of Tables: 

Permanent Tables: Created using the Create Table Statement

Dervied Tables: Rows returned by a subquery and held in memory, encapsulated in parentheses 

EX.: 
```sql
SELECT concat(cust.last_name, ', ', cust.first_name, full_name)
FROM (SELECT first_name, last_name, email
FROM customer
WHERE first_name = 'JESSE') cust; 
```

Temporary Tables: Volatile data held in memory, disappear at the end of every database session. 

EX.: 
```sql
CREATE TEMPORARY TABLE actors_j
(actor_id smallint(5), 
first_name varchar(45), 
last_name varchar(45)); 
```

Virtual Tables(view): Created using the "CREATE VIEW" statement. Looks and acts like a table but has no data associated with it. mainly used to hid columns from users and simplify complex database designs. 

EX.: 
```sql 
CREATE VIEW cust_vw AS 
SELECT customer_id, first_name, last_name, active
FROM customer; 
```

# Joins & Connecting Tables: 

```sql 
SELECT customer.first_name, customer.last_name, time(rental.rental_date) rental.rental_time
FROM customer c
INNER JOIN rental r
ON c.rental_id = r.rental_id
GROUP BY c.first_name, c.last_name
HAVING count(*) >= 40
WHERE date(rental.rental_date) = '2005-06-14'
ORDER BY c.last_name;  
```
* The ON clause is where the two tables are linked and they're linked by foreign keys!
* The c and the r are Aliases, They're used to avoid confusion and redundancy. 
* The WHERE clause is where we specify criteria to view the desired data. 
* GROUP BY groups data by column values, HAVING is specifically used with GROUP BY and functions similarly to WHERE, (Dont' use WHERE with GROUP BY)
* ORDER BY lets you sort the given rows by column data. 

# Data Modeling Theory: 
Scalar Data Type: 

Scalar data types hold one thing among a set of like things. This means a scalar
data type could apply to integers, real numbers, characters, dates, or date-time
values. The Java programming language labels scalar data types as primitives.

Each column holds one fact. 

Why is Data Modeling important: 
* Natural keys should always define unique rows in a table 
* Nonunique keys (like foreign keys) should always access a list of primary keys from another table or copy of the same table 
* Foreign keys should always reference the right primary key

Subject Facts: 

* Always choose the surrogate key, when it maps to one natural key value. 
* Surrogate keys are the auto incrementing ones, it means that those numbers aren't descriptive of anything in the data. 
* Using Unsigned Ints are the best to use because you can use double the amount of integers than you can with a signed int. 
* Internal Identification columns are the natural key
* Natural keys are rows that contain unique instances for the, essentially it's anything that you would typically use in a WHERE clause 
* Something like First and Last name would be an example of a natural composite key

^ These don't guarantee uniqueness, so it's still a problem. 
* Non-Key Data, is data that doesnt describe the natural keys 
* ER models are essential to supporting businesses with Databases 
* Profit & Loss Management manager = Scrum Manager

# Data Relations: 
* One to One Relationship: One entity to another entity
* One to Many Relationship: One entity can have many other entities 
* Many to Many Relationship: Either entity can have multiple of the others. 
* Never design a column that may hold a foreign key
that belongs to one of two or more tables because it
invariably leads to confusion and usage errors.
* Chen notation is another way of doing an ERD essentially 
* UML Notation is object oriented. 

# Normalization: 
There are 8 Normal Forms. 

1. First normal form (1NF) exists when all columns, or attributes, of a table have a
single data type and there are no repeating rows of data, which means rows are
unique. 

2. Second normal form (2NF) exists when a table is already in first normal form and all
non-key columns depend on all of the key columns, where the list of key columns
goes from 1 to n. The list of key columns makes up the natural key of a table, or the
list of columns that makes any row unique in a table. A table that has only a single
column as the natural key, like a vehicle identification number, is automatically in
second normal form because there can’t be a dependency on part of the key (or one
column of a multiple column key).

3. Third normal form (3NF) exists when a table is already in second normal form and
there are no transitive dependencies. A transitive dependency means a non-key
column or set of columns is dependent on another column that generally isn’t part
of the natural key. (A non-key relying on a non-key)