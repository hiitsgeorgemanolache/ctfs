Use the `ssh` command, which also has an `-i` flag that selects a file from which the identity (private key) for RSA or DSA authentication is read.  
Bare in mind the usual protocol requirements still apply, so you need to add `-p 2220`.  
```bash
ssh -i sshkey.private -p 2220 bandit14@localhost
cat /etc/bandit_pass/bandit14
```
