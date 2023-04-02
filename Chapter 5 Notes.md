# Cartesian Products and Cross Joins: 
* A query needs to describe how the tables are related in order for a join to be performed.

Ex. 
```sql 
SELECT c.first_name, c.last_name, a.address
FROM customer c JOIN address a
ON c.address_id = a.address_id; 
```
^ That's an INNER JOIN 

* Get in the habit of specifying joins. 

```sql
SELECT c.first_name, c.last_name, a.address
FROM customer c INNER JOIN address a
ON c.address_id = a.address_id;
```

### Join 3 or more tables: 

```sql 
SELECT c.first_name, c.last_name, ct.city
FROM customer c
INNER JOIN address a 
ON c.address_id = a.address_id
INNER JOIN city ct 
ON a.city_id = ct.city_id; 
``` 

### Using subqueries to join multiple tables 
 ```sql
 SELECT c.first_name, c.last_name, ct.city
     -> FROM customer c
     ->   INNER JOIN address a
     ->   (SELECT a.address_id, a.address, ct.city
     ->    FROM address a
     ->      INNER JOIN city ct
     ->      ON a.city_id = ct.city_id;
     ->    WHERE a.district = 'California'
     ->    ) addr
     ->   ON c.address_id = addr.address_id;
```
* The results of this query will pull all addresses in California without listing all addresses. 

### Joining the same table twice 
```sql 
 SELECT f.title
     -> FROM film f
     ->   INNER JOIN film_actor fa1
     ->   ON f.film_id = fa1.film_id
     ->   INNER JOIN actor a1
     ->   On fa1.actor_id = a1.actor_id
     ->   INNER JOIN film_actor fa2
     ->   ON f.film_id = fa2.film_id
     ->   INNER JOIN actor a2
     ->   On fa2.actor_id = a2.actor_id
     -> WHERE (a1.first_name = 'CATE' AND a1.last_name = 'MCQUEEN'
     ->  AND (a2.first_name = 'CUBA' AND a2.last_name = 'BIRCH');  
```