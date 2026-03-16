## Level Overview

The page presents a form requesting a secret value.

## Source Code Analysis

Inspecting the source code shows that the submitted value is not compared directly.
Instead, the application applies a sequence of transformations to the user input and checks whether the result matches a predefined encoded string.
The encoding process is performed in the following order:

1. `base64_encode`  
2. `strrev`  
3. `bin2hex`  

The target encoded value is defined near the beginning of the PHP source.

## Reversing the Encoding

To determine the correct input, the transformation chain must be reversed. This requires applying the inverse operations in the opposite order:  

1. convert the hexadecimal string back to binary
2. reverse the resulting string with `strrev`
3. decode the result with `base64_decode`

Applying these steps reveals the original secret value:

```text
oubWYf2kBq
```

Submitting the recovered string in the input field satisfies the application check and grants access to the next password,
which is `ZE1ck82lmdGIoErlhQgWND6j2Wzz6b6t`.
