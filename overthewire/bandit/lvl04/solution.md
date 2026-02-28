Use the `cd` command to change the working directory, whose manual page you can find here: [cd(1p) — Linux manual page](https://man7.org/linux/man-pages/man1/cd.1p.html)  
Use the `ls` command to list directory contents, whose manual page you can find here: [ls(1) — Linux manual page](https://man7.org/linux/man-pages/man1/ls.1.html)  
The file is “hidden” because of the **.** at the beginning (Unix convention to keep the workspace cleaner), but `ls` has `-a` flag which outputs all files in the directory, as it does not ignore entries starting with **.**

```bash
#change working directory to *inhere*  
cd inhere
#show all filenames
ls -a
cat *filename*  
```
