## Level Overview

The page displays the following message:
```
Access disallowed. You are visiting from "" while authorized users should come only from "http://natas5.natas.labs.overthewire.org/".
```
This suggests that the application verifies the origin of the request before granting access.

## Analysis

The message implies that access is granted only if the request originates from a specific page.  
Web servers commonly determine the origin of a request using the `Referer` HTTP header.

In this case, the server likely checks whether the request contains:
```
Referer: http://natas5.natas.labs.overthewire.org/
```
Since browsers normally omit this header when the page is accessed directly, the server denies access.

Because HTTP headers can be controlled by the client, the request can be modified to include the expected `Referer` value.

## Exploitation

A custom request can be sent using `curl`, including:

- `-i` to display response headers
- `-u` to provide the current level credentials
- `-H` to add a custom HTTP header

Command used:

```bash
curl -i -u natas4:<password> -H "Referer: http://natas5.natas.labs.overthewire.org/" http://natas4.natas.labs.overthewire.org/
```
## Password
`0n35PkggAPm2zbEpOU802c0x0Msn1ToK`
