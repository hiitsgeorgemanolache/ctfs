## Level Overview

This level builds upon the previous file upload challenge by introducing an additional restriction: the server validates that uploaded files are legitimate images.
The objective is to bypass this validation and execute arbitrary PHP code on the server in order to retrieve the password for the next level.

## Source Code Analysis

The application generates a random filename for uploads while preserving the original file extension:

```php
function makeRandomPathFromFilename($dir, $fn) {
    $ext = pathinfo($fn, PATHINFO_EXTENSION);
    return makeRandomPath($dir, $ext);
}
```

The file is then validated before being stored:

```php
if(filesize($_FILES['uploadedfile']['tmp_name']) > 1000) {
    echo "File is too big";
} else {
    if(move_uploaded_file($_FILES['uploadedfile']['tmp_name'], $target_path)) {
        echo "The file <a href=\"$target_path\">$target_path</a> has been uploaded";
    }
}
```

Additionally, the server checks whether the file is a valid image (e.g., using magic bytes).

## Vulnerability

Although the server attempts to verify that the uploaded file is an image, it relies on superficial checks (such as file headers) rather than enforcing strict content validation.
This allows attackers to craft a file that appears to be a valid image, but contains executable PHP code.

## Exploitation

### Step 1 – Create Malicious File

Construct a file that begins with valid JPEG magic bytes and contains embedded PHP code.

Example:

```php
ÿØÿà<?php echo file_get_contents('/etc/natas_webpass/natas14'); ?>
```

This ensures the file passes image validation while remaining executable.

---

### Step 2 – Bypass Extension Restriction

The server uses the original filename extension.

Even if the file is named `.jpeg`, execution depends on how the server interprets it.

Using an intercepting proxy such as **Burp Suite**, modify the request:

* Change the filename from `.jpeg` to `.php` before it reaches the server

---

### Step 3 – Upload File

Submit the modified request.

The server responds:

```text
The file upload/<random>.php has been uploaded
```

---

### Step 4 – Execute Payload

Navigate to the uploaded file:

```text
/upload/<random>.php
```

The PHP code executes and outputs the password.

---

## Result

The contents of:

```text
/etc/natas_webpass/natas14
```

are displayed, revealing the password for the next level, which is `z3UYcr4v4uBpeX8f7EZbMHlzK4UR2XtQ`.
