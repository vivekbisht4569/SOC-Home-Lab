#  Suricata Detection Rules — Understanding the Basics



The important mindset is:

    Detection Idea
         ↓
    Define What Traffic We Want
         ↓
    Build Rule
         ↓
    Suricata Checks Traffic
         ↓
    Rule Matches
         ↓
    Alert Generated

The goal is not to memorize rules.

The goal is to understand how to **build a rule from a detection requirement**.

---

# 🧩 Basic Suricata Rule Structure

A basic Suricata rule contains:

    RULE HEADER
          +
    RULE OPTIONS

Example:

    alert icmp any any -> any any (msg:"ICMP_TEST"; sid:1000001; rev:1;)

The rule can be visually divided into:

    alert icmp any any -> any any
    ─────────────────────────────
             RULE HEADER

    msg:"ICMP_TEST";
    sid:1000001;
    rev:1;
    ─────────────────────────────
             RULE OPTIONS

---

# 1. Rule Header

The basic header structure is:

    ACTION PROTOCOL SOURCE_IP SOURCE_PORT DIRECTION DEST_IP DEST_PORT

Example:

    alert icmp any any -> any any

Think of it as answering:

    WHAT ACTION?
         ↓
    WHICH PROTOCOL?
         ↓
    FROM WHOM?
         ↓
    FROM WHICH PORT?
         ↓
    GOING WHERE?
         ↓
    TO WHICH PORT?

---

# 2. `alert`

Example:

    alert

Meaning:

    "Generate an alert if the traffic matches this rule."

Concept:

    Traffic
       ↓
    Rule Match
       ↓
    ALERT

For our SOC IDS practice, `alert` is the main action we use.

---

# 3. Protocol

Example:

    icmp

This tells Suricata which protocol the rule should inspect.

Therefore:

    alert icmp

means:

    Generate an alert for matching ICMP traffic.

Common protocols we may use later:

    tcp
    udp
    icmp

The important question is:

    "What protocol am I trying to detect?"

---

# 4. Source IP

Example:

    any

This represents the source IP.

`any` means:

    Any source IP

For example:

    192.168.56.103

would represent a specific source.

Therefore:

    any
       ↓
    Any source

while:

    192.168.56.103
       ↓
    Specific source

---

# 5. Source Port

Example:

    any

This represents the source port.

So:

    alert icmp any any

can be understood as:

    Protocol      = ICMP
    Source IP     = Any
    Source Port   = Any

For ICMP, ports are not used in the same way as TCP/UDP.

However, the source-port position becomes very important when
creating TCP and UDP rules.

---

# 6. Direction — `->`

The symbol:

    ->

defines traffic direction.

The basic idea is:

    SOURCE -> DESTINATION

For our lab:

    192.168.56.103 -> 192.168.56.1

means:

    Kali → Windows

Direction is important because a detection may depend on:

    Who is communicating
          +
    Where the traffic is going

---

# 7. Destination IP

Example:

    any

This represents the destination IP.

`any` means:

    Any destination IP

We could also specify:

    192.168.56.1

which would restrict the destination to the Windows machine.

---

# 8. Destination Port

Example:

    any

This represents the destination port.

So:

    alert icmp any any -> any any

can be read as:

    Alert
      ↓
    ICMP
      ↓
    Any source IP
      ↓
    Any source port
      ↓
    Traffic going toward
      ↓
    Any destination IP
      ↓
    Any destination port

---

# 🧠 Complete Rule Header

The following:

    alert icmp any any -> any any

means:

    "Generate an alert when ICMP traffic is observed
     from any source to any destination."

---

# 🧩 Rule Options

After the rule header comes:

    (
        rule options
    )

Example:

    (msg:"ICMP_TEST"; sid:1000001; rev:1;)

The options provide additional information about the detection.

The three basic options we learned are:

    msg
    sid
    rev

---

# 9. `msg`

Example:

    msg:"ICMP_TEST";

`msg` defines the message/name associated with the detection.

Think:

    "What should the analyst see when this rule fires?"

Example:

    msg:"ICMP_TRAFFIC_DETECTED";

The message should describe what the rule is intended to detect.

---

# 10. `sid`

Example:

    sid:1000001;

SID identifies the rule.

Think:

    SID = Rule ID

Example:

    1000001 → Rule 1
    1000002 → Rule 2
    1000003 → Rule 3

The SID allows a SOC analyst or detection engineer to identify
which specific rule generated an alert.

---

# 11. `rev`

Example:

    rev:1;

`rev` represents the revision/version of the rule.

Example:

    rev:1
       ↓
    First version

If the rule is modified:

    rev:2
       ↓
    Second version

Then:

    rev:3
       ↓
    Third version

This helps track changes made to detection rules.

---

# 🧠 How To Create a Detection Rule

Do NOT start by memorizing syntax.

Start with a detection requirement.

For example:

    "Detect ICMP traffic."

Now answer these questions:

    1. What action?
    2. What protocol?
    3. Who is the source?
    4. What is the source port?
    5. What is the direction?
    6. Who is the destination?
    7. What is the destination port?
    8. What message should the alert have?
    9. What SID identifies the rule?
    10. What revision is this?

---

# 🔨 Building the Rule Step-by-Step

Detection requirement:

    "Generate an alert when ICMP traffic is observed."

### Step 1 — Action

    alert

### Step 2 — Protocol

    icmp

### Step 3 — Source IP

    any

### Step 4 — Source Port

    any

### Step 5 — Direction

    ->

### Step 6 — Destination IP

    any

### Step 7 — Destination Port

    any

Now we have:

    alert icmp any any -> any any

---

### Step 8 — Add Message

    msg:"ICMP_TEST";

### Step 9 — Add SID

    sid:1000001;

### Step 10 — Add Revision

    rev:1;

Final rule:

    alert icmp any any -> any any (msg:"ICMP_TEST"; sid:1000001; rev:1;)

---

# 🧠 The Most Important Learning

We should not think:

    "I have to memorize this rule."

Instead think:

    Detection Requirement
           ↓
    What do I want to detect?
           ↓
    Protocol
           ↓
    Source
           ↓
    Direction
           ↓
    Destination
           ↓
    Rule Identification
           ↓
    Suricata Rule

---

# 🔍 Example of Reading a Rule

If we see:

    alert icmp any any -> any any (msg:"ICMP_TEST"; sid:1000001; rev:1;)

We should be able to read it naturally:

    alert
    → Generate an alert

    icmp
    → Look for ICMP traffic

    any any
    → Any source IP and source port

    ->
    → Traffic moving from source to destination

    any any
    → Any destination IP and destination port

    msg:"ICMP_TEST"
    → Name/message of the detection

    sid:1000001
    → Unique rule ID

    rev:1
    → Rule revision/version 1

---

# 🎯 Detection Rule Mindset

Every time we create a rule, ask:

    WHAT?
      ↓
    What behavior/traffic am I trying to detect?

    PROTOCOL?
      ↓
    TCP / UDP / ICMP / etc.

    SOURCE?
      ↓
    Who is sending the traffic?

    SOURCE PORT?
      ↓
    Which source port?

    DIRECTION?
      ↓
    Where is the traffic going?

    DESTINATION?
      ↓
    Who is receiving it?

    DESTINATION PORT?
      ↓
    Which service/port?

    MESSAGE?
      ↓
    What should the analyst see?

    SID?
      ↓
    Which rule generated the alert?

    REVISION?
      ↓
    Which version of the rule?

---

# 🔄 Detection Workflow

The complete concept is:

    Network Traffic
          ↓
    Suricata receives traffic
          ↓
    Suricata checks detection rules
          ↓
    Does traffic match the rule?
          ↓
       ┌───────┐
       │       │
      YES      NO
       │       │
       ↓       ↓
     ALERT    No Alert
       │
       ↓
    fast.log
       +
    eve.json
       ↓
    SOC Analyst
       ↓
    Investigation

---

# 🚨 Important SOC Concept

A detection rule does NOT necessarily mean:

    "This is definitely an attack."

A rule means:

    "This activity matched a condition
     that we decided was worth detecting."

Therefore:

    Rule Match
        ↓
      ALERT
        ↓
    INVESTIGATE
        ↓
      CONTEXT
        ↓
    Classification

Possible outcomes include:

    Benign Activity
    False Positive
    Suspicious Activity
    True Positive

---

# 🧠 Detection Engineering Mindset

Good detection creation starts with:

    What behavior do I want to detect?

Not:

    "What complicated rule can I write?"

The process should be:

    Security Behavior
          ↓
    Detection Requirement
          ↓
    Rule Logic
          ↓
    Suricata Syntax
          ↓
    Test
          ↓
    Alert
          ↓
    Investigation
          ↓
    Improve Rule

---

# 📌 Current Knowledge Level

At this stage, we understand the basic building blocks of a
Suricata detection rule:

    alert
    protocol
    source IP
    source port
    direction
    destination IP
    destination port
    msg
    sid
    rev

We also understand that the rule has:

    Header
      +
    Options

And most importantly:

    We create the rule from the detection requirement,
    not by blindly memorizing syntax.

---

# 🔑 Quick Revision

    alert
    → What should happen?

    protocol
    → What type of traffic?

    source
    → Who sent it?

    source port
    → Which source port?

    ->
    → In which direction?

    destination
    → Who receives it?

    destination port
    → Which destination service/port?

    msg
    → What should the detection be called?

    sid
    → Which rule is it?

    rev
    → Which version is it?

---

# 🏁 One-Line Memory

    DETECTION IDEA
         ↓
    WHAT TRAFFIC?
         ↓
    SOURCE → DESTINATION
         ↓
    RULE OPTIONS
         ↓
    SURICATA ALERT

The main skill is not memorizing:

    alert icmp any any -> any any (...)

The main skill is being able to look at a detection requirement
and build the correct rule logically.