# Bash Scripting
## Basic actions
* ```$VAR=""``` to create a variable (no space on 2 sides of the equal sign)
* ```echo``` to print
* ```read``` to accept input from users, an example: 
```
$QUESTIOIN="What's your name?" 
echo $QUESTION
read NAME
echo Hello $NAME
```
* Add titles to your script, like this: ```echo -e "\n~~ Questionnaire ~~\n"```

### Actions for a file
* Give a file permissions to do sth: ```chmod <permission> <filename>```
* give a file executable permissions: ```chmod +x <filename>```
* see the path to bash: 
```
which bash
#!<path to interpreter>
```
e.g. #!/bin/bash
* execute script in a file: ```./<filename>```
* list what's in the root of the file system: ```ls /```
* print the content of a file: ```cat <filename>```

### Arrays
* ```ARR=("a", "b", "c")``` to create an array
* ```echo ${ARR[@]}``` to print whole array
* ```echo ${ARR[index]}``` to print variable at a specific index

### Create a random number
* ```N=$(( RANDOM % 6 ))``` to create a random number from 1 to 5

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

## Advanced Bash
* ```<command> > <filename>``` create or overwrite a file;  
* ```>>``` append to the file;  
* redirect ```stderr``` with ```2>```;  
* redirect ```stdout``` with ```1>```;  
* redirect ```stdin``` with ```command < filename_for_stdin``` like this: ```read NAME < name.txt```, this will assign the content of name.txt to the NAME variable  
* set ```stdin``` by the pipe ```|```: use the output from one command as input for another, like this: ```command_1 | command_2```.  This will take the ```stdout``` from command_1 and use it as the ```stdin``` for command_2. e.g.
``` echo Chris | ./script.sh```, will automatically run script.sh with 'Chris' as input  
script.sh:
```
#!/bin/bash
read NAME
echo Hello $NAME
bad_command
```
terminal: 
```
~/project$ echo Chris | ./script.sh
Hello Chris
./script.sh: line 5: bad_command: command not found
```
```
~/project$ echo Chris | ./script.sh 2> stderr.txt
Hello Chris
~/project$ echo Chris | ./script.sh 2> stderr.txt > stdout.txt
```
this will redirect the ```stderr``` output to the ```stderr.txt``` file, rather than showing up in the terminal. you can also redirect the output to ```stdout.txt``` file 
* Use ```grep``` command to search for patterns in text, like this ```grep '<pattern>' <filename>``` or adding flags ```grep <flag> '<pattern>' <filename>```
* ```sed``` can replace text like this: ```sed 's/<pattern_to_replace>/<text_to_replace_it_with>/' <filename>```
* Redirect the input like this: ```sed 's/freecodecamp/f233C0d3C@mp/i' < name.txt```
* output line count with ```wc -l```, e.g. ```grep -o 'cat[a-z]*' kitty_ipsum_1.txt | wc -l```
* flag ```-o``` put all the matches of the pattern on their own line
* ```grep -n 'meow[a-z]*' kitty_ipsum_1.txt | sed -E 's/([0-9]+).*/\1/' >> kitty_info.txt```
  - ```grep -n 'meow[a-z]*' kitty_ipsum_1.txt```: This command uses the grep utility to search for lines in the file ```kitty_ipsum_1.txt``` that match the pattern ```'meow[a-z]*'```. Here's what each part means:
    * ```-n```: It is an option that tells grep to display the line numbers along with the matching lines.
    * ```'meow[a-z]*'```: It is the regular expression pattern being searched. It matches the word "meow" followed by zero or more lowercase letters.
    * ```kitty_ipsum_1.txt```: It is the input file being searched.
  - ```sed -E 's/([0-9]+).*/\1/'```: This command uses the sed utility to perform a substitution operation on each line of input received from the previous command. Here's what each part means:
    * ```-E```: It is an option that tells sed to use extended regular expressions.
    * ```'s/([0-9]+).*/\1/'```: It is the substitution pattern being applied. It captures one or more digits at the beginning of each line and replaces the entire line with the captured digits. Here's the breakdown:
      - ```([0-9]+)```: It is a regular expression pattern that captures one or more digits.
      - ```.*```: It matches the rest of the line after the digits.
      - ```\1```: It represents the first captured group (the digits), and it is used as the replacement.
* ```wc``` command prints info about a file, like this: ```cat kitty_ipsum_2.txt | wc```, it prints out: ```28     307    1678```; 
* ```-l```flag output the number of lines in the file; ```w``` flag for number of words; ```-m``` flag for number of characters
* 3 ways to print outcome with a given input:
  - ```./translate.sh kitty.txt``` using the kitty file as an argument
  - ```./translate.sh < kitty.txt``` with redirection
  - ```cat kityy.txt | ./translate.sh``` with the ```cat``` and pipe method 
