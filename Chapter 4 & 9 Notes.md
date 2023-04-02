# Chapter 4 Filtering
| Intermediate Result | Finale Result |
| :--- | :---: |
| Where TRUE or TRUE | True
| Where TRUE or FALSE | True
| Where FALSE or TURE | True
| Where FALSE or FALSE | False

* If the clause has 3 or more conditions use parentheses 
* The NOT operater is equivalent to != essentially. 
* Don't use the '==' in sql
* The other option for '!=' is '<>' 

Ex: 
```sql
WHERE date(r.rental_date) <> '2005-06-14'; 
```

### Data modifications 
* Typically use inequality/equality operators in data modification. 
* Range conditions: '<' '>' or '<=' '>=' 
* 'BETWEEN' is also an operator. 

EX. 

```sql 
WHERE rental_date BETWEEN '2005-06-14' AND '2005-06-16';
```
* LOWER LIMIT GOES FIRST

* The IN operator finds a range of values 

EX. 
```sql
WHERE rating IN ('G', 'PG');
``` 
## Using Subqueries 
It is also possible to create a subquery that will generate a set for you. If you wanted to assume that any title with the name “pet” in it would be safe for family viewing, you could execute a subquery against the film table to retrieve all ratings associated with these films and then retrieve all films having any of these ratings:

EX. 
```sql 
SELECT title, rating
FROM film
WHERE rating IN (SELECT rating 
FROM film WHERE title LIKE '%PET');
```

* Not in example: 
```sql 
WHERE rating NOT IN ('PG-13', 'R', 'NC-17')
```

### Wildcards
_ = Exactly one character 
% = Any number of characters 

* Can't use a WHERE clause with an aggregate function so you have to use HAVING
* WHERE Clause is executed 2nd overall but is the first filter.
* REGEXP 'la' it looks for something anywhere in the column 
* 'ND$' start at the right of the string. '.ll' match on two Ls
* [a, e], ll will find a list of characters. 
* LIKE is only used with % and _
* Subqueries are always slower than joins. 
* use a join if you can. 
* Subqueries return one row. 
* ANY(subquery), SUM(subquery), ALL(subquery)
