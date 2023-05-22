# PostgreSQL
### Create & drop database/table
```
CREATE DATABASE database;
CREATE TABLE table(col DATATYPE CONSTRAINTS);
DROP TABLE table;
```
### Add & delete columns in a table 
```
ALTER TABLE table ADD COLUMN col;
ALTER TABLE table DROP COLUMN col;
ALTER TABLE table ADD COLUMN col DATATYPE REFERENCES referenced_table(referenced_col);
```
```
ALTER TABLE table ADD UNIQUE(col); 
```
ensure each value in the column has no duplicates
```
ALTER TABLE table ADD COLUMN col SET NOT NULL; 
```
ensure the value is not empty
### Renaming
```
ALTER TABLE table RENAME COLUMN col TO new_col;
ALTER DATABASE database RENAME TO new_database;
```
### Alter data in a table 
```
INSERT INTO table(col1, col2) VALUES(val1, val2),(val3, val4),(...);
DELELTE FROM table WHERE <condition>;
UPDATE table SET col=new_val WHERE <condition>
```

### Selecting
```
SELECT * FROM table;
SELECT a,b FROM table;
SELECT * FROM table ORDER BY col;
```
```
SELECT * FROM table1 FULL JOIN table2 ON table1.primaryKeyCol = table2.primaryKeyCol; 
```
shows one-to-one relationships

### Constraints
```
ALTER TABLE table ADD PRIMARY KEY(col1, col2);
ALTER TABLE table DROP CONSTRAINTS name;
ALTER TABLE table ADD FOREIGN KEY(col) REFERENCES table(col);
```
