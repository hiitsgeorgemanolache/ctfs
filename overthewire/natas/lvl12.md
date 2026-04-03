## Level Overview

The application stores user preferences in a cookie and claims that the data is protected using XOR encryption.
The objective is to manipulate this cookie to set a hidden parameter (`showpassword`) to `"yes"`, which reveals the password for the next level.

## Source Code Analysis

The application initializes default data as:

```php
$defaultdata = array(
    "showpassword" => "no",
    "bgcolor" => "#ffffff"
);
```

The cookie handling process is implemented as follows:

### Decoding (Client → Server)

```php
$tempdata = json_decode(
    xor_encrypt(
        base64_decode($_COOKIE["data"])
    ), 
true);
```

### Encoding (Server → Client)

```php
setcookie("data", base64_encode(
    xor_encrypt(
        json_encode($d)
    )
));
```

## Data Flow

The application applies a reversible transformation pipeline:

```text
JSON → XOR Encrypt → Base64 Encode → Cookie
```

And reverses it when reading:

```text
Cookie → Base64 Decode → XOR Decrypt → JSON Decode
```

## Vulnerability

The system uses XOR encryption with a repeating key.

XOR has a critical property:

```text
A ⊕ B ⊕ B = A
```

This means that if both the plaintext and ciphertext are known, the key can be recovered:

```text
ciphertext ⊕ plaintext = key
```

Because the default JSON structure is predictable, this becomes a **known-plaintext attack**.

## Exploitation

### Step 1 – Obtain Cookie

Retrieve the `data` cookie from the browser:

```text
HmYkBwozJw4WNyAAFyB1VUcqOE1JZjUIBis7ABdmbU1GIjEJAyIxTRg=
```

---

### Step 2 – Identify Known Plaintext

Default JSON:

```json
{"showpassword":"no","bgcolor":"#ffffff"}
```

---

### Step 3 – Recover Key

1. Base64 decode the cookie
2. XOR the result with the known JSON

This reveals a repeating key:

```text
eDWoe
```

---

### Step 4 – Modify Data

Change the JSON to:

```json
{"showpassword":"yes","bgcolor":"#ffffff"}
```

---

### Step 5 – Rebuild Cookie

Apply the same transformation process:

```text
JSON → XOR with key → Base64 encode
```

Resulting cookie:

```text
HmYkBwoSNDYcFhIrJQtHX2YuChZHaHUNAgYrOwAXR351TAMDIjEJA0c5
```

---

### Step 6 – Inject Cookie

Replace the existing `data` cookie in the browser or via an HTTP request.

## Result

When the modified cookie is processed, the application interprets:

```php
$data["showpassword"] == "yes"
```

This triggers the condition that reveals the password for the next level, which is `yZdkjAYZRd3R7tq7T5kXMjMJlOIkzDeB`.
