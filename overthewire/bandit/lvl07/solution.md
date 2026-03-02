`find` also has the `-user uname` and `-group gname` flags.  

```bash
find -user bandit7 -group bandit6 -size 33c
```

Running this command produces many "Permission denied" messages.  
These can be redirected to `/dev/null` using `2>/dev/null`, a useful technique when running commands that traverse the filesystem, where permission errors are expected and not relevant to the task.

In Bash file descriptor notation:

- `0` → standard input (**stdin**)  
- `1` → standard output (**stdout**)  
- `2` → standard error (**stderr**)  

`/dev/null` is a special device file that discards all data written to it, so unwanted error messages are suppressed.  
This allows the desired result, `./var/lib/dpkg/info/bandit7.password`, to appear clearly in the output.  

```bash
find -user bandit7 -group bandit6 -size 33c 2>/dev/null
cat ./var/lib/dpkg/info/bandit7.password
```
