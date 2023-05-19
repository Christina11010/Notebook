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
ALTER TABLE table ADD COLUMN col DATATYPE REFERENCE referenced_table(referenced_col);
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

### Add titles to your script 
```
echo -e "\n~~ Questionnaire ~~\n"
```

### Actions for a file
```
chmod <permission> <filename>
```
Give a file permissions to do sth.
```
chmod +x <filename>
```
give a file executable permissions

```
which bash
#!<path to interpreter>
```
see the path to bash
e.g. #!/bin/bash
```
./<filename>
```
execute script in a file
```
ls /
```
list what's in the root of the file system
```
cat <filename>
```
print the content of a file 

### Arrays
```
ARR=("a", "b", "c") -> create an array
echo ${ARR[@]} -> print whole array
echo ${ARR[index]} -> print variable at a specific index
```

### Create a random number
```
N=$(( RANDOM % 6 ))
```
thic create a random number from 1 to 5

### Other actions for printing
an example:
here adding (( ... )) doesn't change the variable itself, it performs a calculation but output nothing.
$(( ... )) replace the calculation with the result of it.
```
J=5
echo $(( J * 5 + 25 ))
50
echo $J
5
```

## Functions

### if argument:
```
if [[ condition ]]
then
  statement
elseif
then
  statment
else 
  statement
fi 
```

### while loop
```
while [[ condition ]]
do
  statement
done 
```

### Create a function
```
FUNCTION() {
  statement
}
```
# SQL
### Query database using PSQL variable
```
PSQL="psql -X --username=freecodecamp --dbname=students --no-align --tuples-only -c"
$($PSQL "<query_here>")
```
query looks like this: 
```
SELECT col FROM table WHERE <condition>
e.g. MAJOR_ID=$($PSQL "SELECT major_id FROM majors WHERE major='$MAJOR'")
```

### Delete all the data in tables.
Need to truncate any tables that uses a col from it as a foreign key at the same time 
```
TRUNCATE table1, table2, table3
```

### Shell script for parsing a comma-separated file 
Uses "cat" and "read" commands to process a file with comma-seperated values. It reads the content of a file specified by <filename> and uses the "|" (pipe) operator to send the output of the "cat" command to the "read" command, which is used to read the content of each line and assign it to the variables  
The "IFS" (internal field separatorr) variable is set to "," to indicate that the field are separated by commas
```
cat students_test.csv | while IFS="," read VAR1 VAR2 VAR3 VAR4
do
  echo $FIRST
done
```
  You can add placeholders to the field where you do not want to assign a variable to, like this:
```
cat student_test.csv | while INF="," read VA1 _ VAR2 _ _ VAR3
```
  here it skips the 2nd, 4th and 5th field.

### Steps to adding data to a file
```
  cat courses_test.csv | while IFS="," read MAJOR COURSE
  do
  if [[ $MAJOR != "major" ]]
  then
    # get major_id
    MAJOR_ID=$($PSQL "SELECT major_id FROM majors WHERE major='$MAJOR'")

    # if not found
    if [[ -z $MAJOR_ID ]]
    then
      # insert major
      INSERT_MAJOR_RESULT=$($PSQL "INSERT INTO majors(major) VALUES('$MAJOR')")
      if [[ $INSERT_MAJOR_RESULT == "INSERT 0 1" ]]
      then
        echo Inserted into majors, $MAJOR
      fi

      # get new major_id
      MAJOR_ID=$($PSQL "SELECT major_id FROM majors WHERE major='$MAJOR'")
    fi

    # get course_id
    COURSE_ID=$($PSQL "SELECT course_id FROM courses WHERE course='$COURSE'")

    # if not found
    if [[ -z $COURSE_ID ]]
    then
      # insert course
      INSERT_COURSE_RESULT=$($PSQL "INSERT INTO courses(course) VALUES('$COURSE')")
      if [[ $INSERT_COURSE_RESULT == "INSERT 0 1" ]]
      then
        echo Inserted into courses, $COURSE
      fi

      # get new course_id
      COURSE_ID=$($PSQL "SELECT course_id FROM courses WHERE course='$COURSE'")
    fi

    # insert into majors_courses
    INSERT_MAJORS_COURSES_RESULT=$($PSQL "INSERT INTO majors_courses(major_id, course_id) VALUES($MAJOR_ID, $COURSE_ID)")
    if [[ $INSERT_MAJORS_COURSES_RESULT == "INSERT 0 1" ]]
    then
      echo Inserted into majors_courses, $MAJOR : $COURSE
    fi
  fi
done
```
 
### Wildcard
% = any # of characters, _ = one character
```
SELECT * FROM client WHERE client_name LIKE '%A';
SELECT * FROM client WHERE client_name LIKE '%A%';
```
the 1st will select all with name ends with "A"
the 2nd will select all with name that has "A" in it somewhere
```
SELECT * FROM employee WHERE birth_date LIKE '____-10%';
```
This will find all employee with date of birth in October. The 4 "_" is any 4 characters (in this case, the 4 numbers that makes up year)  

```ILIKE``` & ```NOT ILIKE``` are for non-case sensitive patterns 
  
Combining 2 patterns together
```
SELECT * FROM courses WHERE course NOT ILIKE '<pattern>' AND course LIKE <pattern> ;
```

Access empty datapoint using ```IS NULL``` like this: ```WHERE <column> IS NULL``` or access non-empty data using ```IS NOT NULL```
  
Specify the ascending order (ASC) by adding ```ORDER BY col``` at the end of a query, for example: ```SELECT * FROM student ORDER BY pga```. Adding ```DESC``` at the end of the query for descending order. You can also order it by multiple rules, like this: ```ORDER BY col1, col2, col3;``` This will order it based on col1 first, then col2, then col3.
  
Limit the certain number of rows returned by adding ```LIMIT <number>``` at the end of the query
  
```WHERE``` goes first, then ```ORDER BY``` then ```LIMIT```
 
Mathematic functions
```SELECT MIN(col) FROM table``` finds the lowest value in the column. ```MAX()``` for maximum value. 

```SUM()``` for sum, ```AVG()``` for average
  
Round up decimals with ```CEIL()```, round down decimals with ```FLOOR()```, round to the nearest whole number with ```ROUND```, round to a specific no. of decimal places with ```ROUND(<number_to_round>, <decimal_places>)```

  count total number of entries there are in a table for a col. with ```SELECT COUNT(*) FROM col_name``` or ```SELECT COUNT(col) FROM table```
  
  ```DISTINCT``` shows only the unique values, use it like: ```DISTINCT(col)```, or similarly with ```GROUP BY col```. With ```GROUP BY col``` you can add any other aggregate functions (```MIN```, ```MAX```, etc.). For example, to view the major_id col and no. of students in each major_id col, ```SELECT major_id, COUNT(*) FRORM students GROUP BY major_id;``` Another option: ```SELECT <column> FROM <table> GROUP BY <column> HAVING <condition>```
  
  Rename a column: SELECT <column> AS <new_column_name>```
  
  An example with the above: 
  
```echo -e "\nMajor ID, total number of students in a column named 'number_of_students', and average GPA rounded to two decimal places in a column name 'average_gpa', for each major ID in the students table having a student count greater than 1:"
  
echo "$($PSQL "SELECT major_id, COUNT(*) AS number_of_students, ROUND(AVG(gpa), 2) AS average_gpa FROM students GROUP BY major_id HAVING COUNT(*) > 1")"

Result => 
  |8|2.97
41|6|3.53
38|4|2.73
36|6|2.92
37|6|3.38
  ```
### Union
```
SELECT col1 FROM table1 UNION SELECT col2 FROM table2;
```
### Join
```
SELECT col1, col2, col3 FROM table JOIN joined_col ON <condition>;
```
joined_col is the column that both tables have in common. 
the 2 tables are combined when the <condition> is met for certain rows 
 
The majors and students table are linked with the major_id foreign key. If you want to see the name of a major that a student is taking, you need to JOIN the two tables into one. Like this: 
  
  ```SELECT * FROM <table_1> FULL JOIN <table_2> ON <table_1>.<foreign_key_column> = <table_2>.<foreign_key_column>;```
  
  An example: 
  ```SELECT * FROM students FULL JOIN majors ON students.major_id = majors.major_id;```
  
  Outcome: 
  ```
  students=>                                                 
+------------+------------+--------------+----------+-----+----------+-------------------------+
| student_id | first_name |  last_name   | major_id | gpa | major_id |          major          |
+------------+------------+--------------+----------+-----+----------+-------------------------+
|          6 | Rhea       | Kellems      |       36 | 2.5 |       36 | Database Administration |
|          7 | Emma       | Gilbert      |          |     |          |                         |
|          8 | Kimberly   | Whitley      |       37 | 3.8 |       37 | Web Development         |
|          9 | Jimmy      | Felipe       |       36 | 3.7 |       36 | Database Administration |
...
|         34 | Aliya      | Gulgowski    |       42 | 2.6 |       42 | System Administration   |
|         35 | Ana        | Tupajic      |       38 | 3.1 |       38 | Data Science            |
|         36 | Hugo       | Duran        |          | 3.8 |          |                         |
|            |            |              |          |     |       39 | Network Engineering     |
|            |            |              |          |     |       40 | Computer Programming    |
+------------+------------+--------------+----------+-----+----------+-------------------------+
(33 rows)
```
If switch the positiono of students table and major table: ```SELECT * FROM majors FULL JOIN students ON students.major_id = majors.major_id;```

  Result:
  ```
  students=>                                                 
+----------+-------------------------+------------+------------+--------------+----------+-----+
| major_id |          major          | student_id | first_name |  last_name   | major_id | gpa |
+----------+-------------------------+------------+------------+--------------+----------+-----+
|       36 | Database Administration |          6 | Rhea       | Kellems      |       36 | 2.5 |
|          |                         |          7 | Emma       | Gilbert      |          |     |
|       37 | Web Development         |          8 | Kimberly   | Whitley      |       37 | 3.8 |
|       36 | Database Administration |          9 | Jimmy      | Felipe       |       36 | 3.7 |
|          |                         |         10 | Kyle       | Stimson      |          | 2.8 |
|       41 | Game Design             |         11 | Casares    | Hijo         |       41 | 4.0 |
|          |                         |         12 | Noe        | Savage       |          | 3.6 |
...
|       37 | Web Development         |         33 | Dejon      | Howell       |       37 | 4.0 |
|       42 | System Administration   |         34 | Aliya      | Gulgowski    |       42 | 2.6 |
|       38 | Data Science            |         35 | Ana        | Tupajic      |       38 | 3.1 |
|          |                         |         36 | Hugo       | Duran        |          | 3.8 |
|       39 | Network Engineering     |            |            |              |          |     |
|       40 | Computer Programming    |            |            |              |          |     |
+----------+-------------------------+------------+------------+--------------+----------+-----+
(33 rows)
  ```
  
```
SELECT * FROM students LEFT JOIN majors ON students.major_id = majors.major_id;
  ```
"left join" combines only the NOT NULL rows on the left side of "FULL JOIN", in this case, "students" table; similar goes with ```RIGHT JOIN```; Or ```INNER JOIN```, which returns NOT NULL rows for both table1 and table2
  
  If only joining certain columns: 
  ```
  SELECT major FROM students INNER JOIN majors ON students.major_id = majors.major_id;
students=>               
+-------------------------+
|          major          |
+-------------------------+
| Database Administration |
| Web Development         |
| Database Administration |
| Game Design             |
| Game Design             |
...
| Web Development         |
| Database Administration |
| Web Development         |
| System Administration   |
| Data Science            |
+-------------------------+
(23 rows)
  ```
  Without deplicates, add DISTINCT(), like this: 
  
  ```
students=> SELECT DISTINCT(major) FROM students INNER JOIN majors ON students.major_id = majors.major_id;
students=>               
+-------------------------+
|          major          |
+-------------------------+
| Database Administration |
| Game Design             |
| Data Science            |
| System Administration   |
| Web Development         |
+-------------------------+
(5 rows)
  ```

```USING``` to join tables if the foreign key column has the same name in both tables.
  
  ```SELECT * FROM <table_1> FULL JOIN <table_2> USING(<column>);```
  
  You can add a third table to a join like this:  
  ```SELECT * FROM <table_1> FULL JOIN <table_2> USING(<column>) FULL JOIN <table_3> USING(<column>)``` 
 
  This example will join the first two tables into one, turning it into the left table for the second join.
