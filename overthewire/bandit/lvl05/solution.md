When running `ls`, you will see 10 files (*-file00* to *-file09*). Instead of opening all of them to see which is the correct one, we can write a command which shows each file's type and then only open the one in human readable format. To do that, use the `file` command to determine file type and `*` - a shell wildcard (also called a **glob**) which means: **match zero or more characters in this part of the filename.**  
The manual page for `file`: [file(1) — Linux manual page](https://man7.org/linux/man-pages/man1/file.1.html)
```bash
#output the file type for all files starting with “-file”
file ./-file*
```  
Only *-file07* has ASCII text, the rest, just “data”, so:
```bash
cat ./-file07
```
