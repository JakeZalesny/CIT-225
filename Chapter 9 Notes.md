# Subqueries:
* A query within another query. 
* Subqueries are executed first. 
* When the outside query is done the data from the subquery is discarded. 
* If you ever forget what a subquery is doing, run it by itself. 
* The way I understand it, Subqueries kind of act like getters in OOP. 
* Scalar Subquery, returns only one piece of data. 

```sql 
SELECT city_id, city
    -> FROM city
    -> WHERE country_id <>
    ->  (SELECT country_id FROM country WHERE country = 'India');
```
* This query returns all the cities not in India. 
* The subquery returns the country id for India. 
* If you have a Subquery in a conditional equation, you'll get an error. 
* You can't equate to multiple values, but you can see if a value is contained in the set. 

Example: 
```sql
mysql> SELECT country_id
    -> FROM country
    -> WHERE country IN ('Canada', 'Mexico');
```

* Can use the 'IN' and 'NOT IN' operators. 
* The 'ALL' operator allows you to compare to every single value in a set of data individually. 

Example: 
```sql
mysql> SELECT customer_id, sum(amount)
    -> FROM payment
    -> GROUP BY customer_id
    -> HAVING sum(amount) > ANY
    ->  (SELECT sum(p.amount)
    ->   FROM payment p
    ->     INNER JOIN customer c
    ->     ON p.customer_id = c.customer_id
    ->     INNER JOIN address a
    ->     ON c.address_id = a.address_id
    ->     INNER JOIN city ct
    ->     ON a.city_id = ct.city_id
    ->     INNER JOIN country co
    ->     ON ct.country_id = co.country_id
    ->  WHERE co.country IN ('Bolivia', 'Paraguay', 'Chile')
    ->  GROUP BY co.country
    -> );
```

* This query Finds all customers who's film rentals exceed the total payments for all customers in Bolivia, Paraguay, and Chile. 

## Multiple Column Subqueries:
Example: 
```sql 
mysql> SELECT fa.actor_id, fa.film_id
    -> FROM film_actor fa
    -> WHERE fa.actor_id IN
    ->  (SELECT actor_id FROM actor WHERE last_name = ‘MONROE’)
    ->   AND  fa.film_id IN
    ->  (SELECT film_id FROM film WHERE rating = 'PG'
```

* This query uses two subqueries: one to identify all actors with 
the last name Monroe, and the second subquery identifies all films rated PG.

Previous Example Merged looks like: 
```sql 
mysql> SELECT actor_id, film_id
    -> FROM film_actor
    -> WHERE (actor_id, film_id) IN
    ->  (SELECT a.actor_id, f.film_id
    ->   FROM actor a
    ->      CROSS JOIN film f   
    ->   WHERE a.last_name = 'MONROE’
    ->   AND f.rating = 'PG');
```

## Correlated Subqueries
* A correlated subquery is dependent on it's containing statement. 
* Cannot be executed by itself. 
* A correlated statement isn't executed beforehand, rather once for each row in the containing statement. 

Example: 
```sql 
mysql> SELECT c.first_name, c.last_name
    -> FROM customer c
    -> WHERE 20 =
    ->  (SELECT count(*) FROM rental r
    ->   WHERE r.customer_id = c.customer_id);
```
* The reference to c.customer_id is what makes this a correlated query. 
* It's essentially the body of a 'FOR' loop, the containing query gets all of it's rows first and then executes the subquery for each of them. 
* 'EXISTS' is when you want to identify a relationship without looking at quantity. 

Example: 
```sql 
mysql> SELECT c.first_name, c.last_name
    -> FROM customer c
    -> WHERE EXISTS
    -> (SELECT 1 FROM rental r
    ->  WHERE r.customer_id = c.customer_id
    ->    AND date(r.rental_date) < '2005-05-25');
```

* This query identifies any customer who rented a film before May 05, 2005. 
* 'NOT EXISTS' does the opposite. 

## Data Manipulation: 
* Subqueries work in UPDATE, DELETE, and INSERT statements too. 
* The 'FROM' clause can contain subqueries too: 
Example: 
```sql 
mysql> SELECT c.first_name, c.last_name’
    ->   pymnt.num_rentals, pymnt.tot_payments
    -> FROM customer c
    ->   INNER JOIN
    ->    (SELECT customer_id,
    ->       count(*) num_rentals,
    ->       sum(amounts) tot_payments
    ->     FROM payment
    ->     GROUP BY customer_id
    ->    ) pymnt
    ->   ON c.customer_id = pymnt.customer_id;
```

## Data Fabrication: 
* It's possible to create data that doesn't exist in the database at all. 
* Using the 'UNION ALL' clause, we move all data into a single set using multiple select statements. 

The final result looks something like: 
```sql 
mysql> SELECT pymnt_grps.name, count(*) num_customers
    -> FROM
    ->(SELECT customer_id,
    ->   count(*) num_rentals, sum(amount) tot_payments
    -> FROM payment
    -> GROUP BY customer_id
    -> ) pymnt
    ->   INNER JOIN
    -> (SELECT 'Small Fry' name, 0 low_limit, 74.99 high_limit
    -> UNION ALL
    -> SELECT 'Average Joes' name, 75 low_limit, 149.99 high_limit
    -> UNION ALL
    -> SELECT 'Heavy Hitters' name, 150 low_limit, 9999999.99 high_limit
    -> ) pymnt_grps
    -> ON pymnt.tot_payments
    ->    BETWEEN pymnt_grps.low_limit AND pymnt_grps.high_limit
    -> GROUP BY pymnt_grps.name;
```

* First subquery returns the total number of film rentals. 
* The second creates the grouping. 
