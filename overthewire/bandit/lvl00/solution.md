Use the **ssh** command, whose manual pages you can find here: [ssh(1) - Linux man page](https://linux.die.net/man/1/ssh)  
Upon checking the synopsis, you can add a port flag and a destination.  
"**-p** port  
Port to connect to on the remote host. This can be specified on a per-host basis in the configuration file"   
The question asks us to log through port 2220, so we use *-p 2220*, and the destination is *bandit0@bandit.labs.overthewire.org*.  
  
#Log into destination through the specified port  
`ssh -p 2220 bandit0@bandit.labs.overthewire.org`  
  
Write “bandit0” when asked for the password.
