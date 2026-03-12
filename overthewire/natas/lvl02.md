## Initial Analysis

Inspection of the page reveals a message indicating that the password
for the next level can be found on the page. However, the context menu
has been disabled, preventing the use of the **right-click → View Page Source**
option.
## Investigation

Despite the disabled context menu, the page source can still be accessed
using the keyboard shortcut `Ctrl + U`.
This shortcut opens the browser's **View Page Source** window directly,
bypassing the client-side restriction.

## Discovery

Inspection of the HTML source reveals the following comment:

```html
<!--The password for natas2 is TguMNxKo1DSa1tujBLuZJnDUlCcUAPlI -->
```
