# Data Generation, Manipulation, and Conversion

char - Fixed-Length, blank padded strings

varchar - Variable length strings 

text - very large variable length strings. 

```sql 
 INSERT INTO string_tbl (char_fld, vchar_fld, text_fld)
    -> VALUES (‘This is char data’,
    ->   ’This is varchar data’, 
    ->   ’This is text data’);
```
* Older versions of MySQL would truncate the string and issue a warning. 
* Strings are marked by single quotes. 
```sql 
 UPDATE string_tbl
    -> SET text_fld = 'This string didn''t work, but it does now'; 
```
* length() returns how many chars 
* position() location of a substring 
* locate() find charachters other than the first. 
* strcmp() compares strings 
    * -1 if the first is before the second in sort order 
    * 0 if they are the same 
    * 1 if the first string comes after the second

* concat() returns a combined string 
* insert() adds characters into strings 
* substring() removed specified number of characters starting in a specific position 
* mod() does division 
* pow() does exponents 
* ceil() and floor()

### Temporal data
* date, datetime, timestamp, time 
* cast() function can convert strings to dates. 

## Video Notes
* Chars only have a fixed amount of characters. 
