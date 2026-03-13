The page presents a form requesting a secret value, along with a **Submit** button and a link labeled **View sourcecode**.

## Source Code Analysis

Inspecting the provided source code reveals the following PHP logic:

```
<?
include "includes/secret.inc";

if(array_key_exists("submit", $_POST)) {
    if($secret == $_POST['secret']) {
        print "Access granted. The password for natas7 is <censored>";
    } else {
        print "Wrong secret";
    }
}
?>
```

The script imports an external file using:

```
include "includes/secret.inc";
```

This indicates that the variable `$secret` is defined inside the referenced file.

## Investigating the Included File

Navigate to the `/includes` directory and open `secret.inc` by following this URL: `http://natas6.natas.labs.overthewire.org/includes/secret.inc`.
This reveals the following content:

```
<?
$secret = "FOEIUWGHFEEUHOFUOIU";
?>
```

The variable `$secret` is therefore assigned the value:

```
FOEIUWGHFEEUHOFUOIU
```

## Exploitation

The application compares the value submitted through the form (`$_POST['secret']`) with the `$secret` variable.
Providing the discovered value in the input field satisfies the condition:

```
$secret == $_POST['secret']
```

## Result

Submitting the correct secret value grants access and reveals the password for the next level:

```
bmg8SvU1LizuWjx3y7xkNERkHxGre0GS
```
