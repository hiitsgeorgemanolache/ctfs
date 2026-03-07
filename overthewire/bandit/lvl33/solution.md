## Escaping the Uppercase Shell

The objective of this level is to escape a restricted uppercase shell and obtain the final confirmation message.
Upon logging into the level, the system immediately launches a program called `uppershell`. This environment behaves differently from a standard shell:  
- most commands return *permission denied* or *command not found*  
- standard utilities such as `ls`, `cat`, and `cd` cannot be executed  
- the `exit` command does not function normally   
These restrictions indicate that the user is interacting with a **wrapper program** rather than a normal shell environment.

### Behaviour of `uppershell`

Inside `uppershell`, all user input is automatically converted to **uppercase** before being processed. 
While alphabetic characters are transformed, several characters remain unaffected by the conversion, including:  
- digits (`0–9`)  
- many punctuation characters  
- shell variable syntax (`$`)  
These characters can be used to interact with the underlying shell.

### Using Shell Variable Expansion

In Unix-like shells:  
- `$0` represents the name of the currently executing shell or script  
- Variable expansion occurs **before command execution**  
Entering `$0` causes the shell to execute whatever program *$0* resolves to. This effectively spawns a new shell and bypasses the uppercase restriction.
After escaping the restricted shell, the home directory can be explored normally:
```bash
cd ~
ls
#output: README.txt
cat README.txt
```
This produces the final message confirming successful completion of the Bandit wargame:
```bash
Congratulations on solving the last level of this game!

At this moment, there are no more levels to play in this game. However, we are constantly working on new levels and will most likely expand this game with more levels soon.
Keep an eye out for an announcement on our usual communication channels!
In the meantime, you can play some of our other wargames.

If you have an idea for an awesome new level, please let us know!
```
