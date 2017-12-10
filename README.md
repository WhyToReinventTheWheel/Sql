# Sql
* Basics https://www.tutorialspoint.com/sql/
  - Drop vs TRUNCATE : If you actually want to remove the table definitions as well as the data, 
	simply drop the tables. TRUNCATE TABLE empties it, but leaves its structure 
	for future data. Removes all rows from a table without logging the individual row 
  - SQL is case insensitive, which means SELECT and select have same meaning in SQL statements. 
  - SELECT column1, column2....columnN FROM table_name;
  - SELECT column1, column2....columnN FROM table_name WHERE CONDITION;
  - SELECT column1, column2....columnN FROM table_name WHERE CONDITION-1 {AND|OR} CONDITION-2;
  - SELECT column1, column2....columnN FROM table_name WHERE column_name IN (val-1, val-2,...val-N);
  - SELECT column1, column2....columnN FROM table_name WHERE column_name BETWEEN val-1 AND val-2;
  - SELECT column1, column2....columnN FROM table_name WHERE column_name LIKE { PATTERN };
  - SELECT column1, column2....columnN FROM table_name WHERE  CONDITION ORDER BY column_name {ASC|DESC};
  - SELECT SUM(column_name) FROM table_name WHERE CONDITION GROUP BY column_name;
  - SELECT COUNT(column_name) FROM table_name WHERE CONDITION;
  - SELECT SUM(column_name) FROM table_name WHERE  CONDITION GROUP BY column_name HAVING (arithematic function condition);
  
  
 
