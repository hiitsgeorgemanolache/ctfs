I thought of using pipes, which are used to funnel the output of the previous command as input for the next. First, I checked out the file's contents and saw a lot of lines. Then I checked the `man` page of all of the commands recommended on the webpage, and stumbled upon `sort` and one of its flags, `-R`:  
`-R`, `--random-sort` - shuffle, but group identical keys (the `man` page: [sort - sort lines of text files](https://man7.org/linux/man-pages/man1/sort.1.html))  
I could already see the password just from this command, but I wanted to find chain of commands that only outputs the desired line. In order to do that, use the `uniq` command and its `-u` flag, which only prints unique lines (the `man` page: [uniq - report or omit repeated lines](https://man7.org/linux/man-pages/man1/uniq.1.html))  
  
```bash
cat data.txt | sort -R | uniq -u
```
