Run the following commands (similar to the previous level):
```bash
cd /etc/cron.d/
ls -a
#"cronjob_bandit23" can be seen
cat "cronjob_bandit23"
#output:
###
@reboot bandit23 /usr/bin/cronjob_bandit23.sh  &> /dev/null
* * * * * bandit23 /usr/bin/cronjob_bandit23.sh  &> /dev/null
###
cat "/usr/bin/cronjob_bandit23.sh"
#output:
###
#!/bin/bash

myname=$(whoami)
mytarget=$(echo I am user $myname | md5sum | cut -d ' ' -f 1)

echo "Copying passwordfile /etc/bandit_pass/$myname to /tmp/$mytarget"

cat /etc/bandit_pass/$myname > /tmp/$mytarget
###
```
The trick is to put your identity (the “whoami” of the script) as “bandit23” cause you are looking for the password for that level. Run the script command, which means:
basically this function takes the output of the first part- which is "I am user myname" (bandit23 in our case) - and md5sum it, so check and compute the MD5 checksum (so the MD5 value of what was just said and compare it to their checksums), the output will then by cut with -d -f1 which is delimiting by only outputting field 1 - filed start from 1.
*MD5 value is the 128-bit value*
Because you don’t have the permission to open the bandit23 file, you will just cat the /tmp/8ca319486bfbbc3663ea0fbe81326349, which is the result that you will get from doing the “echo I am user …” and get the password -that’s it.
