Not only does the filename start with a **-**, it also has multiple spaces. The shell treats spaces as separating arguments, so simply typing the filename out like that won’t generate the desired output. To fix this issue, we can use quotations.  
```bash
#open a file with ambiguous name - containing spaces
cat '--spaces in this filename--'
