## Port Enumeration

To identify the correct service, `nmap` (Network Mapper) was used with the `-p` and `-sV` flags:
```bash
nmap -p31000-32000 -sV localhost
#-p31000-32000 scans the specified port range
#-sV performs service version detection
```
The scan revealed that five ports were listening within the specified range. Among them, two ports supported SSL/TLS encryption.  
Each SSL-enabled port was tested using:
```bash
openssl s_client -connect localhost:<port>
```
The initial connection resulted in repeated password prompts and continuous *KEYUPDATE* messages. The challenge instructions indicate that in such cases, the *CONNECTED COMMANDS* section of the `openssl s_client` manual should be reviewed.  
Inspection of the documentation revealed two relevant options:  
• `-quiet` — Suppresses session and certificate output.  
• `-ign_eof` — Prevents the connection from closing when EOF (End Of File) is reached.  
The `-quiet` option implicitly enables `-ign_eof`, making it suitable for interactive use.  
The corrected command:
```bash
openssl s_client -quiet -connect localhost:<port>
```
Of the two SSL-enabled ports identified (*31518* and *31790*):  
- Port 31518 echoed input without returning useful data  
- Port 31790 returned a private SSH key after receiving the correct password

## Private Key Extraction

The retrieved private key was saved locally in a temporary working directory:
```bash
mkdir /tmp/bandit16game
cd /tmp/bandit16game
nano sshkey16.private
```
To satisfy OpenSSH’s strict permission requirements (refuses to use private keys that are accessible by other users), the key permissions were restricted:
```bash
chmod 600 sshkey16.private
```
This sets:
- Read and write permissions for the owner
- No permissions for group or others

## SSH Authentication

Authentication was performed using the extracted private key:
```bash
ssh -i sshkey16.private bandit17@localhost
```
After successful login, the password for the next level was retrieved:
```bash
cat /etc/bandit_pass/bandit17
```
