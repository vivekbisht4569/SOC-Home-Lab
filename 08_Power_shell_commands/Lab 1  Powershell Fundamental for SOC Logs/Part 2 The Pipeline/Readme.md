# Part 2 = The Pipeline | 

This is one of the most important PowerShell concepts.

Think of: 
```bash
| 
```
as:

``` bash
"Take the output from the left and send it to the command on the right."
```

Example:
``` bash

Get-Content C:\Suricata\log\eve.json | Select-String "alert"

```

Think:

```bash

Get-Content
     ↓
Read eve.json
     ↓
|
     ↓
Select-String
     ↓
Find lines containing "alert"


```

So:

```bash

COMMAND A | COMMAND B

```

Means:

```bash

    COMMAND A
      ↓
    output
       ↓
    COMMAND B

```

