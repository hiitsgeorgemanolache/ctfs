Use the `tr` command which translates or deletes characters from standard input (`man` page: [tr(1) - Linux man page](https://linux.die.net/man/1/tr). Examining the synopsis shows that `tr` accepts two character sets: **tr [OPTION]... SET1 [SET2]**. Each character in *SET1* is translated to the character at the corresponding position in *SET2*. In other words, index *0* of *SET1* maps to index *0* of *SET2*, and so forth.  
Using this command before opening the file's contents will generate an error message, therefore you should first open it and then pipe the content towards the command we are referring to.
The command:  
```bash
tr "a-zA-Z" "n-za-mN-ZA-M"
```
implements ROT13 by shifting each letter 13 positions forward in the alphabet:
- a → n
- b → o
- ...
- n → a

The sequence wraps around at the end of the alphabet.
This is why the range is written as *n-za-m* instead of something like *n-m*, which would produce the error: *range-endpoints of 'n-m' are in reverse collating sequence order*
```bash
cat data.txt | tr “a-zA-Z” “n-za-mN-ZA-M”
```
