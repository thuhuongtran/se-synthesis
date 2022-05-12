## Mysql
MySQL is an open-source Database Management System (DBMS).

MySQL is the first choice for those web-based projects which require a database merely for data transactions and not anything intricate.

### Optimization
#### Optimize query
1. Use Indexes Where Appropriate
```roomsql
SELECT … WHERE
```
2. Avoid using a function in the predicate
For example:
```roomsql
SELECT * FROM MYTABLE WHERE UPPER(COL1)='123'Copy
```
The `UPPER` notation creates a function, which must operate during the SELECT operation. This doubles the work the query is doing, and you should avoid it if possible.
3. Avoid % Wildcard in a Predicate in the beginning
```roomsql
SELECT * FROM person WHERE name LIKE "%ch"
```
Doing a search for names using the wildcards in the beginning increases the query cost significantly because an indexing scan does not apply to ends of strings. A wildcard at the beginning of a search does not apply indexing.
Instead, a full table scan searches through each row individually, increasing the query cost in the process.

A way to search ends of strings is to reverse the string, index the reversed strings and look at the starting characters. Placing the wildcard at the end now searches for the beginning of the reversed string, making the search more efficient.
4. Specify Columns in SELECT Function
```roomsql
SELECT column1, column2 FROM table
```
5. Use `ORDER BY` Appropriately

If you try to sort different columns in different order, it will slow down performance. You may combine this with an index to speed up the sorting.
7. `GROUP BY` Instead of `SELECT DISTINCT`

The `SELECT DISTINCT` query comes in handy when trying to get rid of duplicate values. However, the statement requires a large amount of processing power.
Whenever possible, avoid using `SELECT DISTINCT`, as it is very inefficient and sometimes confusing.

Avoid using:
```roomsql
SELECT DISTINCT column1, column2 FROM table
```
Instead, try using:
```roomsql
SELECT column1, column2 FROM table GROUP BY column1
```
8. JOIN, WHERE, UNION, DISTINCT

Try to use an `inner join` whenever possible. An outer join looks at additional data outside the specified columns.

The `UNION` and `DISTINCT` commands are sometimes included in queries. Like an outer join, it’s fine to use these expressions if they are necessary. However, they add additional sorting and reading of the database. If you don’t need them, it’s better to find a more efficient expression.

9. Use the `EXPLAIN` Function

Appending the `EXPLAIN` expression to the beginning of a query will read and evaluate the query. If there are inefficient expressions or confusing structures, `EXPLAIN` can help you find them.

#### Other optimization approaches

1. Use InnoDB, Not MyISAM
`MyISAM` is an older database style used for some MySQL databases. It is a less efficient database design. The newer `InnoDB` supports more advanced features and has in-built optimization mechanics.

`InnoDB` uses a clustered index and keeps data in pages, which are stored in consecutive physical blocks. If a value is too large for a page, InnoDB moves it to another location, then indexes the value. This feature helps keep relevant data in the same place on the storage device, meaning it takes the physical hard drive less time to access the data.

2. Use the Latest Version of MySQL

3. Check Pagination Queries

You can create optimizations by only showing the link to the next page, rather than links to all available pages.

4. Use an Automatic Performance Improvement Tool

### MySQL Triggers

In MySQL, a trigger is a stored program invoked automatically in response to an event such as insert, update, or delete that occurs in the associated table.

MySQL supports triggers that are invoked in response to the `INSERT`, `UPDATE` or `DELETE` event.

The SQL standard defines two types of triggers: row-level triggers and statement-level triggers. MySQL supports only row-level triggers.
- A row-level trigger is activated for each row that is inserted, updated, or deleted
- A statement-level trigger is executed once for each transaction regardless of how many rows are inserted, updated, or deleted.
#### Advantages of triggers
- Triggers give an alternative way to run scheduled tasks.
- Triggers can be useful for auditing the data changes in tables.

#### Triggers commands
```roomsql
CREATE TRIGGER trigger_name
{BEFORE | AFTER} {INSERT | UPDATE| DELETE }
ON table_name FOR EACH ROW
trigger_body;

DROP TRIGGER [IF EXISTS] [schema_name.]trigger_name;
```
```roomsql
CREATE TRIGGER before_employee_update 
    BEFORE UPDATE ON employees
    FOR EACH ROW 
 INSERT INTO employees_audit
 SET action = 'update',
     employeeNumber = OLD.employeeNumber,
     lastname = OLD.lastname,
     changedat = NOW();
     
DROP TRIGGER before_billing_update;
```
```roomsql
SHOW TRIGGERS;
```
### MyISAM and InnoDB
Total 5 types of tables are present:
* MyISAM
* Heap
* Merge
* INNO DB
* ISAM

MyISAM is an old storage engine of MySQL. InnoDB : The default storage engine in MySQL 8.0.
InnoDB is a transaction safe storage engine developed by Innobase Oy which is a Oracle Corporation now.

Here are a few of the major differences between InnoDB and MyISAM:

- InnoDB has row-level locking. MyISAM only has full table-level locking.
- InnoDB has what is called referential integrity which involves supporting foreign keys (RDBMS) and relationship constraints, MyISAM does not (DMBS).
- InnoDB supports transactions, which means you can commit and roll back. MyISAM does not.
- InnoDB is more reliable as it uses transactional logs for auto recovery. MyISAM does not.