# 🌐HTTP Traffic Investigation using Wireshark

## 🎯 Objective

The objective of this lab was to learn how to investigate HTTP traffic using Wireshark by capturing real web requests from a Windows machine and analyzing the complete communication between the client and the web server.

Unlike previous labs where the focus was on packets and protocols, this lab focused on understanding **how a SOC Analyst investigates web traffic**.

---

# 🖥️ Lab Environment

- **Host Machine:** Windows 11
- **Packet Analyzer:** Wireshark
- **Website Used:** http://neverssl.com
- **Protocol Investigated:** HTTP

---

# 📚 What I Learned

During this lab I learned how to identify:

- Client IP Address
- Server IP Address
- HTTP GET Request
- HTTP Response
- HTTP Status Codes
- Host Header
- User-Agent
- Referer
- Content-Type
- Web Server Information
- HTTP Timeline
- Investigation Methodology

---

# 🧪 Steps Performed

## Step 1 - Start Wireshark

- Open Wireshark.
- Select the active network interface.
- Start packet capture.

---

## Step 2 - Generate HTTP Traffic

Open the browser and visit:

```
http://neverssl.com
```

Browse a few pages such as:

```
/online/
/favicon.ico
```

This generates unencrypted HTTP traffic.

---

## Step 3 - Apply HTTP Filter

```
http
```

This displays only HTTP packets.

---

## Step 4 - Locate the HTTP Request

Find packets containing:

```
GET /online/ HTTP/1.1
```

---

## Step 5 - Identify the Client

Example:

```
192.168.1.6
```

The client is the machine that sends the HTTP request.

---

## Step 6 - Identify the Server

Example:

```
34.223.124.45
```

The server receives the request and returns the webpage.

---

## Step 7 - Expand HTTP Protocol

Expand the **Hypertext Transfer Protocol** section and inspect:

- Request Method
- Host
- User-Agent
- Referer
- Accept-Encoding
- Full Request URI

---

## Step 8 - Inspect the Response

Locate:

```
HTTP/1.1 200 OK
```

Expand the response and inspect:

- Server
- Content-Type
- Content-Length
- Date
- Status Code

---

# 🔍 Important Findings

## Client

```
192.168.1.6
```

---

## Server

```
34.223.124.45
```

---

## Request Method

```
GET
```

---

## Requested Resource

```
/online/
```

---

## Host

```
uniquesoothingclearmelody.neverssl.com
```

---

## User-Agent

```
Mozilla/5.0 (Windows NT 10.0)
```

---

## Referer

```
http://neverssl.com/
```

---

## HTTP Status Code

```
200 OK
```

Meaning:

The server successfully processed the request.

---

## Web Server

```
Apache/2.4.66
```

---

## Content Type

```
text/html
```

---

## Investigation Timeline

```
Windows Browser
        │
        ▼
HTTP GET Request
        │
        ▼
Web Server
        │
        ▼
HTTP 200 OK Response
        │
        ▼
Webpage Loaded Successfully
```

---

# 🧠 Investigation Methodology

This is the workflow I followed throughout the investigation.

```
1. Capture network traffic.

        ↓

2. Apply protocol filter.

        ↓

3. Identify the client.

        ↓

4. Identify the destination server.

        ↓

5. Locate the HTTP request.

        ↓

6. Examine request headers.

        ↓

7. Locate the HTTP response.

        ↓

8. Verify the HTTP status code.

        ↓

9. Identify the web server.

        ↓

10. Build the complete timeline.

        ↓

11. Write the investigation summary.
```

---

# 📖 SOC Investigation Questions

During every HTTP investigation, I should be able to answer:

- Who initiated the request?
- Which server received the request?
- Which webpage was requested?
- Which HTTP method was used?
- Which browser generated the request?
- Was the request successful?
- Which web server handled the request?
- What content was returned?
- Can I reconstruct the complete communication timeline?

---

# ✅ Investigation Summary

A Windows client generated an HTTP GET request to the NeverSSL web server. The request was successfully processed, and the server responded with **HTTP 200 OK**, returning an HTML webpage served by **Apache/2.4.66**.

Using Wireshark, I successfully reconstructed the complete HTTP conversation by identifying the client, server, request headers, response headers, and the overall communication timeline.

---

# 🎯 Skills Gained

- HTTP Traffic Analysis
- Packet Inspection
- Client-Server Communication
- HTTP Header Analysis
- Status Code Interpretation
- Timeline Reconstruction
- Network Investigation Methodology
- SOC Analyst Investigation Workflow

---

