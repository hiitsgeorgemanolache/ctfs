I checked the diff manual - “diff passwords.new passwords.old” will do the job and the line with “<“ refers to the first file mentioned in the command, which is “passwords.new” in our case and that’s where you will find the password
You could use diff -y —suppress-common lines - has the same effect but will show file1 difference on the left and file2 difference on the right
Diff -y without —suppress-common-lines will show you all the content in both files and draw a little straight line where the difference lies.
