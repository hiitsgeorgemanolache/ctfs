To identify the changed password, the `diff` command was used to compare the two files, whose `man` page can be checked here: [diff(1) — Linux manual page](https://man7.org/linux/man-pages/man1/diff.1.html)
```bash
diff passwords.new passwords.old
```
The output highlights differences between the files.  
Lines prefixed with:  
• `<` - refer to the first file specified in the command (*passwords.new*)  
• `>` - refer to the second file (*passwords.old*)  
The password is located in the line marked with `<`, indicating content present in *passwords.new* but not in *passwords.old*.

## Side-by-Side Comparison

An alternative approach uses the side-by-side comparison mode:
```bash
diff -y --suppress-common-lines passwords.new passwords.old
```
Options used:  
• -y — Displays output in two columns (side-by-side format)  
•--suppress-common-lines — Hides identical lines, showing only differences  
This format presents *File 1* (*passwords.new*) on the left and *File 2* (*passwords.old*) on the right. Only differing lines are displayed, making the modified password immediately visible.

## Full Side-by-Side Output

Running:
```bash
diff -y passwords.new passwords.old
```
without `--suppress-common-lines displays` displays all lines from both files, with a vertical separator indicating where differences occur. This approach provides full context while still visually marking modifications.
You could use diff -y —suppress-common lines - has the same effect but will show file1 difference on the left and file2 difference on the right
Diff -y without —suppress-common-lines will show you all the content in both files and draw a little straight line where the difference lies.
