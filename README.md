# SQL Injection

SQL injection attack consists of insertion of either a partial or complete SQL query via the data input from the client (Browser) to Application (Server).

## SQL Injection Technique:

-	***Union Operator:*** can be used when the SQL injection flaw happens in a SELECT statement, making it possible to combine two queries into a single result or result set.
-	***Boolean:*** use Boolean condition(s) to verify whether certain conditions are true or false.
-	***Error based:*** this technique forces the database to generate an error, giving the attacker or tester information upon which to refine their injection.
-	***Out-of-band:*** technique used to retrieve data using a different channel (e.g., make a HTTP connection to send the results to a web server).
-	***Time delay:*** use database commands (e.g. sleep) to delay answers in conditional queries. It useful when attacker doesn’t have some kind of answer (result, output, or error) from the application



