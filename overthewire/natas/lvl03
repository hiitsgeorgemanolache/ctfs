## Initial Analysis

The page displays the message:

```
There is nothing on this page.
```

Inspection of the HTML source reveals the following element:

```html
<img src="files/pixel.png">
```

This indicates that an image named `pixel.png` is being loaded from a directory named `files`.

## Investigation

Since the image is located inside the `files` directory, the directory itself may also be directly accessible. Attempting to navigate to:

```
/files/
```

reveals that directory listing is enabled on the server.

## Discovery

The directory contains two files:

```
pixel.png
users.txt
```

Opening `users.txt` reveals a list of usernames and passwords. One of the entries contains the credentials for the next level.

## Solution

The password for **natas3** is `3gqisGdR0pjm6tpkDKdIWO2hSvchLeYH`.
