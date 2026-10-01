# Part 1 =   Get-Content 

What does Get-Content do?

It reads the contents of a file.


``` bash

Get-Content C:\Suricata\log\eve.json

```

You'll see JSON events similar to:

``` bash

{"timestamp":"...","event_type":"flow",...}
{"timestamp":"...","event_type":"alert",...}
{"timestamp":"...","event_type":"stats",...}

```

Mental Model

``` bash

Get-Content
     ↓
Read file
     ↓
Give me its contents

```

