# Part 4: Hive

Hive is an important component of the Hadoop ecosystem which adds data warehousing abilities on top of Hadoop and HDFS. It is used primarily to perform ad hoc queries and analysis of large data sets using HiveQL. In addition to standard SQL-like queries, Hive is amenable to use with other tools to enable easy data extraction, transformation, and loading (ETL). 

HiveQL queries are translated into an underlying execution framework (MapReduce, Tez, Spark) which will execute the query functionality on distributed datasets in a cluster.

***Basic Hive Commands***

To access the Hive shell, open a shell terminal in the VM and type:

```
hive
```

To list all databases in the Hive meta store, regardless of the location they are stored at:

```
CREATE DATABASE myfirstdb;
SHOW DATABASES;
```

Verify the creation of this database as a directory in the default location by using the Hue File Browser or typing in a separate Linux shell terminal type:

```
hdfs dfs -ls /user/hive/warehouse
```

Create database with comment and DB properties:

```
CREATE DATABASE firstdb COMMENT 'my wonderful database' LOCATION '/user/cloudera/dbplace' WITH DBPROPERTIES('Created by' = 'User', 'Created on' = '1-Jan-2015');
```

Using a database:

```
USE myfirstdb;
```

Determining the current database in use:

```
SELECT current_database();
```

Describing a database:

```
DESCRIBE DATABASE EXTENDED myfirstdb;
```

Altering the database properties:

```
ALTER DATABASE myfirstdb SET DBPROPERTIES('Created by' = 'New User', 'Rating' = 'Ok');

DESCRIBE DATABASE EXTENDED myfirstdb;
```

Deleting a database:

```
DROP DATABASE myfirstdb CASCADE;
```

***Numeric data types***

* INT
* FLOAT
* DOUBLE
* DECIMAL
* BINARY

***String data types***

* STRING
* VARCHAR(max_length)
* CHAR(LENGTH)

***Data / Time data types***

* TIMESTAMP
* DATA

***Miscellaneous data types***

* BOOLEAN
* BINARY

***Complex data types***

* ARRAY
* STRUCT
* MAP
* UNIONTYPE

Schema is the blueprint of the database.

Schema on read, the data is structured according to the schema at the time when it is retrieved from its location.

Schema on write, the contained data had to conform to the schema at the point of storage.

To create table:

```
CREATE TABLE Accounts (name VARCHAR(10), account VARCHAR(10), month CHAR(3), amount DECIMAL(4,1), num SMALLINT) ROW FORMAT DELIMITIED FIELDS TERMINATED BY "," TBLPROPERTIES ("skip.header.line.count" = "1"); //To skip the first line of the CSV files specify the name of the columns
```

```
hdfs dfs -ls /user/hive/warehouse/mydb.db
```

To store the table at a different location, use the LOCATION clause. For e.g. type at the Hive shell:

```
CREATE TABLE AnotherTable(Name STRING, Age INT) LOCATION '/user/hive/warehouse/somenewplace';
```

Load files in HDFS into the table directory:

```
LOAD DATA INPATH 'labs/hive/accounts.csv' INTO TABLE Accounts;
```

Load files in HDFS into the table directory and overwrite the exist table:

```
LOAD DATA INPATH 'labs/hive/accounts.csv' OVERWRITE INTO TABLE Accounts;
```

Load it with the original file from the local Linux file system:

```
LOAD DATA LOCAL INPATH '/home/cloudera/labstuff/hive/accounts.csv' INTO TABLE Accounts;
```

Describing a table:

```
DESCRIBE Accounts;
```

Shows the CREATE TABLE statement of a table or a view, with all relevant attributes shown:

```
SHOW CREATE TABLE Accounts;
```

Renaming and moving tables:

```
ALTER TABLE Accounts RENAME TO Dog;
SHOW TABLES;
SHOW CREATE TABLE Dog;

ALTER TABLE Dog RENAME TO Accounts;
SHOW TABLES;
SHOW CREATE TABLE Accounts;
```

ALTER TABLE to move a table from one database to another:

```
CREATE DATABASE tempdb;
ALTER TABLE Accounts RENAME TO tempdb.Accounts;
USE tempdb;
SHOW TABLES;
```

Move the table back to its original location with:

```
ALTER TABLE Accounts RENAME TO mydb.Accounts;
USE mydb;
SHOW TABLES;
DROP DATABASE tempdb;
```

Deleting data in a table:

```
TRUNCATE TABLE Accounts;
```

Deleting a table:

```
DROP TABLE Accounts;
DROP TABLE AnotherTable;
```

The general form of the SELECT statement:

```
SELECT [DISTINCT] columnlist FROM table1
[[INNER | LEFT |RIGHT] JOIN table2 ON conditions]
[WHERE conditions]
[GROUP BY columnList]
[HAVING group_conditions]
[ORDER BY columnList]
[LIMIT offset, length];
```

To retrieve all the records and display all rows and columns in the designated table:

```
SELECT * FROM table;
```

To retrieve all the records; displaying all the rows and certain specified columns from the designated table:

```
SELECT name, amount, month FROM Accounts;
```

A literal value can be used as a column, even if is totally unrelated to the data in the table. The literal value appears in all rows and also as the column label: 

```
SELECT 'stuff', Name FROM Accounts;
```

A new column can be created based on an arithmetic expression involving other columns:

```
SELECT Name, Amount*Num FROM Accounts;
```

A column alias is specified as below:

```
SELECT [column_1 | expression] AS descriptive_name1, [column_1 | expression] AS descriptive_name2 …. FROM table_name;
```

The keyword AS is optional and can be omitted, but helps improve readability.

To rename existing columns, for e.g. when these columns are not appropriately or clearly named:

```
SELECT name AS `Customer Name`, Num AS RealNumber FROM Accounts;
```

Accent grave / back ticks (and NOT single quotes) are used to enclose aliases with spaces in them. This is important if these aliases are going to be reused again in a later part of the query, so conventionally all aliases are enclosed with back ticks even if they not have spaces in them.

Aliases also provide a way of specifying a name or header for calculated field columns:

```
SELECT Name, Amount*Num AS Total FROM Accounts;
```

Aliases can also be specified for tables, again for situations when existing tables are not appropriately named:

```
SELECT name FROM Accounts AS A;
```

They are mainly used when there are multiple tables involved in a query and there is a need to clearly distinguish between the different tables and at the same time provide a simple way to refer to these tables. In that case, the table alias is used as a prefix for selected columns within the respective tables. Such a use of the table alias is typically found in:

* Subqueries involving two or more tables
* Join operations involving two or more tables

By default, the ORDER BY clause sorts the result set in ascending order if you don’t specify ASC or DESC explicitly.

```
SELECT name, month, amount FROM Accounts ORDER BY amount;
```

```
SELECT name, month, amount FROM Accounts ORDER BY amount DESC;
```

Sorting on multiple columns:

```
SELECT name, month, amount FROM Accounts ORDER BY amount DESC, month ASC;
```

Sorting on a column alias:

```
SELECT name, month, amount*num AS Total FROM Accounts ORDER BY Total DESC;
```

Using FIELD:

```
SELECT name, month, amount FROM Accounts ORDER BY FIELD(month, 'Jan','Feb','Mar', 'Apr');
```

***Sort Order***

1. When a column is sorted in ascending order, any fields within it with NULL appear first. After NULLs, numbers will appear before strings. For columns sorted in descending order, the sequence is reversed: strings will display first, followed by numbers and the finally NULLs.
2. For strings, there is no differentiation between upper- and lowercase characters. An e is treated the same as an E.
3. For strings, the characters comprising the value are evaluated from left to right.

Filtering with WHERE:

```
SELECT name, month, amount FROM Accounts WHERE name = 'Susan';
```

```
SELECT name, month, amount*num AS Total FROM Accounts WHERE amount*num < 210 AND name = 'Susan';
```

```
SELECT name, amount from Accounts WHERE amount BETWEEN 30 AND 70;
```

```
SELECT name, month from Accounts WHERE month = 'Jan' OR month = 'Feb' OR month = 'Mar';
```

```
SELECT name, month from Accounts WHERE month IN ('Jan', 'Feb', 'Mar');
```

Pattern matching with LIKE:

The LIKE operator is used to select data based on pattern matching. It uses a 2 wildcard characters to match a wide range of different alphanumeric sequences.

* The percentage ( % ) wildcard allows you to match any string of zero or more characters.
* The underscore ( _ ) wildcard allows you to match any single character.

```
SELECT name FROM Accounts WHERE name LIKE '%a%';
```

```
SELECT name FROM Accounts WHERE name LIKE '_us__';
```

LIMIT:

```
SELECT name, month, amount FROM Accounts ORDER BY amount DESC LIMIT 5;
```

Loading data using a query result:

```
CREATE TABLE Person(firstname STRING, Birthmonth STRING, age INT) ROW FORMAT DELIMITED FIELDS TERMINATED BY ',';
```

```
INSERT INTO TABLE Person SELECT name, month, amount from Accounts WHERE Num > 4;
```

```
INSERT OVERWRITE TABLE Person SELECT name, month, amount FROM Accounts WHERE month = 'Feb';
```

DISTINCT:

```
SELECT DISTINCT name FROM Accounts;
```

```
SELECT DISTINCT name, account FROM Accounts;
```

SUM, AVG, MIN, MAX:

```
SELECT SUM(Amount) FROM Accounts;
```

```
SELECT SUM(DISTINCT Amount) FROM Accounts;
```

```
SELECT SUM(Num) AS Result FROM Accounts WHERE Name = 'Peter';
```

```
SELECT SUM(Num+10) AS Total FROM Accounts WHERE month = 'Jan';
```

```
SELECT AVG(Num) AS Average FROM Accounts;
```

```
SELECT MAX(Amount) AS Maximum FROM Accounts WHERE Name = 'Peter';
```

COUNT:

* COUNT(*) returns the number of values in a result set returned by a SELECT statement, regardless of whether these values contain a NULL or not.*
* COUNT(expression) performs like COUNT(*), except it only counts values which are not NULL. The expression can be a column or a calculated field.
* COUNT(DISTINCT expression) counts only distinct values in the result set which are not NULL.

```
SELECT COUNT(*) AS TotalTransactions FROM Accounts;
```

```
SELECT COUNT(DISTINCT Name) AS TotalCustomers FROM Accounts;
```

GROUP BY:

```
SELECT Name FROM Accounts GROUP BY Name;
```

Without the use of aggregation functions, GROUP BY produces the same result as DISTINCT. The query below produces the same result as the above.

```
SELECT DISTINCT name FROM Accounts;
```

```
SELECT Name FROM Accounts GROUP BY Name ORDER BY Name DESC;
```

As each row represents a unique transaction by an individual, the query below counts all transactions associated with each unique individual name:

```
SELECT name, COUNT(*) AS CustomerTransaction FROM Accounts GROUP BY name;
```

To get the sum of all amounts transacted in each unique month:

```
SELECT month, SUM(amount) AS MonthTotal FROM Accounts GROUP BY month ORDER BY FIELD(month, 'Jan','Feb','Mar', 'Apr');
```

When two or more columns are specified in the GROUP BY clause, the grouping is first done on the basis of the first column, followed by the second column and so on. For e.g. the query below counts the number of transactions for each account category associated with each unique individual:

```
SELECT name, account, COUNT(*) AS NumTransactions FROM Accounts GROUP BY name, account;
```

HAVING:

If the WHERE clause is used, the condition specified will filter the rows from the specified table first, which are then subsequently grouped based on the GROUP BY clause. For e.g. the statement below only sums transaction amounts which are higher than 60 from all customers:

```
SELECT name, SUM(amount) AS CustomerTotal FROM Accounts WHERE amount > 60 GROUP BY name;
```

To apply selection criteria to the result set returned by the GROUP BY, use the HAVING keyword instead. For e.g to obtain the total transactions first for each individual customer and then filter on this total instead, use:

```
SELECT name, SUM(amount) AS CustomerTotal FROM Accounts GROUP BY name HAVING CustomerTotal > 60;
```

Combining Tables:

***Inner Join***

```
SELECT * FROM Customers INNER JOIN Orders ON Customers.CustomerID = Orders.CustomerID;
```

```
SELECT C.CustomerID AS `ID`, C.Name, O.OrderID AS `OrderNum`, O.Quantity AS `Qty`, O.Price
FROM Customers AS C INNER JOIN Orders AS O
ON C.CustomerID = O.CustomerID;
```

```
SELECT C.CustomerID `ID`, C.Name, O.OrderID `OrderNum`, O.Quantity `Qty`, O.Price
FROM Customers C INNER JOIN Orders O
ON C.CustomerID = O.CustomerID;
```

```
SELECT C.CustomerID, C.Name, O.OrderID , O.Quantity
FROM Customers AS C INNER JOIN Orders AS O
ON C.CustomerID = O.CustomerID WHERE C.Name = 'Peter';
```

```
SELECT C.Name, COUNT(*) AS `Times`, SUM(O.Quantity) AS `Total Quantity`
FROM Customers AS C INNER JOIN Orders AS O
ON C.CustomerID = O.CustomerID GROUP BY C.Name ORDER BY `Total Quantity` DESC;
```

If there are multiple INNER JOINs in a query, the second INNER JOIN applies to the result set of the first INNER JOIN:

```
SELECT C.Name, O.OrderID, O.Quantity AS `Qty`, R.Amount AS `Refund` FROM Customers AS C INNER JOIN Orders AS O
ON C.CustomerID = O.CustomerID
INNER JOIN Refunds AS R
ON R.OrderID = O.OrderID;
```

***Left Join***

```
SELECT C.CustomerID AS `ID`, C. Name, O.OrderID, O.Quantity AS `Qty`, O.Price
FROM Customers AS C LEFT JOIN Orders AS O
ON C.CustomerID = O.CustomerID;
```

***Right Join***

```
SELECT C.CustomerID AS `ID`, C.Name, O.OrderID, O.Quantity AS `Qty`, O.Price
FROM Customers AS C RIGHT JOIN Orders AS O
ON C.CustomerID = O.CustomerID;
```

***Full Join***

```
SELECT C.CustomerID AS `ID`, C.Name, O.OrderID, O.Quantity AS `Qty`, O.Price
FROM Customers AS C FULL JOIN Orders AS O
ON C.CustomerID = O.CustomerID;
```

IS NULL:

```
SELECT C.CustomerID AS `ID`, C.Name, O.Quantity AS `Qty`
FROM Customers AS C LEFT JOIN Orders AS O
ON C.CustomerID = O.CustomerID WHERE O.Quantity IS NULL;
```

IS NOT NULL:

```
SELECT C.Name, O.OrderID, O.Quantity*O.Price-R.Amount AS `LOSS`
FROM Customers AS C
LEFT JOIN Orders AS O
ON C.CustomerID = O.CustomerID
LEFT JOIN Refunds AS R
ON R.OrderID = O.OrderID
WHERE O.OrderID IS NOT NULL AND R.Amount IS NOT NULL;
```

***Self Join***

```
SELECT Employees.Name AS `Employee`, Managers.Name AS `Manager` FROM Personnel AS Employees INNER JOIN Personnel AS Managers
ON Employees.ManagerID = Managers.EmployeeID ORDER BY `Employee`;
```

```
SELECT Employees.Name AS `Employee`, 1stManagers.Name AS `First Manager`, 2ndManagers.Name AS `Second Manager`
FROM Personnel AS Employees
LEFT JOIN Personnel AS 1stManagers ON Employees.ManagerID = 1stManagers.EmployeeID
LEFT JOIN Personnel AS 2ndManagers ON 1stManagers.ManagerID = 2ndManagers.EmployeeID;
```

***Views***

A database view is a virtual or logical table which is defined as the result of a SELECT query. It is conceptually identical to a table, and thus queries and updates can be executed against it just like a normal table.

Hence, when a query or update is executed on a view, this is converted into queries or updates against the base tables. When the view is queried, the view's original clauses or conditions are evaluated before the query on the view. For example, if the query on a view has a limit of 200 and the view was created with a limit of 100, then the query would return 100 rows.

```
CREATE VIEW HighAmountView AS SELECT Name, Account, Amount FROM Accounts WHERE Amount > 40;
```

```
SELECT * FROM HighAmountView;
```

```
SELECT DISTINCT name FROM HighAmountView WHERE account= 'Savings';
```

A view can also be created with its own columns as shown below.

```
CREATE VIEW TotalTransactionView(Name, Total) AS SELECT Name, SUM(amount) FROM Accounts GROUP BY Name;
```

```
SELECT * FROM TotalTransactionsView;
```

```
SHOW CREATE TABLE TotalTransactionsView;
```

Modifying an existing view:

```
ALTER VIEW HighAmountView SET TBLPROPERTIES('user' = 'spiderman')
```

```
SHOW CREATE TABLE HighAmountView;
```

```
SHOW TBLPROPERTIES HighAmountView;
```

```
ALTER VIEW HighAmountView AS ...
```

Removing a view:

```
DROP VIEW HighAmountView;
```

Subqueries:

```
SELECT OrderID, CustomerID, Quantity FROM (
SELECT * FROM Orders WHERE Quantity > 4) AS TempTable
ORDER BY CustomerID;
```

The table returned from the subquery (SELECT * FROM Orders WHERE Quantity > 4) is known as a derived table or materialized subquery. It must have an alias (in this case, TempTable), even if this alias is not used.

```
SELECT Name, `Total Price` FROM Customers AS C
INNER JOIN(
SELECT CustomerID, SUM(Quantity * Price) AS `Total Price FROM Orders GROUP BY Customer ID
) AS TT
ON C.CustomerID = TT.CustomerID ORDER BY `Total Price`;
```

```
SELECT C.Name, SUM(O.Quantity * O.Price) AS `Total Price`
FROM Customers AS C INNER JOIN Orders AS O
ON C.CustomerID = O.CustomerID
GROUP BY C.Name ORDER BY `Total Price`;
```

```
CREATE VIEW TotalSum AS SELECT CustomerID, SUM(Quantity * Price) AS `Total Price` FROM Orders GROUP BY CustomerID;

SELECT Name,TT.`Total Price` FROM Customers AS C
INNER JOIN TotalSum AS TT
ON C.CustomerID = TT.CustomerID ORDER BY TT.`Total Price`;
```

Subquery is part of a condition:

```
SELECT Name FROM Customers WHERE CustomerID IN (
SELECT CustomerID FROM Orders GROUP BY CustomerID HAVING Sum(Quantity) > 7);
```

```
SELECT C.Name FROM Customers AS C INNER JOIN Orders AS O
ON C.CustomerID = O.CustomerID GROUP BY C.Name HAVING Sum(O.Quantity) > 7;
```

EXISTS:

The EXISTS operator is a Boolean operator which returns true if the subquery returns a result (or matching row) when it is executed and false otherwise.

In addition, the EXISTS operator terminates further processing immediately once it finds a matching row, which helps to improve the performance of the query in some cases.

```
SELECT Name FROM Customers WHERE EXISTS
(SELECT * FROM Orders WHERE Customers.CustomerID = Orders.CustomerID);
```



