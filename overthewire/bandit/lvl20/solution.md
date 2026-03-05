**`setuid`** (set user identity) — allows you to execute a file with the **privileges of the file owner**, not the current user.
Run the following commands:  
```bash
ls
# the name of the desired file can be seen

file bandit20-do
# confirms that the file is executable

./bandit20-do
# no arguments, as the challenge states
```
`./` is used together with an executable file in order to run it from the current directory.  
The outputted information shows that the file is used to read other files as owner.
```bash
./bandit20-do
cat /etc/bandit_pass/bandit20
```
