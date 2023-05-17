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
SELECT * FROM client WHERE client_name LIKE '%LLC';
SELECT * FROM client WHERE client_name LIKE '%LLC%';
```
the 1st will select all with name ends with "LLC"
the 2nd will select all with name that has "LLC" in it somewhere
```
SELECT * FROM employee WHERE birth_date LIKE '____-10%';
```
This will find all employee with date of birth in October. The 4 "_" is any 4 characters (in this case, the 4 numbers that makes up year)
  
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
```
SELECT col1, col2, col3 FROM table LEFT JOIN joined_col ON <condition>;
```
"left join" combines all rows in the result, but only the correct rows has not null result
