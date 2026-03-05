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
The script assigns the current user to a variable - myname=$(whoami)  
Since the cron job runs as bandit23, the variable resolves to - myname = bandit23    
A filename is generated using an MD5 hash, substituting the username, which produces a hash `8ca319486bfbbc3663ea0fbe81326349`. Thus, the target filename becomes `/tmp/8ca319486bfbbc3663ea0fbe81326349`.  
Direct access to `/etc/bandit_pass/bandit23` is restricted. However, the cron job copies its contents into `/tmp`, which is world-readable. The password can therefore be retrieved with `cat /tmp/8ca319486bfbbc3663ea0fbe81326349`.
