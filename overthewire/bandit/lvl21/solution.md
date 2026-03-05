Run the following commands:
```bash
ls
#a file called "suconnect" can be seen, which is a setuid file (the red highlighting helps determine this)
./suconnect
# the following output appears:
# Usage: ./suconnect <portnumber>
# This program will connect to the given port on localhost using TCP.
# If it receives the correct password from the other side,
# the next password is transmitted back.
```
The output reveals that the program establishes a TCP connection to localhost on a specified port.

To interact with this connection, the `nc` (**netcat**) utility can be used. Reviewing the manual shows that the `-l` option which is used to specify `nc` should listen for an incoming connection rather than initiate a connection to a remote host. The destination and port to listen on can be specified either as non-optional arguments, or with options `-s` and `-p` respectively. In this case, write *localhost* and the port number can be the port can be any available value between 1 and 65535. The following command should be run in another terminal, **logged in as *bandit20***:
```bash
#listen on a selected port (random number) with a selected destination
nc -l -s localhost -p 3762
```
 `./suconnect 3762` should be run simultaneously, which connects to the specified port and waits for the *Bandit20* password to be transmitted through the `nc` listener.
