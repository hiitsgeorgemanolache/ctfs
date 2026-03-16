## Level Overview

The page provides a search form similar to the previous level. It searches for words inside `dictionary.txt` that contain the user-supplied input.

## Source Code Analysis

Inspecting the source code reveals the following logic:

```php
<?
$key = "";

if(array_key_exists("needle", $_REQUEST)) {
    $key = $_REQUEST["needle"];
}

if($key != "") {
    passthru("grep -i $key dictionary.txt");
}
?>
```
The application retrieves the needle parameter from the request and stores it in `$key`. If `$key` is not an empty string, the value is inserted directly into 
a shell command executed via `passthru()`.
Because the user input is embedded in the command without proper sanitization or escaping, the application remains vulnerable to command manipulation.

## Input Restrictions

Unlike the previous level, several special characters commonly used for command injection (such as `;` or `|`) are filtered. 
However, the input is still not safely handled when passed to the shell.

## Exploitation

The conditional statement only checks whether `$key` is an empty string:
```php
if($key != "")
```
Submitting the value:
```php
""
```
does not produce an empty string. Instead, it sends two literal quote characters, meaning the condition still evaluates to true and the command is executed.
This results in the following shell command:

```php
grep -i "" dictionary.txt
```
In shell syntax, **""** represents an empty string. An empty search pattern is valid for `grep`, causing every line in the file to match.

Since the input is inserted directly into the command arguments, additional file paths can be supplied. By referencing the password file for the next level, 
the command becomes:

```php
grep -i "" /etc/natas_webpass/natas11 dictionary.txt
```
Because the search pattern is empty, all lines from the specified files are returned. This includes the contents of the password file, 
`UJdqkK1pTu6VLt9UHWAgRZz6sVUZ3lEk` in this case.

## Key Concept
This level demonstrates that filtering a limited set of special characters is insufficient to prevent command injection. 
Even without command separators, attackers may still manipulate command arguments when user input is inserted into shell commands without proper validation
or escaping.
