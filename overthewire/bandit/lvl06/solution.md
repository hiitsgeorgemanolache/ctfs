Once you run `ls`, you will see 20 directories with 6 files each (totalling 120 files). Instead of wasting time by opening each file until the password pops up, use the `find` command and its flags. You can check its `man` page here: [find(1) — Linux manual page](https://man7.org/linux/man-pages/man1/find.1.html)  

Upon checking manual in a thorough manner, you will see `-size n` and `-type c` under the *Tests* section. Use the information provided in the question and write the flags accordingly. For the first one, **n = 1033** and the suffix is **c** for bytes. You are looking for a human-readable non-executable file, so a regular file fits in that category

```bash
find -type f -size 1033c
cat "filename"
```
