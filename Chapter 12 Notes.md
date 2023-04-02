# Indexes and Constraints
- Indexes make table scanning much quicker than usual. 
- It's like adding a save point in a video game. 

EX: 
```sql 
mysql> ALTER TABLE customer
-> FADD INDEX idx_email (email);
```

* You can use the show command in a database to see the index:

EX: 

```sql 
SHOW INDEX FROM customer \G
```

You can remove indexes too 

```sql 
ALTER TABLE customer
DROP INDEX idx_email; 
```

- Unique indexes disallow duplicates. 

```sql
ALTER TABLE customer 
ADD UNIQUE idx_email; 
```

- Indexes can cover multiple columns 

B-tree indexes: Balanced trees

Bitmap Indexes: Good for Yes/No situations 

Text Indexes: Which allows for searches for words and phrases within documents. 

- B-trees are the default 
- A-M / N-Z

## Constraints:
- pretty much apply on any datatype in a database 
- created along with the database. 
- can be done with the ALTER keyword 
    