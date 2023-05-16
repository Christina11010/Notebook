# PostgreSQL
Create & drop database/table
```
CREATE DATABASE database;
CREATE TABLE table(col DATATYPE CONSTRAINTS);
DROP TABLE table;
```
Add & delete columns in a table 
```
ALTER TABLE table ADD COLUMN col;
ALTER TABLE table DROP COLUMN col;
ALTER TABLE table ADD COLUMN col DATATYPE REFERENCE referenced_table(referenced_col);
ALTER TABLE table ADD UNIQUE(col); -> ensure each value in the column has no duplicates
ALTER TABLE table ADD COLUMN col SET NOT NULL; -> ensure the value is not empty 
```
Renaming
```
ALTER TABLE table RENAME COLUMN col TO new_col;
ALTER DATABASE database RENAME TO new_database;
```
Alter data in a table 
```
INSERT INTO table(col1, col2) VALUES(val1, val2),(val3, val4),(...);
DELELTE FROM table WHERE <condition>;
UPDATE table SET col=new_val WHERE <condition>
```
Selecting
```
SELECT * FROM table;
SELECT a,b FROM table;
SELECT * FROM table ORDER BY col;
SELECT * FROM table1 FULL JOIN table2 ON table1.primaryKeyCol = table2.primaryKeyCol; -> shows one-to-one relationships
```
Constraints
```
ALTER TABLE table ADD PRIMARY KEY(col1, col2);
ALTER TABLE table DROP CONSTRAINTS name;
ALTER TABLE table ADD FOREIGN KEY(col) REFERENCES table(col);
```

# Bash Scripting
### Basic actions
```
$VAR="" -> create a variable (no space on 2 sides of the equal sign)
echo -> print
read -> accept input from users 
```

an example: 
```
$QUESTIOIN="What's your name?" 
echo $QUESTION
read NAME
echo Hello $NAME
```

Add titles to your script 
```
echo -e "\n~~ Questionnaire ~~\n"
```

Actions for a file
```
chmod <permission> <filename> -> Give a file permissions to do sth.
chmod +x <filename> -> give a file executable permissions
which bash -> see the path to bash
#!<path to interpreter> -> e.g. #!/bin/bash
./<filename> -> execute script in a file
ls / -> list what's in the root of the file system
cat <filename> -> print the content of a file 
```

Arrays
```
ARR=("a", "b", "c") -> create an array
echo ${ARR[@]} -> print whole array
echo ${ARR[index]} -> print variable at a specific index
```

Create a random number
```
N=$(( RANDOM % 6 )) -> thic create a random number from 1 to 5
```

Other actions for printing, an example:
here adding (( ... )) doesn't change the variable itself, it performs a calculation but output nothing.
$(( ... )) replace the calculation with the result of it.
```
J=5
echo $(( J * 5 + 25 ))
50
echo $J
5
```

### Functions

if argument:
```
if (( condition ))
then
  statement
elseif
then
  statment
else 
  statement
fi 
```

while loop
```
while [[ condition ]]
do
  statement
done 
```

Create a function
```
FUNCTION() {
  statement
}
```
# SQL
Query database using PSQL variable
```
PSQL="psql -X --username=freecodecamp --dbname=students --no-align --tuples-only -c"
$($PSQL "<query_here>")
query looks like this: 
SELECT col FROM table WHERE <condition>
e.g. MAJOR_ID=$($PSQL "SELECT major_id FROM majors WHERE major='$MAJOR'")
```
