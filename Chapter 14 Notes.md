# Views
- A view is a mechanism for querying data. 

Syntax Example: 
```sql 
CREATE VIEW customer_vw
(customer_id,
first_name,
last_name,
email
)
AS
SELECT
customer_id,
first_name,
last_name,
concat(substr(email,1,2), '*****', substr (email, -4)) email
FROM customer;
```

- It's essentially saving a SELECT statement
- Any clauses of the SELECT statement can be used. 
- Views are really good for security because they limit what actually can be seen. 
- You can actually uses Views to create aggregates to query later if you'd like, rather than querying the whole table. 
- Partition data as well. 

- You can't use aggregate functions on views. 
- No group by or having clauses 
- No subqueries 
- No UNION or DISTINCT 
- FROM has an updatable table 
- Only INNER JOINs 
