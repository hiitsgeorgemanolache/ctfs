The page contains a two elements - `Home` and `About`.  
Reviewing the application source code reveals that the selected page is loaded directly from user-controlled input.
This indicates the presence of a **Local File Inclusion (LFI)** vulnerability.  
Because the application does not properly restrict traversal sequences, it becomes possible to move outside the intended 
web directory structure and request files from elsewhere on the system.

## Exploitation

The general challenge description states that level passwords are stored in:

```text
/etc/natas_webpass/
```
To reach that file from the vulnerable parameter, directory traversal sequences can be used:
```text
../../../../etc/natas_webpass/natas8 (the two dots have the same meaning as in Linux, directing to the parent directory
```
This payload is appended to the page parameter in the URL, causing the application to include the password file instead of an expected local page.

## Result

Once the traversal payload is submitted, the contents of the target file are displayed, revealing the password for the next level, 
which is `xcoXLmzMkoIP9D7hlgPlh9XD7OgLAe5Q`.
