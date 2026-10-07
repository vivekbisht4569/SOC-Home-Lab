# Part 3 = Convert From Json 

What does ConvertFrom-Json mean?

Think:

```bash

JSON TEXT
   ↓
ConvertFrom-Json
   ↓
PowerShell OBJECT
   ↓
Access individual fields



```

For example:

```bash

JSON:

{"src_ip":"192.168.56.103","dest_ip":"192.168.56.1","proto":"UDP"}

```

becomes conceptually:

```bash
src_ip   → 192.168.56.103
dest_ip  → 192.168.56.1
proto    → UDP

```
Let's test it

Run:
```bash
Get-Content C:\Suricata\log\eve.json -Tail 1 | ForEach-Object { $_ | ConvertFrom-Json }

```

 ForEach-Object

This is our first new concept.

Suricata's eve.json contains many JSON lines.

Conceptually:

```bash
LINE 1 → JSON event
LINE 2 → JSON event
LINE 3 → JSON event
LINE 4 → JSON event

```

ForEach-Object means:
Take each object that comes through the pipeline and do something with it.

Basic structure:

```bash

ForEach-Object { something }

```

The { } contains the action we want to perform.



 What is $_?

This is extremely important.

Inside:

```bash

ForEach-Object { $_ }

```
$_ means:
the current item

Imagine:

```bash

Line 1
Line 2
Line 3

```

Powershell Processes them:

```bash

$_ = Line 1
$_ = Line 2
$_ = Line 3

```

So:

```bash

ForEach-Object { $_ }

```
basically means:

For each item, give me the current item.

Now combine the pieces
Look at:

```bash
Get-Content C:\Suricata\log\eve.json -Tail 1 |
ForEach-Object {
    $_ | ConvertFrom-Json
                              }

```

Read it from left to right

```bash

   Get-Content
      ↓
   Read eve.json
      ↓
   -Tail 1
      ↓
   Take the last line
      ↓
   |
      ↓
   ForEach-Object
      ↓
   Take the current line ($_)
      ↓
   ConvertFrom-Json
      ↓
   Turn JSON text into a PowerShell object



```

