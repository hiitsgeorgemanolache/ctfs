Looked at previous command I learnt throughout the game to see which one can help me connect - ssh, netcat, but “nc localhost 30002” outputted this message: “I am the pincode checker for user bandit25. Please enter the password for user bandit24 and the secret pincode on a single line, separated by a space.” Then I thought it made sense to write a script to automatically write all the possible 10000 options automatically until I hit the correct one.
Looked at the previous scripts in this game and saw “for i in …” and decided to use it myself, but didn’t know how to equalize width by padding with leading zeroes and there were two main options: “for i in {0000 … 9999}” or for “for i in $(seq -w 0 9999)” and then generate the line with the password written before the pincode - like “gb8KRRCsshuZXI0tUuR6ypOFjiZbf3G8 i”. Saw you have to add “do” and “done”, then you have to make it an executable file which I saved as a “.sh” (used “nano” to do all this so far) with “chmod +x …”. The problem that popped up was that it generated all 10000 lines in the “nc” connection without any output from the daemon, so it needed to be interactive to generate all of the verdicts. Then after some thought, I said I should create a “.txt” file where I have all the generated pincodes and create another executable file which takes every line from the text file and sends it to the daemon to check. So the code to generate those files is (called it “pincode_generator.sh”)
“
#!/bin/bash

for i in $(seq -w 0 9999)

do
        echo gb8KRRCsshuZXI0tUuR6ypOFjiZbf3G8 $i >> pincode_possibilities.txt
done
“
So “pincode_possibilities.txt” has all the combinations - “gb8KRRCsshuZXI0tUuR6ypOFjiZbf3G8 0000”, “gb8KRRCsshuZXI0tUuR6ypOFjiZbf3G8 0001”, …, “gb8KRRCsshuZXI0tUuR6ypOFjiZbf3G8 9999” and I will redirect all of this input into the new “.sh” file, which I will write with “nano”. I looked at a website about bash scripting and saw this code reads each line from a file named input.txt and prints it to the terminal: “
while read line
do
  echo $line
done < input.txt
“
Instead of “input.txt”, I used “pincode_possibilities.txt” and then added “nc localhost 30002” as a pipe after the redirection so that “nc” treats the file as stdin, not just dumping the whole file at once. So the code for the file I named “print_possibilities.sh” looks like this: “
#!/bin/bash

while read line 
do 
        echo $line 
done < pincode_possibilities.txt | nc localhost 30002 >> possibilities_output.txt
“
You “chmod +x” then run it with “./“ and will give you the password.
OPTIONAL:
Catch is you won’t see which pincode is the correct one - it will just say “correct” and give you the password, but I wanted to test myself and see if I can do it. After trying other things and over-complicating it, I realised I can easily redirect all of the output in a file that I called “possibilities_output.txt” and “grep -n” wherever I see the word “correct”, as “-n” shows you the line where this pops up.
After doing that, I get - “9164: Correct”, but the first line is “I am the …” and the pinches start from “0000” so you subtract two from the line number to get the actual correct pincode, which is 9162.
