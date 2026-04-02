This level focuses on exploiting a **file upload vulnerability** to achieve **Remote Code Execution (RCE)**. 
The goal is to bypass restrictions on uploaded files and execute PHP code on the server to retrieve the password.

## Source Code Analysis

Key observations from the server-side logic:

* Files are uploaded to an `/upload` directory.
* The server generates a **random filename** using a function.
* The **file extension is derived from user input (`filename`)**.
* There is a **file size restriction** (must be ≤ 1000 bytes).
* The file is stored using `move_uploaded_file()`.

### Critical Weakness

```text
The server trusts the filename provided by the user to determine the file extension.
```

This creates a vulnerability where an attacker can manipulate how the file is stored and later interpreted.

## 🧠 Exploitation

### Craft a Malicious Payload

Create a PHP file containing:

```php
<?php echo file_get_contents('/etc/natas_webpass/natas13'); ?>
```

This will read and output the password for the next level.

### Bypass File Type Restrictions

To pass upload validation:

* Make the file appear as an image (e.g., `.jpeg`)
* Optionally prepend a minimal JPEG header if needed

However, this alone is not enough...

### Intercept the Request

Use **Burp Suite** to:

- capture the **HTTP POST** request during file upload
- locate the `filename` parameter

### Modify the Extension

Change:

```text
randomname.jpeg → randomname.php
```
Even though the file passed validation as an image, modifying the request ensures the server saves the file as `.php`and executes it when accessed.

### Upload and Execute
- forward the modified request  
- the server responds with a link to the uploaded file  
- click the link  
  
trbs5pCjCrkuSknBBKHhaBxq6Wm1j3LC
