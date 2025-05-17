If you have ever worked with a relationship database, you have probably been exposed to the term : **NORMALIZATION**

But what it is ? why we do it ? How do we do it ? And what bad things would happens if we don't do it ? Why there are 5 level of **Normal Form** ? 

What is Normalization ?

## BAD DATA
Bad data in a database are very common, mostly there is nothing that we can do to fix that, but some bad data can be prevented with database normalization.

Example of unavoidable bad data : 

| customer_id | birthday   |
| ----------- | ---------- |
| 1           | 12/05/2025 |
| 2           | 12/05/2025 |
in this example, is it clear that, the real birthdate of customer 1 and 2 is not what they declared to be.

Example of avoidable bad data: 

| customer_id | birthday   |
| ----------- | ---------- |
| 1           | 01/01/1970 |
| 1           | 25/05/1986 |
In this example the customer 1 have 2 birthdate, this is not possible, and preventable.

This kind of bad data is avoidable with database normalization.
Normalized table are not only protected from contradictory data, they are also easier to understand enhance and extend.

**Normalized tables are protected from :**
- insertion anomalies
- update anomalies
- deletion anomalies

## NORMALIZATION FORM LEVELS

There are 5 levels of Normalization Form, you can imagine those level as "security levels", in the first level "1NF", the security is minimal while in the last "5NF" security is at its best.

### 1 There is no such thing as row order
In a relationship database there is no such thing as row order to convey information.
For example:

table: beatles

| id  | name |
| --- | ---- |
| 1   | Paul |
|     |      |
