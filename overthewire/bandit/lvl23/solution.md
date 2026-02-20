Look in /etc/cron.d/ by using “cd”. Once you’re in the directory, type “ls” (with “-a” for example), and you will see “cronjob_bandit23”, which you will then “cat”. Read the content and you will see “/usr/bin/cronjob_bandit23.sh”, the rest is exactly like the previous level. You “cat” that file and you will see the script. The trick is to put your identity (the “whoami” of the script) as “bandit23” cause you are looking for the password for that level. Run the script command, which means:
echo I am user $myname | md5sum | cut -d ' ' -f 1 — basically this function takes the output of the first part- which is "I am user myname" (bandit23 in our case) - and md5sum it, so check and compute the MD5 checksum (so the MD5 value of what was just said and compare it to their checksums), the output will then by cut with -d -f1 which is delimiting by only outputting field 1 - filed start from 1
*MD5 value is the 128-bit value*
“
myname=$(whoami)
mytarget=$(echo I am user $myname | md5sum | cut -d ' ' -f 1)

echo "Copying passwordfile /etc/bandit_pass/$myname to /tmp/$mytarget
“
Because you don’t have the permission to open the bandit23 file, you will just cat the /tmp/8ca319486bfbbc3663ea0fbe81326349, which is the result that you will get from doing the “echo I am user …” and get the password -that’s it.
