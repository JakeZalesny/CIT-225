# Agregate Functions
* It's too hard to read raw data without doing any filtering and without any grouping. 

EX. 
```sql 
SELECT customer_id, count(*)
    -> FROM rental
    -> GROUP BY customer_id;
```
* Count will get how many rows are in each group. 

Ordering EX: 
```sql
SELECT customer_id, count(*)
    -> FROM rental
    -> GROUP BY customer_id
    -> ORDER BY 2 DESC;
```

* If you want to filter by the count you have to use the 'HAVING' clause instead of 'WHERE' 


### Grouping functions: 
max()

min()

sum()

count()

avg()

* be sure to do explicit grouping, AKA specify the alias that each aggregate function will take. 
* 'DISTINCT keyword can be used in aggregate functions
