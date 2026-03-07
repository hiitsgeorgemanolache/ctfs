The `more` command (documentation: [more(1) — Linux manual page](https://man7.org/linux/man-pages/man1/more.1.html)) is used to display the contents of a file in a terminal. It is particularly useful when working with files containing large amounts of data, as it allows the content to be viewed one screen at a time.  
While `more` is running, several interactive commands are available. One of these is `v`, which opens the currently viewed file in the system’s default editor (commonly `vi`). This command is **not provided as a command-line flag**, but is instead pressed while `more` is active.  
The challenge specifies that the shell used by *bandit26* is **not `bash`**, so the first step is to inspect the user's shell configuration in `/etc/passwd`.
```bash
cat /etc/passwd | grep bandit26
```bash
cat /etc/passwd | grep “bandit26” #opens all the content for passwords and selects the line that talks about bandit26
#output - /usr/bin/showtext

cat /usr/bin/showtext
#output - exec more ~/text.txt
```
This reveals that the program simply executes `more` on the file *~/text.txt*.
```bash
ls
#output - bandit26.sshkey

ssh -i bandit26.sshkey -p 2220 bandit26@localhost #this command should be executed whilst the terminal window is minimised by dragging its corners
```
To ensure that `more` activates instead of immediately exiting, the terminal window should be reduced in size before connecting. This causes more to paginate the output rather than display the entire file at once. When it appears on the screen, pressing `v` opens the file in the `vi` editor.  
Apart from the commonly used *Insert Mode* and *Normal Mode*, `vi` also supports **command-line operations**. One useful command is `:read` (or `:r`), which inserts the contents of a file or the output of a shell command into the current buffer.
```bash
:read /etc/bandit_pass/bandit26
```
