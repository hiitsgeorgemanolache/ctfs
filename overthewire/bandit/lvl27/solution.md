The same SSH command used in the previous level should be executed under identical conditions in order to activate the pagination behaviour of the `more` command and its interactive command, `v`. 
Inside `vim`, the shell used by the editor can be modified with `:set shell=/bin/bash`. This changes the shell used internally by vim to bash. After setting the shell, use `:shell` to start an interactive shell session. This effectively provides a terminal shell running as the *bandit26* user.
Some users may encounter difficulties with this level because certain terminal environments do not reliably trigger the more pagination behaviour, even when the terminal window is resized. Adjusting the terminal size further or reconnecting typically resolves the issue.
In theory, reducing the terminal window size should force the `more` command to paginate its output, which allows access to its interactive commands (such as `v`). However, this behaviour depends on how the terminal dimensions are communicated to the remote shell.  
Terminal applications report their dimensions (rows and columns) to the remote system through the TTY interface. In certain configurations, resizing the local terminal window does not automatically update the remote shell’s understanding of the terminal size. As a result, `more` may still assume that the terminal is large enough to display the entire file and therefore exits immediately without enabling pagination.  
This can occur depending on the terminal emulator, SSH client behaviour, or specific remote shell settings. In such cases, repeatedly resizing the local window may not trigger the expected behaviour.  
A practical workaround is to ensure that the remote shell receives updated terminal dimensions or to attempt the connection again after resizing the window beforehand. Once `more` detects that the content exceeds the terminal height, it enters paginated mode, allowing interactive commands such as `v` to be used.
```bash
ls
#a setuid executable named "bandit27-do" can be observed

./bandit27-do
#output:
#Run a command as another user.

./bandit27-do cat /etc/bandit/bandit_pass/bandit27
```
