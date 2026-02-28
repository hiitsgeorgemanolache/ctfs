**Trick** - if a file’s name begins with a hyphen (`-`), commands like `cat` may misinterpret it as an option.  
To avoid this **option parsing collision**, reference the file explicitly with:

```bash
#open file with a ambiguous name  
`cat ./-`
