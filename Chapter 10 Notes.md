# Joins Revisited: 
* Outer Joins make the Join condition optional essentially. 
* The join definition was changed from inner to left outer, which instructs the 
server to include all rows from the table on the left side of the join

## Left vs. Right: 
* LEFT OUTTER JOIN means that the table on the left determines the rows
* RIGHT OUTTER JOIN means that the table on the right determines the rows
* From what I'm seeing, if you do an outer join as the first join, you have to do the second as one as well. 

## Cartesian Product: 
* Joining a table without any join conditions ^ 
* Done through a CROSS JOIN
* Rarely used and not for large tables

## Natural Joins: 
* NATURAL JOIN will let the server decide the Join conditions. 