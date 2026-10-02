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
