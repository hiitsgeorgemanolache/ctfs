Once authenticated, the page displays a message indicating that the password
for the next level can be found on the same page.

## Initial Analysis

- the visible page content does not directly display the password  
- this suggests that additional information may be present in the HTML source

## Investigation

To inspect the HTML source:

1. right-click anywhere on the page  
2. select **"View Page Source"**, or use the shortcut **Ctrl + U**

## Discovery

Within the HTML source, the following comment is present:

```html
<!--The password for natas1 is 0nzCigAq7t2iALyvU9xcHlYN4MlkIwlq -->
```
HTML comments are not rendered in the browser interface but remain visible in
the page source. In this case, the password for the next level is embedded
directly inside such a comment.
