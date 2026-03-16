## Level Overview

The page provides a search form that returns words from `dictionary.txt` containing the user-supplied string.

## Source Code Analysis

Reviewing the source code shows that the application passes user input into a system command that performs a search against `dictionary.txt`.

Although the intended functionality is only to search for matching words, the implementation does not safely sanitize user input before embedding it into the 
command. This introduces a **command injection** vulnerability.

## Key Concept

This level demonstrates command injection. When user input is incorporated into shell commands without proper sanitization or escaping, arbitrary commands may 
be executed on the server, leading to unauthorized access to sensitive files.

## Exploitation

Because the input is inserted into a shell command, command separators can be used to terminate the intended search operation and append a new command.

In this case, the payload is structured so that the original command is closed and a second command is executed to print the password file for the next level:

```text
""; cat /etc/natas_webpass/natas10
```
The appended `cat` command reads the password directly from:
```text
/etc/natas_webpass/natas10
```
## Result

Submitting the payload causes the application to execute the injected command and display the contents of the password file. 
The first output line reveals the password for the next level: `t7I5VHvpa14sJTUGV0cbEsbYfFP2dmOu`.
