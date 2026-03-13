## Level Overview

The page displays the message:
```
Access denied. You are not logged in.
```
This suggests that the application checks whether the user is authenticated before revealing the password.

## Analysis

Inspection of the browser's developer tools reveals a cookie:
```
loggedin=0
```
This indicates the application uses a client-side cookie to determine the authentication state.
Due to the cookies being stored in the browser and sent with each request, the user can modify them, 
causing an authentication check bypass (if the application relies solely on this value).

## Exploitation

The cookie value can be modified directly in the browser:  
1. Open Developer Tools
2. Navigate to **Application → Cookies**
3. Locate the cookie named `loggedin`
4. Change its value from `0` to `1`
5. Refresh the page

## Result
After refreshing the page with the modified cookie, the following message can be seen:
```
Access granted. The password for natas6 is 0RoJwHdSKWFTYR5WuiAewauSuNaBXned
```
