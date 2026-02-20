You do the same as you did in the previous 2 levels to get to the content of “cronjob_bandit24” and this is its content:
“
#!/bin/bash

cd /var/spool/$myname/foo - you change directory to /var/spool/bandit24/foo (cause this is the password you are looking for
echo "Executing and deleting all scripts in /var/spool/bandit24/foo:" - this cronjob executes and deletes all scripts, so whenever you add it, it will be deleted
for i in * .*; - all the documents in a folder
do
    if [ "$i" != "." -a "$i" != ".." ]; - if the name is not “.” (Current directory) or “..” (Parent directory)
    then
        echo "Handling $i"
        owner=‘$(stat --format “%U” ./$I)’ - %U is the name of the owner and it will be formatted throughout stat to have the name in it
        if [ "${owner}" = "bandit24” ];
            timeout -s 9 60 ./$i - sets a 60 second timer to 9, which is to sigkill, killing the document
        fi
        rm -f ./$i - force removes it
    fi
done
“
So the best way to solve this is to create a temporary directory with mktemp -d, where you create the script to retrieve the password from “cat /etc/bandit_pass/bandit24” and copy the content of this script file (which you have to make as executable using “chmod +x”) into the “/var/spool/$myname/foo” directory
Keep checking with ls and then if you redirected all the commands to a new file, a new file should pop up in tmp, which you then cat to get the password.

#!/bin/bash

cat /etc/bandit_pass/bandit24 > /tmp/tmp.mtqU03S7tz/myscript.sh
