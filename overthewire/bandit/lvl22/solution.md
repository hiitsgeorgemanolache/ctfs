Look in /etc/cron.d/ by usingg “cd”. Once you’re in the directory, type “ls” (with “-a” for example), and you will see “cronjob_bandit23”, which you will then “cat”.  @reboot bandit22 /usr/bin/cronjob_bandit22.sh  &> /dev/null
* * * * * bandit22 /usr/bin/cronjob_bandit22.sh  &> /dev/null is the content - means that every minute, the cron checks for … and both stderror and stdoutput go to dev/null, which is a trash can for data (anything you send there just disappears). The “cron” job is run once at system start-up as user bandit22
* * * * * → this is the cron schedule format (minute, hour, day, month, weekday).
* A * means “every possible value.”
* So * * * * * = every minute of every hour, every day
Then, you “cat” cronjob_bandit22.sh” and you will see a tmp folder, which you then “cat” to retrieve the password.
