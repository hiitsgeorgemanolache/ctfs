The `mktemp -d` command creates a temporary directory  
You look through the “cp” manual and see the synopsis - source destination, so
```bash
#copy the contents of the original file, then change directory to the one that was created using the command written at the top (tdm - temporary directory name)
cp data.txt *tdm*
cd *tdm*
```
Because the file was compressed multiple times, we have to reverse the process by repeatedly decompressing. For that, use the `xxd` command and its `-r` flag (check the `man` page for a helpful example on `xxd -r`: [xxd(1) - Linux man page](https://linux.die.net/man/1/xxd))
```bash
#open the contents of a file, reverse hexdump and redirect the output in a new file called hexdump
cat data.txt | xxd -r > hexdump
```
There are about 7 more decompressions and the formats are sensitive so you have to change the name to match the format of the data type, which can be done using the `mv` command (check the `man` page for the synopsis: [mv(1) - Linux man page](https://linux.die.net/man/1/mv)). Furthermore, these decompressions will be done using `gzip` and `bzip2`, so check both of their manual pages and you will come across the `-d` flag, which decompresses.
```bash
#for gzip -
mv data.txt data.gz
gzip -d #to decompress once renamed
#for bzip2 -
mv data.txt data.bzip2
bzip2 -d #to decompress once renamed
```
After some decompressions, you will get to a POSIX tar archive. When you look through the `tar` manual (here it is: [tar(1) - Linux man page](https://linux.die.net/man/1/tar), you will see the `-x` flag to extract files from an archive, and the synopsis also mentions adding an `-f` flag for archives.
```bash
#extract files from an archive, use archive file or device ARCHIVE
tar -xf *tararchivename*
#one letter flags can be concatanated in one hyphen, but bare in mind that the order in which you write them is essential if any of the flags requires an argument
#repeat previous processes
cat *asciitextfile*
```
