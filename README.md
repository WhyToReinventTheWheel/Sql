# Sql
* Basics https://www.tutorialspoint.com/sql/
  - DML VS DDL -- DML is abbreviation of Data Manipulation Language. It is used to retrieve, store, modify, 
	delete, insert and update data in database. DDL is abbreviation of Data Definition Language. 
	It is used to create and modify the structure of database objects in database.
  - Drop vs TRUNCATE : If you actually want to remove the table definitions as well as the data, 
	simply drop the tables. TRUNCATE TABLE empties it, but leaves its structure 
	for future data. Removes all rows from a table without logging the individual row 
  - SQL is case insensitive, which means SELECT and select have same meaning in SQL statements. 
  - SELECT column1, column2....columnN FROM table_name;
  - SELECT column1, column2....columnN FROM table_name WHERE CONDITION;
	SELECT ID, NAME, SALARY FROM CUSTOMERS WHERE NAME = 'Hardik';
	SELECT ID, NAME, SALARY FROM CUSTOMERS WHERE SALARY > 2000;
  - SELECT column1, column2....columnN FROM table_name WHERE CONDITION-1 {AND|OR} CONDITION-2;
	SELECT ID, NAME, SALARY FROM CUSTOMERS WHERE SALARY > 2000 AND age < 25;
	SELECT ID, NAME, SALARY FROM CUSTOMERS WHERE SALARY > 2000 OR age < 25;
  - SELECT column1, column2....columnN FROM table_name WHERE column_name IN (val-1, val-2,...val-N);
  - SELECT column1, column2....columnN FROM table_name WHERE column_name BETWEEN val-1 AND val-2;
  - SELECT column1, column2....columnN FROM table_name WHERE column_name LIKE { PATTERN };
	* Finds any values that start with 200.
		WHERE SALARY LIKE '200%'
	* Finds any values that have 200 in any position.
		WHERE SALARY LIKE '%200%'
	* Finds any values that have 00 in the second and third positions.
		WHERE SALARY LIKE '_00%'
	* Finds any values that start with 2 and are at least 3 characters in length.
		WHERE SALARY LIKE '2_%_%'
	* Finds any values that end with 2.
		WHERE SALARY LIKE '%2'
	* Finds any values that have a 2 in the second position and end with a 3.
		WHERE SALARY LIKE '_2%3'
	* Finds any values in a five-digit number that start with 2 and end with 3.
		WHERE SALARY LIKE '2___3'
  - SELECT column1, column2....columnN FROM table_name WHERE  CONDITION ORDER BY column_name {ASC|DESC};
	* ASC is default Order
	SELECT * FROM CUSTOMERS ORDER BY NAME, SALARY;

	
  - SELECT COUNT(column_name) FROM table_name WHERE CONDITION;
  - SELECT SUM(column_name) FROM table_name WHERE  CONDITION GROUP BY column_name HAVING (arithematic function condition);
  - UPDATE table_name SET column1 = value1, column2 = value2...., columnN = valueN WHERE [condition];
	UPDATE CUSTOMERS SET ADDRESS = 'Pune' WHERE ID = 6;
  - DELETE FROM table_name WHERE [condition]; 
	DELETE FROM CUSTOMERS WHERE ID = 6;
	DELETE FROM CUSTOMERS;   // This will delete all records
  - DROP TABLE table_name;
  - TRUNCATE TABLE table_name;
  - SELECT COUNT(*) AS "RECORDS" FROM CUSTOMERS; 
  - SELECT CURRENT_TIMESTAMP;
  - INSERT INTO TABLE_NAME (column1, column2, column3,...columnN)  
		VALUES (value1, value2, value3,...valueN);

# GroupBy
	The SQL GROUP BY clause is used in collaboration with the SELECT statement to arrange 
	identical data into groups. This GROUP BY clause follows the WHERE clause in a SELECT 
	statement and precedes the ORDER BY clause.	
	* SELECT SUM(column_name) FROM table_name WHERE CONDITION GROUP BY column_name;
		SELECT store_id,active,count(*) FROM sakila.customer group by store_id, active;
		store_id	active		count(*)
		1			0			8
		1			1			318
		2			0			7
		2			1			266
	 
# Constraints
	CUSTOMERS table
	
		CREATE TABLE CUSTOMERS(
		   ID   INT              NOT NULL,
		   NAME VARCHAR (20)     NOT NULL,
		   AGE  INT              NOT NULL UNIQUE,
		   ADDRESS  CHAR (25) ,
		   SALARY   DECIMAL (18, 2) DEFAULT 5000.00,       
		   PRIMARY KEY (ID)
		);
	ORDERS table
		CREATE TABLE ORDERS (
		   ID          INT        NOT NULL,
		   DATE        DATETIME, 
		   CUSTOMER_ID INT references CUSTOMERS(ID),
		   AMOUNT     double,
		   PRIMARY KEY (ID)
		);
	
	- Constraints are the rules enforced on the data columns of a table.
	- Constraints could be either on a column level or a table level.
	- NOT NULL − Ensures that a column cannot have NULL value.
	- DEFAULT − Provides a default value for a column when none is specified.
	- UNIQUE − Ensures that all values in a column are different.
	- PRIMARY− Uniquely identifies each row/record in a database table.
		* Primary key column cannot have NULL values.
		* A table can have only one primary key, which may consist of single or multiple fields.
		* multiple fields primary key called composite key.
		* If you use the ALTER TABLE statement to add a primary key, 
			the primary key column(s) should have already been declared 
			to not contain NULL values (when the table was first created).
	- FOREIGN− Uniquely identifies a row/record in any of the given database table.
		* A foreign key is a key used to link two tables together.
		* A Foreign Key is a column or a combination of columns whose values 
			match a Primary Key in a different table.
		* 
	- CHECK− The CHECK constraint ensures that all the values in a column satisfies certain conditions.
	- INDEX − Used to create and retrieve data from the database very quickly.

# Joins
	
	SELECT ID, NAME, AGE, AMOUNT FROM CUSTOMERS, ORDERS WHERE  CUSTOMERS.ID = ORDERS.CUSTOMER_ID;
	
	INNER JOIN − returns rows when there is a match in both tables.
		SELECT  ID, NAME, AMOUNT, DATE FROM CUSTOMERS INNER JOIN ORDERS ON CUSTOMERS.ID = ORDERS.CUSTOMER_ID;
		select C.customer_id, C.first_name, A.address FROM sakila.customer as C , sakila.address as A where C.address_id = A.address_id;
	LEFT JOIN − returns all rows from the left table, even if there are no matches in the right table.
				left join returns all the values from the left table, plus matched values from the right 
				table or NULL in case of no matching join predicate.
		SELECT  ID, NAME, AMOUNT, DATE FROM CUSTOMERS LEFT JOIN ORDERS ON CUSTOMERS.ID = ORDERS.CUSTOMER_ID;		
		
	RIGHT JOIN − returns all rows from the right table, even if there are no matches in the left table.
				right join returns all the values from the right table, plus matched values from the left table 
				or NULL in case of no matching join predicate.
		SELECT ID, NAME, AMOUNT, DATE FROM CUSTOMERS RIGHT JOIN ORDERS ON CUSTOMERS.ID = ORDERS.CUSTOMER_ID;
		
	FULL JOIN − The SQL FULL JOIN combines the results of both left and right outer joins.
				The joined table will contain all records from both the tables and fill in 
				NULLs for missing matches on either side.
		SELECT  ID, NAME, AMOUNT, DATE FROM CUSTOMERS FULL JOIN ORDERS ON CUSTOMERS.ID = ORDERS.CUSTOMER_ID;
	
	SELF JOIN − is used to join a table to itself as if the table were two tables, temporarily renaming at least one table in the SQL statement.
		SELECT  a.ID, b.NAME, a.SALARY FROM CUSTOMERS a, CUSTOMERS b WHERE a.SALARY < b.SALARY;
		SELECT A.Name ,A.Population, B.Name , B.Population FROM world.city A , world.city B where A.Population < B.Population;
				Luanda		2022000	Alger	2168000
				Baku		1787800	Alger	2168000
				Fortaleza	2097757	Alger	2168000
				BrasÃ­lia	1969868	Alger	2168000
			
	CARTESIAN JOIN/CROSS JOIN − returns the Cartesian product of the sets of records from the two or more joined tables.
			SELECT  ID, NAME, AMOUNT, DATE FROM CUSTOMERS, ORDERS;