---
title: "SQL"
date: "2019-12-17"
---

## AGGREGATE FUNCTIONS

Aggregate functions are useful functions that helps unify, summerize data in a table: **SUM()** → Will Sum all the values of a series **MAX()** → Return the max of a series of values **MIN()** → Return the lowest value of a series **AVG()** → Return the average value **COUNT()** → Retourn the number of records Some examples:

```
SELECT MAX(invoice_total)

FROM invoces;
```

```
SELECT

   MAX(invoice_total) AS highest,

   MIN(invoice_total) AS lowest,

   AVG(invoice_total) AS average,

   SUM(invoice_total * 1.1) AS summerize,

   COUNT(*) AS numbers_of_invoices,

   COUNT(DISTINCT client_id) AS clients

WHERE invoice_date > '2019-1-1'
```

As you can see you can use expressions inside the functions, and also other stuff like the DISTINCT keyword to make sure to count event the repetitions!

### **GROUP BY**

You can also group data by some other data! Let's see how:

```
SELECT

   client_id,

   SUM(invoice_total) AS total_sales

FROM invoices

GROUP BY client_id

ORDER BY total_sales DESC
```

Remember that GROUP BY clause goes before the ORDER BY, or it will generate an ERROR! You can also GROUP BY different columns:

```
SELECT 

  state,

  city,

  SUM(invoice_total) AS total_sales

FROM 

  invoices i 

JOIN clients USING (client_id)

GROUP BY state, city
```

### **HAVING clause**

Ok now let's imagine that you want to show only the results of a SUM that are greater than 500, in this case, normally you can think you can use a WHERE clause: "WHERE total\_sales > 500", but unfortunately this is not possible because the WHERE is executed before the GROUP BY clause, so this will not work! For this specific scenario you need to use the HAVING clause:

```
SELECT 

  client_id,

  SUM(invoice_total) AS total_sales

FROM 

  invoices

GROUP BY client_id

HAVING total_sales > 500
```

Of course, you can filter with more conditions like a normal WHERE clause, using AND, OR etc! The only difference is that you can not include the HAVING clause for columns that are not SELECTED! While with the WHERE clause you can filter by a column that you don't select for the output, using HAVING you must SELECT the column before or it will not work:  

## SUBQUERIES

Subqueries are an excellent tool to use for creating advanced complex queries, let's have a look! For example, let see how we can write a query to have the list of items that are more expensive of lettuce (that have an id of 3)!

```
-- Find products that are more expensive than Lettuce (id = 3)



SELECT * 

FROM products

WHERE unit_price > 

(

   SELECT unit_price

   FROM products

   WHERE product_id = 3 

)
```

The first thing that gets executed is the code inside the parenthesis and then the result is passed to the main query! In this other example, we try to find employees that earn more than the avg employee :

```
-- Find products that are more expensive than Lettuce (id = 3)



SELECT * 

FROM employees

WHERE salary > (

   SELECT AVG(salary)

   FROM employees

)
```

Note that a SUBQUERY it is not limited to the WHERE clause but you can use it with other clauses like FROM or WHERE or IN... Let's have a look what we can do with the IN clause:

### SUBQUERIES (IN)

Let's imagine that we want to find the products that have never been ordered, how can we solve this problem? Because we have a table with all the orders and a table with all the products we can do:  

```
-- Find products that have never been ordered



SELECT *

FROM products

WHERE product_id NOT IN(

SELECT DISTINCT product_id 

FROM order_items

)


```

So are trying to retrieve all the info from the table products but only if the product\_id is not inside another table! Fun no?

### SUBQUERY VS JOIN

Some times you can have the same result using JOIN instead of SUBQUERIES, let see an example:

```
-- Find clients without an invoices



SELECT *

FROM clients

WHERE client_id NOT IN(

SELECT DISTINCT client_id 

FROM invoices

)


```

this could be rewritten like this:

```
-- Find clients without an invoices



SELECT *

FROM clients

LEFT JOIN invoices USING (client_id)

WHERE invoice_id IS NULL
```

Using a LEFT JOIN we can easily have the same result. So what is the correct method? That depends on a couple of things, one is performance and the other is readability! For the moment let assume that the 2 ways have the same performance, in this case you should use the more readable one and in this case I think is the first one since it more readable using the NOT IN clause!

## ALL/ANY KEYBOARD

Let's imagine that we want to select invoices larger than all invoices for client 3 so, first of all, let's find out what is the larger invoice of client 3:

```
SELECT MAX(invoice_total) 

FROM invoices

WHERE client_id = 3;
```

We can use the MAX() function to find the largest number, now we can use this query as SUBQUERY to find all the invoices larger than this number!

```
SELECT * FROM invoices

WHERE invoice_total > (

   SELECT MAX(invoice_total) 

   FROM invoices

   WHERE client_id = 3;

)
```

And this works, but there is another solution to this problem using the ALL keyword, look:

```
SELECT * FROM invoices

WHERE invoice_total > ALL (

   SELECT invoice_total 

   FROM invoices

   WHERE client_id = 3;

)
```

Since the subquery return a multi values table, using the ALL keyword we can compare invoice\_total with ALL the result of the subquery In the same way, we can use ANY if we want that the invoice must be at least greater than one of the subquery table result!

## CORRELATED SUBQUERIES

Ok, this is easier to see that to explain:

```
SELECT * 

FROM employees e

WHERE  salary > (

   SELECT AVG(salary)

   FROM employees

   WHERE office_id = e.office_id

)
```

In this query we see a particular thing, in the subquery we reference the table not in the subquery ("WHERE office\_id = e.office\_id"), even if this can look a useless reference it is not, because this is used to reference the row, not all the table! This query actually will run the subquery every time it will read the first query row, so when we have the subquery passing the WHERE clause, it is actually passing the reference of that row! In this way, we can return only the salary that is greater than the average salary of the employee office! This is a little hard to pass, I need to exercise!

## FUNCTIONS

### NUMERIC FUNCTIONS

Functions are piece of code that we can reuse easly: **ROUND()** → This will round the number we pass: ROUND(5.7) will be 6, ROUND(5.73 , 1) will be 5.7, ROUND(5.7321 , 2) will be 5.73 **TRUNCATE()** → This will just erase all the decimal that we don't need: TRUNCATE(5.7) → 5, TRUNCATE(5.76 , 1)→5.7 , TRUNCATE(5.7321 , 2) → 5.73 **CEALING()**→ This will return the smallest integer that is greater then then number we pass: CEALING(5.7) → 6, **FLOOR()** → Exact opposite of CEALING()! FLOOR(5.7)→5 **ABS()**→ This return the ABSOLUTE VALUE of a number (-1 → 1) **RAND()** → This return a random number between 0 and 1 Here a list of all supported numeric functions of SQL: [https://www.geeksforgeeks.org/sql-numeric-functions/](https://www.geeksforgeeks.org/sql-numeric-functions/)

### STRING FUNCTIONS

**LENGTH()** → Count the number of char in a string **UPPER()** → Will capitalize all the Char in a string **LOWER()** → Will decapitalize all the Chars in a string **LTRIM()** → Eliminate Left white spaces **RTRIM()** → Eliminate Right white spaces **TRIM()** → Eliminate Left and Right spaces **LEFT('string',4)** → Will return the first 4 chars starting from the LEFT "strin" **RIGHT('string',2)** → Will return the first 2 chars starting from the RIGH "ng" **SUBSTRING('string',2,4)** → Will return the substring starting from the 2th char to the 4th char **LOCATE('n','string')** → This will return the position of the first 'n' it will find in the string so 5 (starting from 1)(returning 0 if not existing) **REPLACE('string',' ing','ong')** → This will replace from the 'string' the part where is 'ing' with 'ong' so it became 'strong' **CONCAT ('firstname','  ','secondname')** → This concatenates 2 or more strings Here there is a list of all supported string functions of SQL: [https://www.geeksforgeeks.org/sql-string-functions/](https://www.geeksforgeeks.org/sql-string-functions/) Here some aggregate string functions as well: [https://docs.microsoft.com/en-us/sql/t-sql/functions/string-agg-transact-sql?view=sql-server-ver15](https://docs.microsoft.com/en-us/sql/t-sql/functions/string-agg-transact-sql?view=sql-server-ver15)

### **DATE FUNCTIONS**

NOW() → Will return the current time of NOW 2019-03-11 12:44:03 **CURDATE()** → This will return only the date part 2019-03-11 **CURTIME()** → This will return only the time part 12:44:03 **YEAR(NOW())** → This will return only the year **MONTH(NOW())** → This will return only the month **DAY(NOW())** → This will return only the day **HOUR(NOW())** → Get the current hour **MINUTE(NOW())** → Get the current minute **SECOND(NOW())** → Get the current second All those functions return INTEGER values but other useful return strings: **DAYNAME(NOW())** → This will get the name of the day (monday sunday etc) **MONTHNAME(NOW())** → This will return the name of the month (april,may etc) **EXTRACT()** → this will extract all the infos we have upthere but in a more standard way: **EXTRACT(DAY FROM NOW()), EXTRACT(YEAR FROM NOW())**.. etc

#### FORMATTING DATES

You know that in MySQL we use string to represent dates: "2019-12-28",  but this system is not good if you want to show your result at a client, but you may not know that you can format the date in mysql, how? like this: **DATE\_FORMAT**(NOW(),'_%y_') → This will display the year with 2 digit: 19 **DATE\_FORMAT**(NOW(),'_%Y_') → This will display 2019 **DATE\_FORMAT**(NOW(),'_%m %y_') → 03 19 **DATE\_FORMAT**(NOW(),'_%M %Y_') → March 2019 **DATE\_FORMAT**(NOW(),'_%d %M %Y_') → 26 March 2019 Here some other useful info about how to format date in mysql : [https://www.w3schools.com/sql/func\_mysql\_date\_format.asp](https://www.w3schools.com/sql/func_mysql_date_format.asp)

#### **CALCULATION IN DATES**

Here some useful functions for performing calculations on day and times: **DATE\_ADD**(NOW(), INTERVAL 1 DAY) => This will add 1 day to the current date **DATE\_ADD**(NOW(), INTERVAL -1 YEAR) => This will remove 1 year to the current date **DATE\_SUB**(NOW(),INTERVAL 1 YEAR) => This will remove 1 year to the current date **DATE\_DIFF**('_2019-1-5_' , '_2019,1,1_') => This will calculate the difference between those dates, result : 4 **TIME\_TO\_SEC**('_09:00_')=> Will return the number of second passed from midnight

#### POSTGRES AND DATES

Dates are treated differently for PostgreSQL, first, you have 3+ types of date type: **DATETIME or TIMESTAMP:** Structured "real" date and time values, containing year, month, day, hour, minute, second and millisecond for all useful date & time values (4713 BC to over 100,000 AD). This can be with or without time zone **DATE :** Simplified integer-based representation of a date defining only year, month, and day. **INTERVAL:** Structured value showing a period of time, including any/all of years, months, weeks, days, hours, minutes, seconds, and milliseconds. "1 day", "42 minutes 10 seconds", and "2 years" are all INTERVAL values.

#### Which do I want to use: DATE or TIMESTAMP? I don't need minutes or hours in my value

That depends. DATE is easier to work with for arithmetic (e.g. something reoccurring at a random interval of days), takes less storage space, and doesn't trail "00:00:00" strings you don't need when printed. However, TIMESTAMP is far better for real calendar calculations (e.g. something that happens on the 15th of each month or the 2nd Thursday of leap years). More below. Now, to work with TIMESTAMP and INTERVAL, you need to understand these few simple rules :

##### 1\. The difference between two TIMESTAMPs is always an INTERVAL

```
TIMESTAMP '1999-12-30' - TIMESTAMP '1999-12-11' = INTERVAL '19 days'


```

##### 2\. You may add or subtract an INTERVAL to a TIMESTAMP to produce another TIMESTAMP

TIMESTAMP '1999-12-11' + INTERVAL '19 days' = TIMESTAMP '1999-12-30'

##### 3\. You may add or subtract two INTERVALS

```
INTERVAL '1 month' + INTERVAL '1 month 3 days' = INTERVAL '2 months 3 days'
```

#### Operations with DATEs

##### 1\. The difference between two DATES is always an INTEGER, representing the number of DAYS difference

```
DATE '1999-12-30' - DATE '1999-12-11' = INTEGER 19


```

##### You may add or subtract an INTEGER to a DATE to produce another DATE

```
DATE '1999-12-11' + INTEGER 19 = DATE '1999-12-30'


```

##### Because the difference of two DATES is an INTEGER, this difference may be added, subtracted, divided, multiplied, or even modulo (%)

##### As with TIMESTAMP, you may NOT perform Addition, Multiplication, Division, or other operations with two DATES

Also other DATE FUNCTIONS in PostgreSQL: [http://www.postgresqltutorial.com/postgresql-date-functions/](http://www.postgresqltutorial.com/postgresql-date-functions/)  

## IFNULL and COALESCE

Imagine that in your table there are some null values, but you don't want to show "null" but "not assigned" instead, to do so you can use the IFNULL clause like this:

```
SELECT

   order_id,

   IFNULL(shipper_id, 'Not assigned')

FROM orders


```

In this way, if the shipper\_id is null it will be replaced with the 'not assigned' word. Now imagine that if you have a null value you want to replace this value with the value of another column, in this case you can use COALESCE:

```
SELECT

   order_id,

   COALESCE(shipper_id,comments,comments , 'Not_assigned')

FROM orders
```

In this case, COALESCE first try if "shipper\_id is not null than try the second parameter in the list, if that is also null, then it will show 'not assigned'! Using COALESCE you can have a list of columns to try before passing him your default value.

## IF & CASE

Let's imagine that you want to show 'archived' if an order date is less than one year, and 'Active' if the order is done this year! In this case we must use the IF clause:

```
SELECT

    order_id,

    order_date,

    IF( YEAR(order_date) =  YEAR(NOW()), 'active','archived')

FROM orders
```

The use of the IF clause i a little bit like the ? in javascript,  first you pass your expression test than the TRUE result, and then the FALSE result! But what if we have more than just one expression more than one test to do? We can make a cascade of IF clauses but this is not really a good way to properly test for more things. The best and correct way is (as in many programming languages) use the CASE clause, and this is the sintax:

```
SELECT

    order_id,

    WHEN YEAR(order_date) = YEAR(NOW()) THEN 'Active'

    WHEN YEAR(order_date) = YEAR(NOW())-1 THEN 'Last Year'

    WHEN YEAR(order_date) < YEAR (NOW()) THEN 'Archived'

    ELSE '...'

FROM orders
```

So as you can see the Sintax is like this:

```
SELECT

    WHEN expression THEN value

    ELSE value

FROM orders
```

The ELSE, of course, is in case no one of those test result in a true value! you can close the IF statement with:

```
IF condition THEN

 code;

END IF;
```

## VIEWS

The views are powerful tools that help you in writing complex often reused query, views are like theoretical tables build with Queries, imagine that you need a specific table that is a join of 3 differents table, you could want to create a double of those tables to access it fastly and without having to rewrite all the query all the time, well you can do it one time and store the table (more the SQL) in a view! Then you can access it like it is a normal table, you can join this stable with other tables, etc... But this is not the only reason why views are a very powerful tool, safety is another important factor! Let's imagine you want to give access to part of a table to a user, you can create a query to access that part of a table than convert it in a view and give that user only access to that table! Or it is also useful to 'duplicate'  tables with views, why? because if you have 50-100 stored queries that you use on the database,  they become useless if you want to change some tables, and you have to rewrite those queries again. But if you have copied the table in his respective view, you can change the table and you need only to modify the views to make sure that all the queries works. How do you create views? that's the syntax:

```
CREATE VIEW view_name AS

(here goes your query)
```

Here's an example:

```
CREATE VIEW sales_by_client AS

    SELECT

         c.client_id,

         c.name,

         SUM(invoice_total) AS total_sales

    FROM clients c

    JOIN invoices i USING (client_id)

    GROUP BY client_id, name
```

remember that the view is not run is just created and stored inside the views folder. Now you can call it like a normal table (even if it is more like a virtual table), but what if you want to make a change in your view? How do you proceed? There are 2 ways to modify your view: you can drop your view and create another one or you can replace your view. If you want to drop your view you can type:

```
DROP VIEW view_name;
```

And then create it back! Or if you want to replace your view:

```
CREATE OR REPLACE VIEW view_name AS

(here's goes the query of the view)
```

Good practice, by the way, is to store your views in ".sql" files and put them under source control (Git), this way you can edit your views even if you don't remember the code!

### UPDATABLE VIEWS

As I told you before views are like a virtual table that you can use like normal tables, but that is not completely true, adding new lines, modify data, or eliminate data are things that you can do on views only in specific situations: If your view DON'T  HAVE the DISTINCT keyword, ANY aggregate functions (MIN, MAX, SUM...), any GROUP BY / HAVING statement nor any UNION, we can say that that view is a UPDATABLE VIEW and that means that you can do CRUD operations on it! But there is a catch on that, sometimes when you update the views the row that you have updated it may disappear from the view because the conditions to show the line are changed, and you may want to prevent that, and make sure that you can not modify the record in a way that makes it disappear from the view, to do so you must use the "WITH CHECK OPTION" statement at the end of your query view! This will prevent any modification of the records of that view that will end up in a no show result returning an error if you try.

### POSTGRESQL VIEWS (Materialized Views)

https://techdifferences.com/difference-between-view-and-materialized-view.html View is a **virtual table**, created using **Create View** command. This virtual table contains the data retrieved from a **query expression**, in Create View command. View can be created from one or more than one base tables or views. A view can be queried like you query the original base tables. It is **not** that the View is **precomputed** and **stored** on the disk instead, a View is **computed** each time it is used or accessed. Whenever a view is used the query expression in Create View command is executed at that particular moment. Hence, you always get the **updated** data in a View. If you update any content in View, it is reflected in the original table, and if any changes had been done to the original base table, it would reflect in its View. But this makes the performance of a View **slower**. For example, a view is created from the join of two or more tables. In that case, you have to pay time to resolve Joins each time a View is used. But it has some **advantages** like it do **not** require **storage space**. You can create a **customized** view of a complex database. You can **restrict** the user from accessing sensitive information in a database. Reduces the **complexity** of queries by getting data from several tables into a single customized View. Materialized View is the **Physical copy** of the original base tables. The Materialized View is like a **snapshot** or **picture** of the original base tables. Like View, it also contains the data retrieved from the **query expression** of **Create Materialized View** command. But unlike View, the Materialized View are **precomputed** and **stored** on a disk like an object, and they are **not updated** each time they are used. Instead, the materialized view has to be updated **manually** or with the help of **triggers**. The process of updating the Materialized View is called **Materialized View Maintenance**. Materialized View responds faster in comparison to View. It is because the materialized view is precomputed and hence, it does not waste time in resolving the query or joins in the query that creates the Materialized View. Which in turn responses faster to the query made on materialized view. Let us check the syntax of Materialized View:

```
Create Materialized View V

Build [clause] Refresh [ type]

ON [trigger ]

As <query expression>
```

Where "**Build"** clause decides when to populate the Materialized View. Refresh type decides how to update the Materialized View and trigger decides when to update the materialized View. Materialized Views are generally used in the **data warehouse**.

## STORED PROCEDURES

Let's imagine that you are building an application that uses databases, where are you going to store your queries? Store them inside the application is not exactly a good practice,  the SQL code may interfere with your code, and also for certain languages you need to compile the code before it gets executed, and so you can not change the values of your queries and recompile and redistribute your software, pretty annoying, so what is the best practice to store your queries? You can use stored procedures! Stored procedures are useful because they  sometimes make SQL queries faster, they are really helpful to store and organize SQL, and also they improve data security! Let's begin:

```
CREATE PROCEDURE get_clients()

BEGIN

    SELECT * FROM clients;

END
```

This is the basic way to create a stored procedure, we use the CREATE PROCEDURE function, we use the parenthesis to pass variables but we'll see that later... Every procedure starts with a BEGIN statement and ends with an END and as you can see every statement (even if there is only one) must end with a ";" (at least in MySQL, in other databases like PostgreSQL or others this is usually not required). Here it is the tricky part, we want to give all these statements as to MySQL a single unit, rather than individual statements separated by a semicolumn, we want MySQL to take all this entire unit and create a procedure for us called "get\_clients", we don't wont to execute individual statements, how do we do that? Well, we have to change the default delimiter ";" to something else, like this:

```
DELIMITER $$



CREATE PROCEDURE get_clients()

BEGIN

    SELECT * FROM clients

END$$



DELIMITER ;
```

By conventions we use "$$" two dollars to create a new delimiter, but you can use whatever you want. This is how we create stored procedures in MySQL, but in other database management systems, you may don't need to change the default delimiter. If you execute this code you will find in the stored procedure folder (of MySQL workbench) the new stored procedure. If you want you can use the workbench tool of MySQL to create stored procedures, with the took you don't need to change the delimiter, etc... Also, and again, it is useful to save stored procedure queries(create and delete statements) inside .sql files and put the file in source control (git).

### CALL STORED PROCEDURES

OK, everything interesting but how do we USE those stored procedures? we use the CALL statement:

```
CALL get_clients()
```

And usually, this is what we do inside our software, we call those stored procedures, but sometimes you want to call stored procedure in your SQL code.

### DROP STORED PROCEDURES

This is handy if you made an error and you want to remake the store procedure:

```
DROP PROCEDURE IF EXISTS get_clients()
```

This should be saved in an .slq file too!

### STORED PROCEDURES PARAMETERS

In stored procedures, you can also, of course, pass parameters let see how:

```
DELIMITER $$

CREATE PROCEDURE get_client_by_state(state CHAR(2))

BEGIN

     SELECT * FROM client c

     WHERE c.state = state;

END $$



DELIMITER ;
```

As you can see we use "client c" and then "c.state" to not confuse the state in the table and the state in parameter when comparing the two in the "WHERE" clause. Now for calling this stored procedure:

```
CALL get_client_by_state('CA');
```

In this case, if we don't pass a parameter we get an ERROR, because for this stored procedure the parameter is mandatory. But there is a way to make the procedure work even if we don't pass any parameters, using some default values:

```
DELIMITER $$ 



CREATE PROCEDURE get_client_by_state(state CHAR(2))

BEGIN

  IF state IS NULL THEN 

     SET state = 'CA'

  END IF;

  SELECT * FROM client c 

  WHERE c.state = state; 

END 



$$ DELIMITER ;
```

We use an IF statement to control if the parameter exists and if he doesn't we pass a default value. Using this method we can also return different values if the condition is not satisfied:

```
DELIMITER $$ 

CREATE PROCEDURE get_client_by_state(state CHAR(2))

BEGIN

  IF state IS NULL THEN 

     SELECT * clients

  ELSE

      SELECT * FROM client c 

      WHERE c.state = state;

  END IF; 

END 

$$ DELIMITER ;
```

However, this approach is a little bit amateurish, I think there is a way to combine those two queries in a single query:

```
DELIMITER $$ 

CREATE PROCEDURE get_client_by_state(state CHAR(2))

BEGIN

    SELECT * FROM  client c

    WHERE c.state = IFNULL(state,c.state) 

END 

$$ DELIMITER ;
```

Using the clause "IFNULL" we make the query way better good looking: if "state" is null, it will return all the line where c.state equal c.state, but c.state is always equal to c.state so pratically it will return all the lines.

### PARAMETER VALIDATION & RAISING ERRORS

Ok now we imagine that we have a stored procedure to store the amount of payment done, well, in this case, the amount can not be negative, must be a positive number, with the thing that we have seen since now we can check if a parameter exists or not, but not what to do if a parameter exists but it is not correct, so let's dive in:

```
CREATE PROCEDURE make_payment

(

    invoice_id INT,

    payment_amount DECIMAL(9,2)

    payment_day DATE

)

BEGIN

    IF payment_amount <= 0 THEN

           SIGNAL SQLSTATE '22003'

                SET MESSAGE_TEXT = 'Invalid Payment Amount';

    END IF;

    UPDATE invoices i

    SET 

         i.payment_total = payment_amount ,

         i.payment_date = payment_date = payment_date

   WHERE

         i.invoice_id = invoice_id;

END
```

Ok let's begin: With the first "IF" we check if the parameter that we have sent is actually greater than zero, and that is pretty much easy to understand, but what if it is not? Well, we throw an error, to do so we use the SIGNAL statement: [https://dev.mysql.com/doc/refman/5.5/en/signal.html](https://dev.mysql.com/doc/refman/5.5/en/signal.html) _[`SIGNAL`](https://dev.mysql.com/doc/refman/5.5/en/signal.html "13.6.7.4 SIGNAL Statement") is the way to “return” an error. [`SIGNAL`](https://dev.mysql.com/doc/refman/5.5/en/signal.html "13.6.7.4 SIGNAL Statement") provides error information to a handler, to an outer portion of the application, or to the client. Also, it provides control over the error's characteristics (error number, `SQLSTATE` value, message). Without [`SIGNAL`](https://dev.mysql.com/doc/refman/5.5/en/signal.html "13.6.7.4 SIGNAL Statement"), it is necessary to resort to workarounds such as deliberately referring to a nonexistent table to cause a routine to return an error._  When we signal we must also say the kind of error, using the error numbers (like 404 for HTML), for SQL we use the SQLSTATE clause, and we pass the number. Here's a list of SQLSTATE values: [https://www.ibm.com/support/knowledgecenter/en/SSEPEK\_11.0.0/codes/src/tpc/db2z\_sqlstatevalues.html](https://www.ibm.com/support/knowledgecenter/en/SSEPEK_11.0.0/codes/src/tpc/db2z_sqlstatevalues.html) Then we can pass a message to show as an error, this is useful to understand better the error:

```
SET MESSAGE_TEXT = 'Invalid Payment Amount';
```

Now, that said, it is not good to check for EVERY error, since the code would be unreadable that way, I think it's better to maintain the error checking within a reasonable size (check only for really important stuff) and prefer to change the code inside the software as a validator. It is also true that some software like C need compilation to run, and so it may be useful sometimes to make more parameters validations in stored procedures.

## VARIABLES

Not because it is SQL there are no variables :), actually, there are 2 kinds of variables: User / Session Variables, and Local Variables. **User / Session Variables**: Those variable are eliminated only when the users close the connection or the session is closed, and they are declared like this:

```
SET @invoice_count = 0;
```

**Local Variables:** Those variables are instead valid only for the execution of the code (like for stored procedures), when the code is executed those variable are destroyed let see an example: We want to calculate a risk factor, and to calculate risk factor the formula is:

```
 --risk_factor = invoices_total / invoices_count * 5
```

and now we can see how to implement this formula:

```
CREATE PROCEDURE get_risk_factor()

BEGIN

    DECLARE risk_factor DECIMAL (9,2) DEFAULT 0;

    DECLARE invoices_total DECIMAL (9,2);

    DECLARE invoices_count INT;



    SELECT COUNT(*), SUM(invoice_total)

    INTO invoices_count, invoices_total

    FROM invoices;



    SET risk_factor = invoicestotal / invoices_count * 5;

    

    SELECT risk_factor

END


```

So as you can see you use "DECLARE" to create the variable of a specific type, use "DEFAULT" if you want to get a default value for that variable, and then using the "SELECT" statement and "INTO" we pass the result of the query inside those 2 variables. In the end we select the "risk\_factor" to get our result...

## FUNCTIONS

Functions are like stored procedures, but with the difference that they can return only a single value, so no column or multiple outputs. A good example could be the calculation of "risk\_factor" of the last example. The syntax for creating functions it's very similar to the syntax of creating stored procedure:

```
CREATE FUNCTION get_risk_factor_for_client( client_id INT )

RETURNS DECIMAL(9,2)

READS SQL DATA

BEGIN 

   DECLARE risk_factor DECIMAL (9,2) DEFAULT 0; 

   DECLARE invoices_total DECIMAL (9,2); 

   DECLARE invoices_count INT; 

   

   SELECT COUNT(*), SUM(invoice_total) 

   INTO invoices_count, invoices_total 

   FROM invoices i 

   WHERE i.client_id = client_id  



   SET risk_factor = invoicestotal / invoices_count * 5; 

   

   RETURN IFNULL(risk_factor,0)

END
```

So we use "CREATE FUNCTION" to create the function, then we use a "RETURNS" statement to say which kind of data it is gonna return. After the "RETURNS" (with an "S") statement we need to set the attributes of a function, in MySQL functions must have at least one attribute: **DETERMINISTIC:** It means that if we pass the same value in this function 2 times it will always have the same output, this is useful when you know that you are not going to return a value based on other values in your database because those values can changes. Those are like Functional programming **READS SQL DATA:** This attribute means that you have some "SELECT" statement in your function that reads data. **MODIFY SQL DATA:** This attribute means that you will have an "INSERT", "UPDATE" or "DELETE" statements in your function, that modify data. And you must have at least one of those, but you can have more, if your function reads data, and modify it, you can use "READS SQL DATA" and "MODIFY SQL DATA" Then you "BEGIN" the function, do your code (use the parameter of the function if you need to) and then use the "RETURN" (without "S") to return the value you have calculated. We use "IFNULL" to make sure that if the return value is null, we will get zero instead. Now, how to use the function? Like this:

```
SELECT 

   client_id,

   name,

   get_risk_factor_for_client(client_id) AS risk_factor

FROM clients
```

### DROP FUNCTIONS

And to delete a function we can use the "**DROP FUNCTION**" statement:

```
DROP FUNCTION IF EXIST get_risk_factor_for_client;
```

## TRIGGERS

A trigger is a piece of code that is executed before or after an "INSERT UPDATE DELETE"  statement, quite often we use triggers to enforce data consistency, for example in our DB we can have multiple payments towards a given invoice, let's imagine that in a table we have a column called "payment\_total" and the value that we have in this column should be equal to the sum of all the payments for a specific invoice. So when we insert a new record in the payments table, we should make sure that the payment total column is updated as well, and this is where we use a trigger, let's see how to do this:

```
DELIMITER $$

    CREATE TRIGGER payments_after_insert

    AFTER INSERT ON payments

    FOR EACH ROW

BEGIN

    UPDATE invoices

    SET payment_total= payment_total + NEW.amount 

    WHERE invoice_id = NEW.invoice_id;

END $$

DELIMETER ;
```

As always (like this is normal) we change the delimiter("$$") and we use the "CREATE TRIGGER" statement, followed by the name of the trigger ("payment\_after\_insert"), with this kind of name we know that our trigger is launched on the payment table, but AFTER an INSERT, this a usual convention to name triggers. That's why we use "AFTER INSERT ON" in the next statement, we could have used "AFTER DELETE ON" or "BEFORE UPDATE ON" depending on what we are trying to achieve. "FOR EACH ROW" specify that we should do this for each row inserted, so if we insert 5 rows using a single statement, the trigger should run 5 times, some database management system also supports table-level triggers that get fired only once, unfortunately, MySQL doesn't support this. Now as always we "BEGIN" the code for the trigger, so we "UPDATE" the invoices table and we set the "payment\_total" column equal to payment\_total plus... and now we use the NEW keyword, this keyword returns the row that was just inserted, we also have the "OLD"  keyword that is useful when we update or deleting a row so the "OLD" keyword refers to the old value of that row. We use the "." to access the value of what we are inserting (in this case we "NEW.amount" give us the value of the amount column that we are adding). And of course, we use "WHERE" to indicate that those modifications are necessary where the invoice id corresponds to the new id that we are inserting.

### THE INFINITE LOOP TRIGGER

There is a risk for an infinite loop when using triggers for example if we create a trigger that runs when we update a table, and in the script of the trigger we modify that table, that script will run infinitely! And that's bad. So in order to avoid any of this, inside the code of the trigger, we should never modify the same table (especially when using UPDATE and DELETE).

### SEEING THE TRIGGERS

Unfortunately, there is no way to see the triggers as table, but there is a way to see all triggers that we have in our DB:

```
SHOW TRIGGERS
```

This will show us all the trigger that are running in the DB

```
SHOW TRIGGERS LIKE 'payments%'
```

To see all the triggers that have 'payments' in the name

### DROP TRIGGERS

Of course there is a way also to delete triggers, and the syntax is very similar to what we have already done:

```
DROP TRIGGER IF EXIST 'payments_after_insert';
```

As a good practice we should have creation and dropping triggers code inside ".sql" files and then you can put them in source control.

### USING TRIGGERS FOR AUDITING

We already have said that we use triggers for data consistency, but another common use for triggers is log database changes for auditing and reports, for example, if someone modify or delete a record we can log in the change, so we can see later who made the change and what is changed and when, etc... Let's imagine that we have an Audit table for the "payments" table, we can create a trigger like this:

```
DELIMITER $$

    CREATE TRIGGER payments_after_insert

    AFTER INSERT ON payments

    FOR EACH ROW

BEGIN

    INSERT INTO payments_audit 

    VALUES (NEW.client_id, NEW.date, NEW.amount, 'Insert',NOW() ) 

END $$

DELIMETER ;
```

or we can do:

```
DELIMITER $$

    CREATE TRIGGER payments_after_delete

    AFTER INSERT ON payments

    FOR EACH ROW

BEGIN

    INSERT INTO payments_audit 

    VALUES (OLD.client_id, OLD.date, OLD.amount, 'Delete',NOW()) 

END $$

DELIMETER ;
```

As you can see this way we log all the insert and all the delete from this table into payments\_audit table, this is good if you don't have a lot of table but it's usually better to use a general to table to log this kind of stuff but we will see that later on the course.

## EVENTS

If a trigger it's executed if we make an action (like INSERT or UPDATE) the events are time-related, an event could be explained as a task(SQL code) that is executed according to a schedule, it can be executed on a regular basis like every day at 10 am, or once a month, it is time-based. The main goal of events is to automate maintenance tasks, such as "deleting stale data" or "copying data from one table to an archive table" or "aggregating data to generate reports". Now before we can schedule an event we need to turn on MySQL event scheduler,  that is the process that runs in the background, and it constantly looks for events to execute;

```
SHOW VARIABLES;
```

This show all the system variables of MySQL, and there are quite a few of those, so we use:

```
SHOW VARIABLES LIKE '%event';
```

So we can see if the event scheduler is on or off, in the case is off we can do:

```
SET GLOBAL  event_scheduler = ON
```

and also we can turn it OFF if we don't have schedules and we don't want to waste resources. ok now let's create an event:

```
DELIMITER $$

CREATE EVENT yearly_delete_stale_audit_rows

    ON SCHEDULE

    -- AT '2019-05-01'

    EVERY 2 DAYS




```

As always the first thing that we have to do (that is not annoying, absolutely not annoying) is to change the DELIMITER, then we use the "CREATE EVENT" statement with the name of the event, in this case, "yearly\_delete\_stale\_audit\_rows" and we use this naming convention for a reason: "yearly" stating how often the code gets executed,  and then what the code does. Then you add ON SCHEDULE and then if you want to schedule the code for one time run use "AT '2019-05-01' " o

## TRANSACTIONS

Transactions are a way to secure that the executions of SQL code, in a transaction if one o more SQL code don't get executed, the other SQL codes that where executed before are annulled, if you must do a money transaction for example, from one table to another, you have to detract money from one table and add money to another table, if you do the first and during the second there is a problem and the SQL don't get executed, you will lose your money and your friend will not get any. So in order to make sure that the transaction is fulfilled, if the second code dont get execute the first one is annulled.

### ACID PROPERTIES

Transactions have a few properties that you need to know: **Atomicity:** The transactions are like atoms, they are unbreakable, each transaction is a single unit at work no matter how many statements it contains. Either all the statements are executed and the transaction is "COMMITTED" or the transaction is "ROLLBACK" and all the changes are undone. **Consistency:**  That means that with transactions the data remains in a consistent state, we won't end up with an order without an Item. **Isolation:** That means that every transaction is Isolated and so protected from each other if they try to modify the same data. They can not interfere with each other if multiple transactions try to modify the same data, the rows that are been affected get locked so only one transaction at the time can update those rows, and the other transaction has to wait to the other transaction to complete. **Durability:**  That means that once a transaction is committed, the changes made by the transaction are permanent, so if you have a power failure or a system crash, you are not going to lose the changes.

### CREATE A TRANSACTION

How to get transactions:

```
START TRANSACTION;



INSERT INTO orders (customer_id, order_date, status) 

VALUES (1,'2017-01-01', 1);



INSERT INTO order_items

VALUES (LAST_INSERT_ID(), 1, 1,1);



COMMIT; --ROLLBACK IF YOU WANT TO STOP THE TRANSACTION
```

We use "START TRANSACTION" to initiate a transaction, then we pass al the statements that we want, and then you use the "COMMIT" command to permutate the changes. If you want to check for errors manually and stop the transaction manually, you can use the "ROLLBACK" command to stop the transaction.

### TRANSACTION THEM ALL

In reality, in MySQL, all the transactions are wrapped in TRANSACTIONS and it does a "COMMIT" if the code does not return an ERROR. So when we have an "INSERT UPDATE DELETE" MySQL wrap this in  a transaction and then it will do a commit automatically, this is controlled using a system variable called "autocommit":

```
SHOW VARIABLES LIKE 'autocommit';
```

if you want to see if the variable is set to "ON" or "OFF"

## CONCURRENCY AND LOCKING

Concurrency is when two users try to access the same data at the same time, and concurrency can become a problem when one user modifies the data that other users are trying to retrieve or modify, we are going to see how MySQL handle concurrency by default and how to resolve most common concurrency problems.

### DEFAULT MYSQL HANDLE

You have already seen how MySQL handles concurrency by default with transactions, it **locks the data** you want to modify in the database, and if other statements try to access the same data they have to wait until the transaction is either committed or rolled back. There special cases where the default behavior is not sufficient, in those situations you can override the default behavior.

### CONCURRENCY  PROBLEMS

Now that we know what concurrency is, let's see what are the common problems with it:

#### Lost Updates:

This happens when two transactions try to update the same data if we don't use locks, in this situation the transaction that commits later, will override the changes made by the previous transaction. Let's imagine that we have two users that are trying to update the same customer, one is trying to increase the points of the customer, and the other is trying to update the state of the customer. So we have 2 transactions in order, A and B, the one that commits last will erase the update of the other transaction, so if B will commit last we are going to have another state for the customer but no points updated, and vice versa (so it seems that when you update a data in a row you update all the row). To prevent this behavior we use LOCKS, as in MySQL default system.

#### Dirty Reads:

Those happen when you are trying to read data that are not committed yet, for example, transaction A changes the point of a customer from 10 to 20, but before it commits the change, transactions B rates this customer and based on the customer points make a decision (points base discount charge). Now, what if transaction A rollback before transaction B complete? Transaction B will have data that have never existed, so in this scenario, we have read uncommitted data in transaction B, our data was dirty, that's why we need a layer of isolations during transactions so that data modified by transactions are not immediately visible to other transactions, unless, of course, is committed. The Standard sequel defines four isolations level, (that we are going to see later) and one of these is "**READ COMMITED**", so that transaction can only read committed data. We are going to see how to set an isolation level later.

#### Non-repeating Reads

So with isolation level, we can make sure that we can only read committed data, but what if during a course of a transaction you read something twice and get different results? For example transaction A reads our customer points and sees that this customer has 10 points, and so it will make a business decision based on this value. Now before transaction A complete, another transaction, transaction B, updates the points for this costumer to zero, so back to transaction A, we need to read the points from this customer one more time (perhaps as part of a subquery) and we get a different result. This is a non-repeatable or inconsistent read. How should we handle this situation? we can argue that at any point in time we should make decisions based on what is the most up to date data; if that is the case for the business scenario, we don't have to worry about anything here, but we can also argue that at the time the transaction started this costumer had 10 points and so give him 10$ discount, in which case we should not see that change when doing a read, we should see the "initial snapshot", if this is what we want then we want to increase the isolation level of our transaction. The sequel standard defines another isolation level called "**REPEATABLE READS**", with this level our reads are repeatable and consistent even if the data gets changed by other transactions. We will see only the snapshot that was established at the first read.

#### Phantom Reads

Imagine in a transaction A, you are querying all the customer that have 10 points, and give them discount code, at the same time transaction B updates the point of another customer that was not returned by our query, so this customer is now eligible for this discount code, but at the time we query the customers table we didn't see this customer. So after transaction A complete there is still one eligible customer for the discount code that didn't receive the code, this is what we call a "**phantom read**", how can we solve this problem? Well, it depends on the business problem we are trying to solve, and how important it is, to include this customer in our transaction: we can always re-execute transaction A at a later time, and the customer will be included this time, so not a big deal, but if it is absolutely critical to include all eligible customer in our we have to make sure that no other transactions are running that can impact our query to find our eligible customers, and for this, we have another isolation level called "**SERIALIZABLE**" and this will guarantee  that our transaction will be aware of changes currently being made by other transactions to the data, if there are other transactions modifying the data that can impact our query result, our transaction has to wait for them to complete, so transactions will be executed sequentially, this is the highest level of isolation that we can apply to a transaction and it gives us the most certainty in our operations, but it comes with a cost, the more users and transactions we have, the more waits we're going to experience and the system is going to slow down, so this isolation level can hurt performance and scalability, and for this reason we should reserve this only in scenario where is absolutely critical  and necessary to prevent phantom reads.

### ISOLATION TRANSACTIONS LEVEL

- READ UNCOMMITTED
- READ COMMITTED
- REPEATABLE READ
- SERIALIZABLE

Those are all the possibles transaction isolations level in SQL, the default transaction isolation level is "REPEATABLE-READ", you can see it doing:

```
SHOW VARIABLE LIKE 'transaction_isolation';
```

Now if you want to change the transaction isolation level you can do it in different ways: you can change the variable value for the next transaction(and then it return to default) or you can change the transaction isolation level per session (using keyword "SESSION") (each session is a connection to the database) so that all the transaction for that sessions have a different isolation transaction level or even if it is not recommended you can change the transaction isolation level globally, for any future transactions (using keyword "GLOBAL"):

```
SET TRANSACTION ISOLATION LEVEL SERIALIZABLE 

--FOR THE NEXT TRANSACTION
```

```
SET SESSION TRANSACTION ISOLATION LEVEL SERIALIZABLE

--FOR ALL THE NEXT TRANSACTION IN THE SESSION
```

```
SET GLOBAL TRANSACTION ISOLATION LEVEL SERIALIZABLE

--FOR ALL THE NEXT TRANSACTION IN THE SESSION
```

## DEADLOCKS

Now let's look at another classic problem in databases: Deadlocks! What are deadlocks? Deadlocks happen when differents transactions can not complete because it's waiting for another transaction to complete , but that transaction can not complete because it is waiting for the first transaction to complete... look:

```
-- CONNECTION 1

START TRANSACTION;



UPDATE customer 

SET state = 'VA'

WHERE customer_id = 1;



UPDATE orders 

SET status = 1 

WHERE order_id = 1;



COMMIT;
```

```
-- CONNECTION 2

START TRANSACTION;



UPDATE orders 

SET status = 1 

WHERE order_id = 1;



UPDATE customer 

SET state = 'VA'

WHERE customer_id = 1;



COMMIT;
```

Those two are the exact same code but inverted in order and executed from 2 different connections, let's now have a look at the error that we can get: Let's imagine that those 2 codes are executed at the same time, so the first code execute a modification on the customer table, for the customer 1,  so it locks it in the database, now the second code update the "orders" table, and lock the order number 1, at this point the first code (in the second part) try to update the "orders" table but it can't because the data is locked and it must wait until the second transaction gets completed, now we get to the second code (the second part of it) but can not get executed because the customer table for customer 1 is locked and it must wait for the first transaction to finish, but the first transaction is waiting for the second transaction to finish so they block each other out, and no transition get committed and we get a deadlock error, actually the second one gets executed but this is another story. How to resolve the deadlock problem? Generally speaking, deadlocks are not a big issue unless they happen very frequently if you are an app dev you should write your application in a way that it can reissue a transaction if it gets rolled back because of a deadlock, or maybe just tell the user that the operation failed and to try again, but there are some few things you can do to minimize deadlocks(you can really get rid of them):

- If you frequently detect deadlocks in two transactions look at their code, these transactions could be part of 2 stored procedures, specifically look at the order of the statements in your transactions, if they are in the reverse order, it is likely that you are going to have a deadlock, so switch them.
- You can try to keep your transaction small and short in duration so they are not likely to collide with other transactions. If you have transactions that operate in really large tables, those may take a long time to run and then you are at risk for collisions. If that is the case see if you can schedule those transactions to run during not busy moments.

## DATA TYPES

### STRINGS

- CHAR(x)   -- Fixed length
- VARCHAR(x)  -- Variable length - Max: 65.535 characters (64 kb)
- MEDIUMTEXT  --max: 16mb (roughtly 16milions characters) -good for json object of medium size,cs view strings etc
- LONGTEXT -- max: 4GB, good for books, or years of log files,

We also have

- TINYTEXT max: 255 bytes/characters
- TEXT max: 64kb (64k char)

But for this range of character it's better to use VARCHAR type because can be indexed All these types support international characters, English letters use 1 byte, European and middle east languages use 2 bytes, and Asian languages use 3 bytes per character. So if a type of column is a char of 10, MySQL will reserve 30 bytes per record

### INTEGERS

- TINYINT:1byte so from -128 to 127 good to store ages of people
- UNSIGNED TINYINT: 1 byte, from 0 to 255 good also for ages, and a lot of other values (colors) and also prevent accidental negative value errors
-  SMALLINT: 2bytes \[-32k to 32k\]
- MEDIUMINT: 3bytes \[-8M,8M\]
- INT:  4bytes \[-2B,2B\]
- BIGINT: 8bytes\[-9Z,9Z\]

If you try to pass a number out of range you will get an error. Apart from UNSIGNED, numeric types have another attribute and that is called "0 field", this is useful in situations where you want show an exact number of digit, like this: INT(4) this will show "0001" even if the number is below the 4 digits

### FLOATING POINT

- DECIMAL (p,s) - Fixed number of digit after the decimal value, DECIMAL(9,2) = 1234567,89
- FIX, DEC, NUMERIC - those are exactly like DECIMAL
- FLOAT, DOUBLE  - those are for those who need or high number or very small number (scientific calculation for example) they dont store the exact value but an appoximation
- FLOAT 4 bytes
- DOUBLE 8  bytes

### BOOLEAN TYPE

- BOOL
- BOOLEAN

### ENUM & SET TYPES

There are times where we want to restrict the value for a column to a limited list of strings(sizes, states, etc)

- ENUM('small','medium','large')
- SET(multiple values)

Even if this may look appealing to you, enums are generally bad,  and you should avoid them. the first reason is that change the members of an enum can be expensive, enums are not reusable(in other tables you have to change them every timesà). A better approach is to have a separate table (like sizes) where you can store all the sizes and any other attribute (like measurement), you can reuse this table in multiple places. We refer to this kind of table as a lookup table.

### DATE and TIME TYPES

\*in postgreSQL dates are treated differently

- DATE -- store the date without time
- TIME -- store only the time without the date
- DATETIME -- store date and time
- TIMESTAMP --4bytes so up to (2038)
- YEAR --Store four-digit year

We use timestamp usually to keeping track of when a row was inserted or updated, timestamp is 4 bytes long so you can use it until 2038. This is called the year 2038 problem DATETIME use 8bytes, so beyond 2038 use DATETIME!

### BLOBS

We use blob types to store a large amount of binary data, like images, video, pdf etc... in MySQL we have 4 blob types:

- TINYBLOB: 255 bytes
- BLOB: 65kb
- MEDIUMBLOB: 16Mb
- LONGBLOB: 4Gb

Generally speaking, it is better to keep your files out of your databases.

### JSON

you can pass a JSON a parameter,

## USING JSON

MySQL is not like other NO-SQL platforms of course, but it doesn't mean that it can not store JSON!

```
UPDATE products

SET properties = '

{

    "dimensions":[1,2,3],

    "weight":10,

    "manufacturer":{"name":"sony"}

}

'

WHERE product_id=1;
```

This is one way to store JSON in SQL columns, but there are other way, and interesting functions:

```
UPDATE products

SET properties = JSON_OBJECT(

'weight',10,

'dimensions',JSON_ARRAY(1,2,3),

'manufacturer', JSON_OBJECT('name','sony')

)

WHERE product_id = 1;
```

How to extract info inside a JSON object with SQL?

```
SELECT product_id, JSON_EXTRACT(properties,'$.weight') AS weight

FROM products

WHERE product_id = 1;
```

Using "JSON\_EXTRACT we can retrieve data from JSON, we pass the column where the JSON is ("properties") and then we use the $ sign to mean "this object" and use "." as usual; You can use another syntax for the same result:

```
SELECT product_id, properties -> '$.weight' AS weight --use ->> if you want to get rid of " in result

FROM products

WHERE product_id = 1;
```

Of course we also have some functions to update JSON Objects in MySQL

```
UPDATE products 

SET properties = JSON_SET(

      properties,

      '$.weight',20,

      '$.age',10

)

WHERE product_id = 1;
```

We also have "JSON\_REMOVE" to remove some property of an object

```
UPDATE products 

SET properties = JSON_REMOVE(

      properties,

      '$.age'

)

WHERE product_id = 1;
```

## INDEXING FOR HIGH PERFORMANCE

Indexing is very important in large databases and High traffic websites because they can improve the performance of queries dramatically. This is a very important topic that every developer and Database administrator must learn and understand. Indexes are basically data structure, that database engine uses to quickly find data, as an analogy you may think of a telephone directory, you can quickly find someone else phone number because numbers are sorted by last names, and database engines use indexes to find data in a similar way. Let's imagine that we want to find customers located in California, without an Index MySQL have to test every record in the customer table, this is not a big deal in a small table with relative few hundred of thousand records, but as our table grow larger the cost of these queries is going to increase dramatically, we can speed up this query by creating an index state column, and this is like creating a directory of customers sorted by state, in this directory, or more accurately in this index, you only got the state of a customer and the reference to the customer record in the table. So MySQL can quickly find the corresponding customers using this index, and then it will read those records from the table, this is way faster than reading every record of the single table. In a lot of cases, indexes are small enough that they can fit into memory (RAM) and that's why they are so powerful, reading from memory is always better than reading from hard disk. So indexes help us find records quickly, but they come with a cost: **Increased size of a Database:** They have to be permanently stored in the database next to our table. **Slow down the writes:** Every time we need to insert or update or delete a record, MySQL has to update the corresponding indexes and this will impact the performance of our machine. For those reasons we should reserve indexes for **PERFORMANCE CRITICAL QUERIES** One of the common error of developers is that they add indexes at the time of designing tables, you don't create indexes based on your tables, you create them based on your queries! Because the all point of the indexes is to speed up slow queries. Internally indexes are store like binary trees, but I prefer to present them as tables because it is easier to visualize and understand. But they are a tree not a table. Binary trees are basically one of the fundamental data structure that computer science students learn at school, they don't really need to understand how binary trees work with databases indexes.

### CREATING AN INDEX

Ok let's build an index for the example we already presented: all the states in the table.

```
CREATE INDEX idx_state ON customers (state)
```

We use the "CREATE INDEX" statement to create the index and then the name, now for the name conventions uses "idx" as prefix (index) and then the column that we are creating the index for.

### VIEW INDEXES

```
SHOW INDEXES IN customers;
```

The result of this is a table with information about indexes, column name (where the index is applied), collation (how data is sorted in the index) : A = Ascending, B = descending, cardinality represents an estimated number of uniques values in the index, to get more accurate values we use "ANALYZE TABLE name\_table" statement, to run a better analysis, and then re-run the "SHOW INDEX" command,  the primary key is already an index and it is also called the clustered index, so every table can have a maximum of one clustered index, others indexes that are not primary keys are called secondary indexes. If you look at the type for all indexes you can see "BTREE" that is short for "binary tree",

### PREFIX INDEXES

Ok, we already know how to create new indexes, now the index you want to create an index is a string column (varchar, text, blob), an index may consume a lot of space and it won't perform well, smaller indexes tend to perform better because they can fit in memory and this makes our search faster. So when indexing string columns we don't want to include the entire column in the index, we only want to include the first few characters or the prefix of the column so our index will be smaller. Let's say we want to create an INDEX on the last name column in the costumer table:

```
CREATE INDEX idx_lastname ON customers(last_name(20))
```

Since "last\_name" is a character column we can specify inside a parenthesis the number of characters you want to include in the index, this is optional for char and varchar columns, but it is compulsory for text and blob columns. In our case, we want to include the first 20 characters in the index, and now we have a prefix index, I choose 20 but to identify the correct number we should see the data, we want to include enough characters that can uniquely identify each customer. Using only one character is not a good index because it doesn't allow MySQL to quickly find a customer by their last name. An index may return 100,000 customer who last name start with A, here is a tip to find a good number :

```
SELECT 

     COUNT (DISTINCT LEFT(last_name, 1)),

     COUNT (DISTINCT LEFT(last_name, 5)),

     COUNT (DISTINCT LEFT(last_name, 10))

FROM customers;
```

With this method, we can try out the number of unique results for different number of characters, the objective is to get around the total number of the row in the column without inserting too much data.If the data in the table are 1000 and the first count gives us 25, that is bad indexing because we have to iterate then for 1000/25 records, the second count (5 chars) give us 960 differents record, this is a preatty good indexing (1000/960), the third one give us 990 so it is better than 960 but it is not worth the other 5 char \* 1000 of memory needed.

### FULL-TEXT INDEX

In MySQL and other database management systems, we have a special kind of index called "full-text index", we use this index to build a fast and flexible search engine in our application. Let's say we want to create a blog website and we want to give the customer and we want to give the user the ability to search for blog posts, the consequent table would be composed of :

- post\_id
- title
- body
- date\_published

Now let's imagine that the user is looking for the words "reacts" and "redux", how do search for those info inside the database?

```
SELECT * 

FROM posts 

WHERE title LIKE '%react redux%' OR

             body LIKE '%react redux%';
```

with this code, we can find any post that has this search phrase in their title or inside their body, but there are few issues with this query: The first issue is that currently, we don't have an index on these columns as our post table grows larger, this query is going to get slower and slower, and we can not use the prefix index, in this case, to help us out with this query because it only controls the first characters of a string. The other issue with this query is that it returns only the posts that have the exact two words in this order(react,redux), so this will not return posts that have only "react" or "redux" or the two word in an inverse order, so this query is not suitable for a search engine, and this is where you use full-text index. With these indexes we can implement a fast and powerful search engine in your application. These indexes work very differently from regular indexes, they include the entire string column, they don't store just a prefix, they ignore any stop words like "in", "on", "the", and they basically store a list of words, and for each word, and for each word, they store a list of rows or documents that these rows appear in. Let's create one:

```
CREATE FULLTEXT INDEX idx_title_body ON post (title, body);
```

Now if we want to search inside the database we can do something like this:

```
SELECT *

FROM posts

WHERE MATCH(title, body) AGAINST('react redux');
```

We use the keyword "MATCH" in this case. We have passed "title" and "body" because we have created the index with those two columns, if we had created the index with 3 columns we should have passed the 3 columns, otherwise MySQL is going to complain. Then we use the "AGAINST" keyword to pass the word we want to look for, and now this will return any post that has one or both these keywords in their title or body, these words can be in any order, and they can be separated by one or more words.

#### NOW LET'S TAKE THIS TO ANOTHER LEVEL

One of the beauties of the full-text searches is that they include a relevance score, based on a number of factors MySQL calculates a relevancy score for each row that contains the search phrase. This score is floating-point number between 0 to 1, 0 no relevance, 1 perfect match, so let's display the relevant score in the result:

```
SELECT *, MATCH(title, body) AGAINST('react redux')

FROM posts

WHERE MATCH(title, body) AGAINST('react redux');
```

This is pretty useful, and a little bit like google. Now, this full-text search engine has two modes: Naturale Language Mode: This is the default mode, Boolean Mode: Not the default mode and with this mode, we can include or exclude certain words like how we use google.

```
SELECT *, MATCH(title, body) AGAINST('react redux') 

FROM posts 

WHERE MATCH(title, body) AGAINST('-react redux +form' IN BOOLEAN MODE);
```

Now we can exclude words by prefixing it with a minus "-", and this will tell the search engine that we want all the posts that contain "redux" that don't have "react" in it. We can also obligate the search engine to have a word either on the title or on the body by adding "+" before the word. We can also search for an exact phrase using double quotes:

```
WHERE MATCH(title, body) AGAINST('"handling a form"' IN BOOLEAN MODE);
```

And yes, the double quotes must be inside the single quote in this case.

### COMPOSITE INDEXES

Ok, we saw how to create an index, how to index text, how to do a full search inside text, etc... Now, what if you need two indexes for the same table? you can create 1 index for each column that you need, this will work but this is not the best way to index the table: Let's imagine that we want to lool for customers located in California who have more than 1000 points:

```
EXPLAIN SELECT curstomer_id FROM customer

WHERE state= 'CA' AND POINT > 1000
```

Using the keyword EXPLAIN we will see a lot of useful information about this query, one of those is the possible\_keys that tell us what key it can use to get the result faster, and if you have indexed the 2 columns (state and point), you will see both of them, but if you look at the column key you will see that only one key is used and not both, so for the other part of the job MySQL has to table scan the rest of records (even if there is an index for those). And this is why a composite index (like a crossed index) is a very useful tool, with a composite index, we can index multiple columns together:

```
CREATE NEW INDEX idx_state_points ON customers (state, points)
```

So this is the creation of a composite index, you can ask if the order actually matter, and it does (but we are going to see this later). So after getting this new composite index, let's explain the same query one more time and see the results: the actually used key this time is our new composite index, and this time the number of scanned rows is 58 (previously 112) (you can see that from the column "rows"), so this composite index is doing a better job at optimizing our query. So, since we have already said that the indexes are designed by our needs and no in "table designing" moment, it is also true to say that probably most of the time it is better to use composite indexes because a query can have multiple filters, also indexes can help us store data faster, so if you have multiple columns in our index, you can also speed up sorting our data. One of the common mistakes that a lot of beginners make is to create a lot of separate indexes on each column, and the more indexes you have, the slower you write operations are going to be. So, how many columns should you include in your index? In MySQL, an index can have a maximum of 16 columns, and generally speaking, that's a pretty high number, you don't want to hit that, I would say between 4-6 columns bodes well, but don't take this as hard and fast rule, or best practice, you should always experiment based on your queries and the amount of data you have.

### ORDER OF COLUMNS IN COMPOSITE INDEXES

As we already said, in a composite index, the order of the column is important, so here two basic rules:

- Put the most frequently used column first
- Put the columns with a high cardinality first
- Take your query into account

**"Put the most frequently used column first"**, this makes perfect sense, if we have 5 queries and all or most of these queries look up customers by state, it makes sense to put that column first, because this can help narrow down searches. "**Put the columns with a high cardinality first**", cardinality represents the number of unique values in the index, for example, the "last\_name" column has a high cardinality, as well as phone numbers, less age, and even less gender (according to biology and not SJW). "**Take your query into account**", for some queries it's not better to put cardinality first, for example, if you want index state ('FR','IT','GB') and last name, as we said before the last name has more cardinality than states, but if our query is looking for all the last names that start with "A" the cardinality is reduced by a lot because you will probably have more people whose the last name starts with "A" than people in a specific state, so for this query, it is better to inverse the order. So don't forget to take your query into account.

### USE SPECIFIC INDEX

If you want to use a specific index for your query you can:

```
SELECT customer_id --Test it with Explain

FROM customers

USE INDEX (idx_lastname_state)

WHERE state = 'FR' AND lastname LIKE 'A%';
```

This way you can easily test your indexes with the "EXPLAIN" keyword.

### WHEN INDEXES ARE IGNORED

There are situations where you have an index but you still experience performance problems, let's look at one of the common scenarios:

```
SELECT customer_id

FROM customers

WHERE state = 'FR' OR points < 1000;
```

This time instead of using the "AND" keyword we use "OR",

## SECURING DATABASES

If you don't take security seriously, people can access and misuse your data, so in this section, we are going to learn about user accounts, privileges, for securing databases.

### CREATING AN USER

When you develop your application, with your SQL server running in your pc, you connect to it with the root user, the same user account that is created at the time you install MySQL, now when it comes to using MySQL in a real production environment, we need to create additional user accounts, and give them specific privileges. Let say that you are creating a web or a desktop application that needs to access data in a MySQL database, so you need to create a user account and give it permission to read and write data to your database, but nothing more. So this user account should not be able to change the structure of your database, it shouldn't be able to create a w table, or dropping existing ones, because this can mess up with everything. As another example let's say someone new joins your organization as a database administrator, you need to create an account so he can manage some databases or even the entire server. This is how you create a new user account:

```
CREATE USER user IDENTIFIED BY 'password' 



CREATE USER user@127.0.0.1 IDENTIFIED BY 'password' 



CREATE USER user@localhost IDENTIFIED BY 'password' 



CREATE USER user@simonepanebianco.fr IDENTIFIED BY 'password'  

CREATE USER user@'%.simonepanebianco.fr' IDENTIFIED BY 'password'
```

We use the keyword "CREATE USER" and then the user name, then we can restrict from where the user can connect using the @ sign, you can type the IP address of the new computer(in this case 127.0.0.1 it's the same computer), this is very useful in a cloud environment, where typically you have a web server and a database server, and, on the database server, when creating a new user account, you want to make sure that that user account can only connect from the web server, so this is where you specify the IP address of your webserver. We can also specify a hostname (like "localhost" witch represent the same computer once again), or a domain name (like "simonepanebianco.fr") and now this user can connect from any computer in this domain, but they won't be able to connect from the subdomains, to provide that we must add a wildcard here (" '%.simonepanebianco.fr' ") and "%" represents any subdomains. And of course, we pass the password  with the "IDENTIFIED BY" statement.

### VIEWING AN USER

Now that we have created our user, we want to be able to see it. By default MySQL have a database called "mysql" in witch inside we have a table called "user" so we can select everything from this table:

```
SELECT * FROM mysql.user
```

The result is a table with all the users and all the information needed:

- Host: represent where the user can connect from("localhost", "simonepanebianco.fr", "%" that means anywhere)
- User: the user name
-  Several columns that determine the permission for each user (we are going to see this later)

If you do so in a newly installed version of MySQL you can see that you have 4 users, witch one of those is "root", the basic one, and if you see in the host column you will see "localhost" that means that you can access this database server with the "root" account only if you are on the same machine. You can also see the user in the navigator panel if you use the MySQL workbench.

### DROPPING USERS

As you can create an account you can also DELETE an account, pretty easy:

```
DROP USER user@simonepanebianco.fr
```

Yes, you should also use the host when deleting an user.

### CHANGING PASSWORDS

There are two ways to do this:

```
SET PASSWORD FOR user = 'newpassword'



SET PASSWORD = 'newpassword' --for the currently connected user
```

Or you can use the workbench panels to do the same thing

### GRANTING PRIVILEGES

Once you have created a user account you should assign some privileges. Let's see two scenarios:

#### FIRST SCENARIO

In the first scenario, you have a Web/Desktop application and you want to allow this application to read, write data in your database, but nothing more. You don't want this application to be able to create a new table or modify existing tables, that is something that only an ADMIN should be able to do. Let's imagine that we have an application called Moon, and this application needs to read and write data in the SQL store database, so first, we need to create a user account for this application:

```
CREATE USER moon_app INDENTIFIED BY 'password';
```

Now we use the "GRANT" statement to give this user a few privileges, we want this user to be able to "SELECT", "INSERT", "UPDATE", "DELETE", as well as "EXECUTE" stored procedures, There are the typical privileges we want to give the user account for an application.

```
GRANT SELECT, INSERT, UPDATE, DELETE, EXECUTE 

ON sql_store.*

TO moon_app; --specify host or domain name here if necessary
```

Once we know what the privileges are, we should also specify for which database you can have those privileges using the "ON" keyword, here you can use the "\*" to indicate all the tables, or pass the table you want the access for: "sql\_store.customers" (not very common but doable). And then use the "TO" keyword to specify the user (and remember that you have to specify also the hostname if necessary)

#### SECOND SCENARIO

The second scenario is for our admins, someone new joins our organizations, and you want to give them administrative privileges over one or more databases, or perhaps the entire server. For this user we want more than just the one we gave for the web app, we want the user capable of create tables, create triggers, modify existing tables, etc... Here is a list of all the privileges supported by MySQL: [https://dev.mysql.com/doc/refman/8.0/en/privileges-provided.html](https://dev.mysql.com/doc/refman/8.0/en/privileges-provided.html) You can find all kind of privileges you may need for in that list, but here we talk about the privilege ALL, the highest level of privilege we can give to someone and is what is typically given to an Admin.

```
GRANT ALL

ON *.*

TO user
```

Using \*.\* we give access to all the databases and tables;

### VIEWING PRIVILEGES

If you want to see the privileges of a user account, you can use SQL:

```
SHOW GRANTS FOR user;

SHOW GRANTS; --Current user
```

### REVOKE PRIVILEGES

If you want to revoke a privilege:

```
REVOKE CREATE VIEW

ON sql_store.*

FROM user;
```
