## SQL Synthesis
SQL stands for Structured Query Language

SQL can execute queries, retrieve, update, create, delete data, tables, create stored procedures, views, set permissions
### Select, where, and, or, not, null/not null, limit, as, group by, having, order by, exist, any/all
```roomsql
SELECT column1, column2, ...
FROM table_name
WHERE condition1 AND condition2 OR condition3 ...
GROUP BY column_name(s)
HAVING condition
ORDER BY column_name(s);

SELECT column1, column2, ...
FROM table_name
WHERE NOT condition;

SELECT column1, column2, ...
FROM table_name
ORDER BY column1, column2, ... ASC|DESC;

SELECT column_names
FROM table_name
WHERE column_name IS NULL;

SELECT column_names
FROM table_name
WHERE column_name IS NOT NULL;

SELECT column_name(s)
FROM table_name
WHERE condition
LIMIT number;

SELECT column_name AS alias_name
FROM table_name;

SELECT column_name(s)
FROM table_name
WHERE EXISTS
(SELECT column_name FROM table_name WHERE condition);

SELECT column_name(s)
FROM table_name
WHERE column_name operator ANY
  (SELECT column_name FROM table_name WHERE condition);
  
SELECT column_name(s)
FROM table_name
WHERE column_name operator ALL
  (SELECT column_name
  FROM table_name
  WHERE condition);
```
```roomsql
SELECT COUNT(DISTINCT Country) FROM Customers;

SELECT * FROM Customers
WHERE NOT Country='Germany' AND NOT Country='USA';

SELECT COUNT(CustomerID), Country
FROM Customers
GROUP BY Country
ORDER BY COUNT(CustomerID) DESC;

SELECT COUNT(CustomerID), Country
FROM Customers
GROUP BY Country
HAVING COUNT(CustomerID) > 5;

SELECT SupplierName
FROM Suppliers
WHERE EXISTS (SELECT ProductName FROM Products WHERE Products.SupplierID = Suppliers.supplierID AND Price < 20);

SELECT ProductName
FROM Products
WHERE ProductID = ANY
  (SELECT ProductID
  FROM OrderDetails
  WHERE Quantity = 10);
  
SELECT ProductName
FROM Products
WHERE ProductID = ALL
  (SELECT ProductID
  FROM OrderDetails
  WHERE Quantity = 10);
```

### Insert
```roomsql
INSERT INTO table_name (column1, column2, column3, ...)
VALUES (value1, value2, value3, ...);
```

### Update
```roomsql
UPDATE table_name
SET column1 = value1, column2 = value2, ...
WHERE condition;
```

### Delete
```roomsql
DELETE FROM table_name WHERE condition;
```

### Function
```roomsql
SELECT MIN(column_name)
FROM table_name
WHERE condition;

SELECT MAX(column_name)
FROM table_name
WHERE condition;

SELECT COUNT(column_name)
FROM table_name
WHERE condition;

SELECT AVG(column_name)
FROM table_name
WHERE condition;

SELECT SUM(column_name)
FROM table_name
WHERE condition;
```
The MySQL `IFNULL()` function lets you return an alternative value if an expression is NULL, or we can use the COALESCE() function
```roomsql
SELECT ProductName, UnitPrice * (UnitsInStock + IFNULL(UnitsOnOrder, 0))
FROM Products;

SELECT ProductName, UnitPrice * (UnitsInStock + COALESCE(UnitsOnOrder, 0))
FROM Products;
```
### Like, In/Not In, Between
```roomsql
SELECT column1, column2, ...
FROM table_name
WHERE columnN LIKE pattern;

SELECT column_name(s)
FROM table_name
WHERE column_name IN (SELECT STATEMENT);

SELECT column_name(s)
FROM table_name
WHERE column_name BETWEEN value1 AND value2;
```
```roomsql
SELECT * FROM Products
WHERE Price BETWEEN 10 AND 20;

SELECT * FROM Products
WHERE Price NOT BETWEEN 10 AND 20;
```
- `%`	Represents zero or more characters	bl% finds bl, black, blue, and blob
- `_`	Represents a single character	h_t finds hot, hat, and hit
- `[]`	Represents any single character within the brackets	h[oa]t finds hot and hat, but not hit
- `^`	Represents any character not in the brackets	h[^oa]t finds hit, but not hot and hat
- `-` Represents any single character within the specified range	c[a-b]t finds cat and cbt

### Join
```roomsql
SELECT column_name(s)
FROM table1
INNER JOIN table2
ON table1.column_name = table2.column_name;

SELECT column_name(s)
FROM table1
LEFT JOIN table2
ON table1.column_name = table2.column_name;

SELECT column_name(s)
FROM table1
RIGHT JOIN table2
ON table1.column_name = table2.column_name;

SELECT column_name(s)
FROM table1
FULL OUTER JOIN table2
ON table1.column_name = table2.column_name
WHERE condition;

SELECT column_name(s)
FROM table1 T1, table1 T2
WHERE condition;
```
```roomsql
SELECT Orders.OrderID, Customers.CustomerName, Shippers.ShipperName
FROM ((Orders INNER JOIN Customers ON Orders.CustomerID = Customers.CustomerID)
INNER JOIN Shippers ON Orders.ShipperID = Shippers.ShipperID);

SELECT A.CustomerName AS CustomerName1, B.CustomerName AS CustomerName2, A.City
FROM Customers A, Customers B
WHERE A.CustomerID <> B.CustomerID
AND A.City = B.City
ORDER BY A.City;
```
### Union
The `UNION` operator is used to combine the result-set of two or more `SELECT` statements.

Every `SELECT` statement within `UNION` must have the same number of columns
```roomsql
SELECT column_name(s) FROM table1
UNION
SELECT column_name(s) FROM table2;

SELECT column_name(s) FROM table1
UNION ALL
SELECT column_name(s) FROM table2;
```
```roomsql
SELECT City FROM Customers
UNION
SELECT City FROM Suppliers
ORDER BY City;
```
### Into
The `SELECT INTO` statement copies data from one table into a new table.
```roomsql
SELECT *
INTO newtable [IN externaldb]
FROM oldtable
WHERE condition;
```
```roomsql
SELECT * INTO CustomersBackup2017 IN 'Backup.mdb'
FROM Customers;

SELECT CustomerName, ContactName INTO CustomersBackup2017
FROM Customers;
```
The `INSERT INTO SELECT `statement copies data from one table and inserts it into another table, and requires that the data types in source and target tables match.
```roomsql
INSERT INTO table2 (column1, column2, column3, ...)
SELECT column1, column2, column3, ...
FROM table1
WHERE condition;
```
### Procedures
```roomsql
CASE
    WHEN condition1 THEN result1
    WHEN condition2 THEN result2
    WHEN conditionN THEN resultN
    ELSE result
END;
```
```roomsql
SELECT OrderID, Quantity,
CASE
    WHEN Quantity > 30 THEN 'The quantity is greater than 30'
    WHEN Quantity = 30 THEN 'The quantity is 30'
    ELSE 'The quantity is under 30'
END AS QuantityText
FROM OrderDetails;

SELECT CustomerName, City, Country
FROM Customers
ORDER BY
(CASE
    WHEN City IS NULL THEN Country
    ELSE City
END);
```
A stored procedure is a prepared SQL code that you can save, so the code can be reused over and over again.
```roomsql
CREATE PROCEDURE procedure_name
AS
sql_statement
GO;
```
```roomsql
EXEC procedure_name;
```
```roomsql
CREATE PROCEDURE SelectAllCustomers
AS
SELECT * FROM Customers
GO;
```
```roomsql
EXEC SelectAllCustomers;
```
### Database
```roomsql
CREATE DATABASE databasename;
DROP DATABASE databasename;

BACKUP DATABASE databasename
TO DISK = 'filepath';

CREATE TABLE table_name (
    column1 datatype,
    column2 datatype,
    column3 datatype,
   ....
);

DROP TABLE table_name;

ALTER TABLE table_name
ADD column_name datatype;

CREATE TABLE table_name (
    column1 datatype constraint,
    column2 datatype constraint,
    column3 datatype constraint,
    ....
);
```
The `CHECK` constraint is used to limit the value range that can be placed in a column.
```roomsql
CREATE TABLE Persons (
    ID int NOT NULL,
    LastName varchar(255) NOT NULL,
    FirstName varchar(255) NOT NULL,
    Age int
);

CREATE TABLE Persons (
    ID int NOT NULL UNIQUE,
    LastName varchar(255) NOT NULL,
    FirstName varchar(255),
    Age int
);

CREATE TABLE Persons (
    ID int NOT NULL,
    LastName varchar(255) NOT NULL,
    FirstName varchar(255),
    Age int,
    CONSTRAINT UC_Person UNIQUE (ID,LastName)
);

ALTER TABLE Persons
DROP INDEX UC_Person;

ALTER TABLE Persons
DROP CONSTRAINT UC_Person;

CREATE TABLE Persons (
    ID int NOT NULL PRIMARY KEY,
    LastName varchar(255) NOT NULL,
    FirstName varchar(255),
    Age int
);

CREATE TABLE Orders (
    OrderID int NOT NULL,
    OrderNumber int NOT NULL,
    PersonID int,
    PRIMARY KEY (OrderID),
    FOREIGN KEY (PersonID) REFERENCES Persons(PersonID)
);

CREATE TABLE Persons (
    ID int NOT NULL,
    LastName varchar(255) NOT NULL,
    FirstName varchar(255),
    Age int CHECK (Age>=18)
);

CREATE TABLE Persons (
    ID int NOT NULL,
    LastName varchar(255) NOT NULL,
    FirstName varchar(255),
    Age int,
    City varchar(255) DEFAULT 'Sandnes'
);

CREATE TABLE Persons (
    Personid int NOT NULL AUTO_INCREMENT,
    LastName varchar(255) NOT NULL,
    FirstName varchar(255),
    Age int,
    PRIMARY KEY (Personid)
);
```
### Indexes
Indexes are used to retrieve data from the database more quickly than otherwise. The users cannot see the indexes, they are just used to speed up searches/queries.
```roomsql
CREATE INDEX index_name
ON table_name (column1, column2, ...);

CREATE UNIQUE INDEX index_name
ON table_name (column1, column2, ...);

ALTER TABLE table_name
DROP INDEX index_name;
```
### Date
- `DATE` - format `YYYY-MM-DD`
- `DATETIME` - format: `YYYY-MM-DD HH:MI:SS`
- `TIMESTAMP` - format: `YYYY-MM-DD HH:MI:SS`
- `YEAR` - format `YYYY` or `YY`
### View
In SQL, a view is a virtual table based on the result-set of an SQL statement.
```roomsql
CREATE VIEW view_name AS
SELECT column1, column2, ...
FROM table_name
WHERE condition;
```
### Data Types
#### String
* `CHAR(size)`	A FIXED length string (can contain letters, numbers, and special characters). The size parameter specifies the column length in characters - can be from 0 to 255. Default is 1
* `VARCHAR(size)`	A VARIABLE length string (can contain letters, numbers, and special characters). The size parameter specifies the maximum column length in characters - can be from 0 to 65535
* `BINARY(size)`	Equal to CHAR(), but stores binary byte strings. The size parameter specifies the column length in bytes. Default is 1
* `VARBINARY(size)`	Equal to VARCHAR(), but stores binary byte strings. The size parameter specifies the maximum column length in bytes.
* `TINYBLOB`	For BLOBs (Binary Large Objects). Max length: 255 bytes
* `TINYTEXT`	Holds a string with a maximum length of 255 characters
* `TEXT(size)`	Holds a string with a maximum length of 65,535 bytes
* `BLOB(size)`	For BLOBs (Binary Large Objects). Holds up to 65,535 bytes of data
* `MEDIUMTEXT`	Holds a string with a maximum length of 16,777,215 characters
* `MEDIUMBLOB`	For BLOBs (Binary Large Objects). Holds up to 16,777,215 bytes of data
* `LONGTEXT`	Holds a string with a maximum length of 4,294,967,295 characters
* `LONGBLOB`	For BLOBs (Binary Large Objects). Holds up to 4,294,967,295 bytes of data
* `ENUM(val1, val2, val3, ...)`	A string object that can have only one value, chosen from a list of possible values. You can list up to 65535 values in an ENUM list. If a value is inserted that is not in the list, a blank value will be inserted. The values are sorted in the order you enter them
* `SET(val1, val2, val3, ...)`	A string object that can have 0 or more values, chosen from a list of possible values. You can list up to 64 values in a SET list
#### Numeric
* `BIT(size)`	A bit-value type. The number of bits per value is specified in size. The size parameter can hold a value from 1 to 64. The default value for size is 1.
* `TINYINT(size)`	A very small integer. Signed range is from -128 to 127. Unsigned range is from 0 to 255. The size parameter specifies the maximum display width (which is 255)
* `BOOL`	Zero is considered as false, nonzero values are considered as true.
* `BOOLEAN`	Equal to BOOL
* `SMALLINT(size)`	A small integer. Signed range is from -32768 to 32767. Unsigned range is from 0 to 65535. The size parameter specifies the maximum display width (which is 255)
* `MEDIUMINT(size)`	A medium integer. Signed range is from -8388608 to 8388607. Unsigned range is from 0 to 16777215. The size parameter specifies the maximum display width (which is 255)
* `INT(size)`	A medium integer. Signed range is from -2147483648 to 2147483647. Unsigned range is from 0 to 4294967295. The size parameter specifies the maximum display width (which is 255)
* `INTEGER(size)`	Equal to INT(size)
* `BIGINT(size)`	A large integer. Signed range is from -9223372036854775808 to 9223372036854775807. Unsigned range is from 0 to 18446744073709551615. The size parameter specifies the maximum display width (which is 255)
* `FLOAT(size, d)`	A floating point number. The total number of digits is specified in size. The number of digits after the decimal point is specified in the d parameter. This syntax is deprecated in MySQL 8.0.17, and it will be removed in future MySQL versions
* `FLOAT(p)`	A floating point number. MySQL uses the p value to determine whether to use FLOAT or DOUBLE for the resulting data type. If p is from 0 to 24, the data type becomes FLOAT(). If p is from 25 to 53, the data type becomes DOUBLE()
* `DOUBLE(size, d)`	A normal-size floating point number. The total number of digits is specified in size. The number of digits after the decimal point is specified in the d parameter
* `DOUBLE PRECISION(size, d)	 `
* `DECIMAL(size, d)`	An exact fixed-point number. The total number of digits is specified in size. The number of digits after the decimal point is specified in the d parameter. The maximum number for size is 65. The maximum number for d is 30. The default value for size is 10. The default value for d is 0.
* `DEC(size, d)`	Equal to DECIMAL(size,d)