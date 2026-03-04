## Interactive vs Non-Interactive SSH Sessions
In this level, access was restricted due to modifications in the shell initialization files. The `.bashrc` configuration was altered to automatically terminate the session upon login, preventing normal interactive shell access.  
Typically, SSH login provides an **interactive session**, allowing direct command execution via a CLI environment. However, SSH also supports **non-interactive sessions**, where a command is provided during the connection request. In this mode:  
- The specified command is executed immediately upon login.  
- The session terminates automatically after execution.  
- No interactive shell is spawned.
## Executing a Remote Command via SSH
Instead of logging in normally, a command was supplied directly during the SSH connection:  
```bash
ssh -p 2220 bandit18@bandit.labs.overthewire.org cat readme
```
Multiple commands can be executed by enclosing them in quotation marks:
```bash
ssh -p 2220 bandit18@bandit.labs.overthewire.org "command1 && command2"
```
Common operators include:  
• && — Executes the second command only if the first succeeds.  
• ; — Executes commands sequentially regardless of success.  
• | — Pipes the output of one command into another.
