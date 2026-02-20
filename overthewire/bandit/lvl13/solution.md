The “mktemp -d” command makes a temporary directory
You look through the “cp” manual and see the synopsis - source destination, so “cp data.txt *name of the temp directory made by previous command” then “cd” to that directory
The file is ASCII and “xxd” makes a hexdump or does the reverse, but because it was repeatedly compressed, you are going to have to decompress, so reverse, which is “-r”. In an example from the “xxd” manual you see that if you want to xxd -r and save the result in a new file - you put “xxd -r > output_file”, therefore:
“cat data.txt | xxd -r > hexdump”
you are going to do about 7 more decompressions and the formats are sensitive so you have to change the name to match the format of the data type. The mv synopsis is “source destination” so:
For gzip - mv “data.txt data.gz” and “gzip -d” to decompress once renamed
For bzip2 - mv “data.txt data.bzip2” and “bzip2 -d” to decompress once renamed
After some decompressions, you will get to a POSIX tar archive. When you look through the “tar” manual, you will see “tar -x” to extract files from an archive. The synopsis also mentions adding an “-f” for archives, so “tar -x -f *tararchivename*” = “tar -xf *tararchivename*” will take you to the next step
When you have one letter commands, you can add them all in one “-“, but the order is essential when one of the options requires an argument
Keep going until you get an ASCII text, which you “cat”.
