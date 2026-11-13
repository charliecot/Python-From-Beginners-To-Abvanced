# HTTP Responses: Headers, Cookies, Content, and Python Requests

A practical guide to reading HTTP responses and understanding the
metadata sent between a server and a client.

## 1. The HTTP response structure

A typical HTTP/1.1 response looks like this:

``` http
HTTP/1.1 200 OK
Content-Type: application/json
Content-Length: 36
Cache-Control: no-store
Set-Cookie: sessionid=abc123; Path=/; HttpOnly; Secure; SameSite=Lax

{"message":"Products retrieved"}
```

A response contains:

1.  **Status line** --- protocol version, status code, reason phrase.
2.  **Headers** --- metadata describing the response and how to handle
    it.
3.  **Body** --- optional content such as HTML, JSON, text, an image, or
    a file.

The blank line separates headers from the body. HTTP/2 and HTTP/3 use
different wire formats, even though tools often display their
information in a familiar readable form.

**Important distinction:** `User-Agent` is normally a **request
header**, not a response header. It identifies the client software to
the server. `Server`, when present, is a response header and may
identify server software.

## 2. Status codes

  -----------------------------------------------------------------------
  Range                   Category                Meaning
  ----------------------- ----------------------- -----------------------
  1xx                     Informational           Preliminary
                                                  information; request
                                                  may continue

  2xx                     Success                 Request successfully
                                                  handled

  3xx                     Redirection             Further
                                                  navigation/action is
                                                  indicated

  4xx                     Client error            Request cannot be
                                                  fulfilled as sent

  5xx                     Server error            Server failed to fulfil
                                                  an apparently valid
                                                  request
  -----------------------------------------------------------------------

Common codes:

  -----------------------------------------------------------------------
  Code                                Meaning
  ----------------------------------- -----------------------------------
  `100 Continue`                      Client may continue sending the
                                      request

  `101 Switching Protocols`           Protocol switch accepted

  `200 OK`                            Request succeeded

  `201 Created`                       Resource created

  `202 Accepted`                      Accepted for processing, not
                                      necessarily completed

  `204 No Content`                    Success; no response body

  `206 Partial Content`               Partial representation returned

  `301 Moved Permanently`             Permanent redirect

  `302 Found`                         Temporary redirect

  `303 See Other`                     Retrieve another URL, usually with
                                      GET

  `304 Not Modified`                  Reuse a cached representation if
                                      available

  `307 Temporary Redirect`            Redirect while preserving method

  `308 Permanent Redirect`            Permanent redirect while preserving
                                      method

  `400 Bad Request`                   Invalid or malformed request

  `401 Unauthorized`                  Authentication missing or invalid

  `403 Forbidden`                     Server refuses authorization

  `404 Not Found`                     Resource not found

  `405 Method Not Allowed`            Method unsupported for this
                                      resource

  `408 Request Timeout`               Server timed out waiting for
                                      request

  `409 Conflict`                      Request conflicts with resource
                                      state

  `410 Gone`                          Resource is no longer available

  `413 Content Too Large`             Request body too large

  `415 Unsupported Media Type`        Request body format unsupported

  `422 Unprocessable Content`         Content understood but cannot be
                                      processed

  `429 Too Many Requests`             Rate limit exceeded

  `500 Internal Server Error`         Unexpected server failure

  `501 Not Implemented`               Functionality unsupported

  `502 Bad Gateway`                   Gateway received invalid upstream
                                      response

  `503 Service Unavailable`           Server temporarily unavailable

  `504 Gateway Timeout`               Gateway did not receive upstream
                                      response in time
  -----------------------------------------------------------------------

A `404` or `500` is still an HTTP response. It does not necessarily mean
the network connection failed.

## 3. What are HTTP headers?

Headers are name/value metadata fields. They describe the message,
content, caching, authentication, cookies, and other behavior.

Header names are case-insensitive: `Content-Type`, `content-type`, and
`CONTENT-TYPE` are the same field name.

### Common response headers

  ----------------------------------------------------------------------------------------------------------
  Header                  Purpose                 Example
  ----------------------- ----------------------- ----------------------------------------------------------
  `Date`                  Time message was        `Date: Wed, 12 Nov 2026 10:00:00 GMT`
                          generated               

  `Server`                Server software         `Server: nginx`
                          identifier; may be      
                          omitted                 

  `Content-Type`          Media type of body      `Content-Type: application/json`

  `Content-Length`        Content size in bytes   `Content-Length: 128`
                          when applicable         

  `Content-Encoding`      Compression/coding of   `Content-Encoding: gzip`
                          representation          

  `Content-Language`      Language of             `Content-Language: en`
                          representation          

  `Content-Disposition`   Inline or attachment    `Content-Disposition: attachment; filename="report.pdf"`
                          behavior; suggested     
                          filename                

  `Content-Location`      Location associated     `Content-Location: /report`
                          with representation     

  `ETag`                  Identifier for          `ETag: "abc123"`
                          representation version  

  `Last-Modified`         Last modification date  `Last-Modified: Tue, 11 Nov 2026 12:00:00 GMT`

  `Vary`                  Request headers that    `Vary: Accept-Encoding`
                          affect selected         
                          representation          

  `Location`              Redirect or related     `Location: /login`
                          target URL              

  `Allow`                 Methods supported by    `Allow: GET, HEAD`
                          resource                

  `Retry-After`           When a client should    `Retry-After: 120`
                          retry                   

  `WWW-Authenticate`      Authentication          `WWW-Authenticate: Basic realm="Members"`
                          challenge               

  `Accept-Ranges`         Indicates range-request `Accept-Ranges: bytes`
                          support                 

  `Content-Range`         Describes returned      `Content-Range: bytes 0-99/500`
                          range                   

  `Age`                   Approximate time        `Age: 60`
                          response has spent in a 
                          shared cache            

  `Via`                   Identifies intermediary `Via: 1.1 proxy`
                          proxies                 
  ----------------------------------------------------------------------------------------------------------

Some headers can occur more than once. In particular, multiple
`Set-Cookie` fields must not be treated as an ordinary comma-joined
header.

## 4. Content-Type

`Content-Type` tells the client what kind of content the message body
contains.

Examples:

``` http
Content-Type: text/html; charset=utf-8
Content-Type: application/json
Content-Type: text/plain; charset=utf-8
Content-Type: image/png
Content-Type: application/pdf
Content-Type: application/xml
Content-Type: text/css
Content-Type: application/javascript
Content-Type: application/octet-stream
```

  Media type                   Typical content
  ---------------------------- ---------------------
  `text/html`                  HTML page
  `text/plain`                 Plain text
  `application/json`           JSON data
  `application/xml`            XML data
  `application/pdf`            PDF document
  `application/octet-stream`   Generic binary data
  `image/jpeg`                 JPEG image
  `image/png`                  PNG image
  `image/svg+xml`              SVG image
  `audio/mpeg`                 MP3 audio
  `video/mp4`                  MP4 video
  `text/css`                   CSS stylesheet
  `application/javascript`     JavaScript

A parameter such as `charset=utf-8` describes the character encoding.
Compare:

-   `Content-Type`: describes the content actually sent in this message.
-   `Accept`: request header telling the server which response media
    types the client can accept.

For uploads, `multipart/form-data` is a common **request** content type.

## 5. Content-Length and Content-Encoding

### Content-Length

`Content-Length` gives the message content size in **bytes**, not
characters.

``` http
Content-Length: 1024
```

That is 1,024 bytes. A Unicode character may occupy multiple UTF-8
bytes. Streaming and some protocol framing arrangements may mean this
header is absent. Compression and automatic decompression also mean the
byte count exposed by a library may differ from the transferred
representation size.

In Requests:

``` python
print(response.headers.get("Content-Length"))
print(len(response.content))  # bytes exposed by Requests
print(len(response.text))     # decoded Python characters
```

Do not assume `len(response.text)` equals `Content-Length`.

### Content-Encoding

Describes coding applied to the representation, commonly compression:

``` http
Content-Encoding: gzip
Content-Encoding: br
Content-Encoding: zstd
```

Requests normally decompresses supported response content when you
access `.content` or `.text`.

`Content-Encoding` is not the same as HTTP/1.1 `Transfer-Encoding`,
which concerns message transfer framing.

### Content-Disposition

Often used for downloads:

``` http
Content-Disposition: attachment; filename="backup.zip"
```

-   `inline`: display the content if possible.
-   `attachment`: treat it as a download.

A filename from a server is untrusted input; validate it before using it
as a filesystem path.

## 6. Request headers versus response headers

This distinction is essential when reading Burp Suite history.

### Common request headers

  Header                Purpose
  --------------------- -----------------------------------------------
  `Host`                Identifies target host in HTTP/1.1
  `User-Agent`          Identifies client software
  `Accept`              Lists acceptable response media types
  `Accept-Encoding`     Lists supported content encodings
  `Accept-Language`     Lists preferred human languages
  `Authorization`       Carries credentials or an access token
  `Cookie`              Sends stored cookies to the server
  `Content-Type`        Describes request body
  `Content-Length`      Request body size in bytes, when applicable
  `Origin`              Origin initiating a request
  `Referer`             Referring page, subject to browser policy
  `If-None-Match`       Conditional request using an ETag
  `If-Modified-Since`   Conditional request using a modification date
  `Range`               Requests part of a representation

### Common response headers

  -----------------------------------------------------------------------
  Header                              Purpose
  ----------------------------------- -----------------------------------
  `Set-Cookie`                        Asks client to store a cookie

  `Location`                          Redirect/target URL

  `WWW-Authenticate`                  Describes authentication challenge

  `Retry-After`                       Suggests when to retry

  `Cache-Control`                     Caching directives

  `Expires`                           Expiration date for caching

  `ETag`                              Representation version identifier

  `Last-Modified`                     Modification time

  `Access-Control-Allow-Origin`       CORS permission for an origin

  `Strict-Transport-Security`         Requests HTTPS-only behavior in
                                      supporting browsers

  `Content-Security-Policy`           Restricts browser resource loading
                                      and actions

  `X-Content-Type-Options`            Can prevent MIME sniffing with
                                      `nosniff`

  `X-Frame-Options`                   Legacy framing restriction

  `Referrer-Policy`                   Controls referrer information

  `Permissions-Policy`                Controls selected browser features
  -----------------------------------------------------------------------

A header's presence does not mean Requests enforces browser security
behavior. Requests is an HTTP client, not a browser.

## 7. User-Agent

`User-Agent` is normally sent by the client in the request.

Example browser-style value:

``` http
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 Chrome/157.0.0.0 Safari/537.36
```

Requests normally sends a value identifying its library and version,
such as `python-requests/2.x`.

Set one explicitly:

``` python
import requests

headers = {
    "User-Agent": "MyLearningClient/1.0",
    "Accept": "application/json",
}

response = requests.get(
    "https://httpbin.org/headers",
    headers=headers,
    timeout=15,
)

print(response.text)
```

Inspect what your code sent:

``` python
print(response.request.headers.get("User-Agent"))
```

This is not the response's headers; it is the prepared request
associated with the response. Changing User-Agent does not turn Requests
into a browser.

## 8. Cookies: Cookie versus Set-Cookie

Cookies allow a server to ask a client to store small pieces of state
and send them on later matching requests.

### Server sets a cookie

``` http
Set-Cookie: sessionid=abc123; Path=/; HttpOnly; Secure; SameSite=Lax
```

  -----------------------------------------------------------------------
  Part                                Meaning
  ----------------------------------- -----------------------------------
  `sessionid=abc123`                  Cookie name and value

  `Path=/`                            Path scope

  `HttpOnly`                          Browser JavaScript cannot read it
                                      through `document.cookie`

  `Secure`                            Browser sends it only over secure
                                      transport

  `SameSite=Lax`                      Restricts some cross-site sending
                                      contexts
  -----------------------------------------------------------------------

The client later sends:

``` http
Cookie: sessionid=abc123
```

Remember:

-   `Set-Cookie` is a response header.
-   `Cookie` is a request header.
-   Cookie attributes such as `HttpOnly`, `Secure`, `Path`, and
    `SameSite` are not copied into the request `Cookie` header.

### Important cookie attributes

  -----------------------------------------------------------------------
  Attribute                           Purpose
  ----------------------------------- -----------------------------------
  `Expires`                           Absolute expiry date

  `Max-Age`                           Lifetime in seconds

  `Domain`                            Domain scope, subject to cookie
                                      rules

  `Path`                              Path scope

  `Secure`                            Send only over secure transport

  `HttpOnly`                          Prevent access through browser
                                      JavaScript

  `SameSite=Strict`                   Strong cross-site sending
                                      restriction

  `SameSite=Lax`                      Allows some top-level cross-site
                                      navigations

  `SameSite=None`                     Allows cross-site sending; modern
                                      browsers require `Secure`

  `Partitioned`                       Requests partitioned cookie storage
                                      in supporting browsers
  -----------------------------------------------------------------------

A cookie without `Expires` or `Max-Age` is generally a session cookie.
`HttpOnly` does not stop the browser sending the cookie; it stops page
JavaScript reading it. Cookies are not inherently encrypted, so use
HTTPS.

### Inspect cookies with Requests

``` python
import requests

session = requests.Session()
response = session.get("https://example.com/", timeout=15)

# Cookies set by this response
print(response.cookies.get_dict())

# Cookies accumulated in the session
for cookie in session.cookies:
    print("Name:", cookie.name)
    print("Value:", cookie.value)
    print("Domain:", cookie.domain)
    print("Path:", cookie.path)
    print("Secure:", cookie.secure)
    print("Expires:", cookie.expires)
```

`response.cookies` represents cookies set in that response.
`session.cookies` is the session's accumulated cookie jar. Requests is
not a browser and does not enforce every browser cookie policy
identically.

A server may send multiple `Set-Cookie` fields. Do not split them
naively on commas; cookie expiry dates can contain commas. Use a cookie
jar.

## 9. Redirects and Location

Example:

``` http
HTTP/1.1 302 Found
Location: https://example.com/login
```

Requests follows redirects by default for common methods such as GET.

``` python
response = requests.get("https://example.com/old", timeout=15)

print("Final URL:", response.url)
print("Redirect history:", response.history)

for old_response in response.history:
    print(old_response.status_code, old_response.headers.get("Location"))
```

Inspect the first response without following it:

``` python
response = requests.get(
    "https://example.com/old",
    allow_redirects=False,
    timeout=15,
)

print(response.status_code)
print(response.headers.get("Location"))
```

`response.history` contains earlier redirect responses; `response.url`
is normally the final URL.

## 10. Caching headers

  -----------------------------------------------------------------------
  Header/directive                    Meaning
  ----------------------------------- -----------------------------------
  `Cache-Control: no-store`           Caches should not store the
                                      response

  `Cache-Control: no-cache`           Stored response generally needs
                                      revalidation before reuse

  `Cache-Control: max-age=3600`       Freshness lifetime of 3,600
                                      seconds, subject to other rules

  `Cache-Control: private`            Intended for private caches, not
                                      shared caches

  `Cache-Control: public`             May be stored by shared caches when
                                      other rules permit

  `Expires`                           Expiration date

  `ETag`                              Representation version identifier

  `Last-Modified`                     Modification date

  `Vary`                              Request fields that affect
                                      representation selection

  `Age`                               Approximate time in a shared cache
  -----------------------------------------------------------------------

A client may send:

``` http
If-None-Match: "version-123"
```

If unchanged, the server may return `304 Not Modified`, normally without
a representation body.

## 11. Authentication and browser security headers

### Authentication

`WWW-Authenticate` is a response header that can challenge a client:

``` http
HTTP/1.1 401 Unauthorized
WWW-Authenticate: Basic realm="Members"
```

`Authorization` is normally a request header:

``` http
Authorization: Bearer YOUR_ACCESS_TOKEN
```

Difference:

-   `401`: credentials are missing or unacceptable; normally includes an
    authentication challenge.
-   `403`: server understood the request but refuses authorization.

Never print real tokens, passwords, or session-cookie values in logs.

### Browser security headers

  ---------------------------------------------------------------------------------------------
  Header                        Example                                 Purpose
  ----------------------------- --------------------------------------- -----------------------
  `Strict-Transport-Security`   `max-age=31536000; includeSubDomains`   Requests HTTPS-only
                                                                        behavior in supporting
                                                                        browsers

  `Content-Security-Policy`     `default-src 'self'`                    Restricts resource
                                                                        sources and browser
                                                                        actions

  `X-Content-Type-Options`      `nosniff`                               Prevents certain
                                                                        MIME-sniffing behavior

  `X-Frame-Options`             `DENY`                                  Legacy framing
                                                                        protection

  `Referrer-Policy`             `strict-origin-when-cross-origin`       Controls referrer
                                                                        information

  `Permissions-Policy`          `camera=(), microphone=()`              Restricts selected
                                                                        browser features
  ---------------------------------------------------------------------------------------------

### CORS headers

``` http
Access-Control-Allow-Origin: https://frontend.example
Access-Control-Allow-Methods: GET, POST, OPTIONS
Access-Control-Allow-Headers: Content-Type, Authorization
Access-Control-Allow-Credentials: true
Access-Control-Expose-Headers: X-Request-ID
Access-Control-Max-Age: 600
```

CORS is a browser-enforced cross-origin access mechanism, not
authentication. Requests does not enforce browser CORS rules.
Credentialed CORS responses cannot use `Access-Control-Allow-Origin: *`;
the server must specify an allowed origin.

## 12. Python Requests: response attributes and methods

Assume:

``` python
response = requests.get("https://example.com", timeout=15)
```

### Important attributes/properties

  -----------------------------------------------------------------------
  Attribute                           Meaning
  ----------------------------------- -----------------------------------
  `response.status_code`              Numeric HTTP status

  `response.reason`                   Reason phrase, if available

  `response.headers`                  Response header mapping

  `response.text`                     Body decoded to a Python string

  `response.content`                  Body as bytes

  `response.url`                      Final URL

  `response.encoding`                 Encoding used for `.text`

  `response.apparent_encoding`        Encoding guessed from content, when
                                      supported

  `response.cookies`                  Cookies set in this response

  `response.history`                  Earlier redirect responses

  `response.elapsed`                  Time until response headers were
                                      received

  `response.request`                  Prepared request associated with
                                      response

  `response.ok`                       True for status codes below 400

  `response.is_redirect`              Whether response is a redirect

  `response.is_permanent_redirect`    Whether response is a permanent
                                      redirect

  `response.raw`                      Lower-level response stream
  -----------------------------------------------------------------------

### Important methods

  Method                          Purpose
  ------------------------------- --------------------------------------
  `response.json()`               Parse JSON body into Python data
  `response.raise_for_status()`   Raise HTTPError for 4xx/5xx
  `response.iter_content()`       Iterate over body chunks
  `response.iter_lines()`         Iterate over lines in streamed body
  `response.close()`              Close response and release resources

`response.ok` does not mean exactly status 200; it is true for status
codes below 400. `raise_for_status()` raises for 4xx and 5xx, not
ordinary redirects.

## 13. `.text`, `.content`, and `.json()`

  -----------------------------------------------------------------------
  Expression              Python result           Use
  ----------------------- ----------------------- -----------------------
  `response.text`         `str`                   HTML, readable text

  `response.content`      `bytes`                 Images, PDFs, archives,
                                                  raw body bytes

  `response.json()`       Usually `dict` or       JSON API response
                          `list`                  
  -----------------------------------------------------------------------

Example:

``` python
print(type(response.text))     # str
print(type(response.content))  # bytes
```

For a JSON body:

``` json
{"username": "alice", "active": true}
```

`.json()` produces a Python dictionary with `True` as the Boolean value.
Do not assume every response is JSON; parsing can fail if the body is
empty, HTML, plain text, or invalid JSON.

## 14. Complete response-inspection example

``` python
import requests

url = "https://httpbin.org/response-headers?Content-Type=application/json"

try:
    response = requests.get(
        url,
        headers={
            "User-Agent": "HTTP-Learning-Client/1.0",
            "Accept": "application/json",
        },
        timeout=(5, 20),
    )

    print("Status:", response.status_code)
    print("Reason:", response.reason)
    print("Final URL:", response.url)
    print("Encoding:", response.encoding)
    print("Elapsed:", response.elapsed)

    print("\\n--- RESPONSE HEADERS ---")
    for name, value in response.headers.items():
        print(f"{name}: {value}")

    print("\\nContent-Type:", response.headers.get("Content-Type"))
    print("Content-Length:", response.headers.get("Content-Length"))
    print("Content-Encoding:", response.headers.get("Content-Encoding"))
    print("Cache-Control:", response.headers.get("Cache-Control"))
    print("Server:", response.headers.get("Server"))
    print("Location:", response.headers.get("Location"))

    print("\\n--- REQUEST HEADERS SENT ---")
    for name, value in response.request.headers.items():
        print(f"{name}: {value}")

    print("User-Agent sent:", response.request.headers.get("User-Agent"))

    print("\\n--- BODY ---")
    print(response.text)
    print("Body bytes exposed by Requests:", len(response.content))

    print("\\n--- COOKIES SET BY THIS RESPONSE ---")
    for cookie in response.cookies:
        print(cookie.name, cookie.value)

    print("\\n--- REDIRECT HISTORY ---")
    for old_response in response.history:
        print(old_response.status_code, old_response.url)

except requests.exceptions.Timeout:
    print("The request timed out.")

except requests.exceptions.ConnectionError as error:
    print("Connection problem:", error)

except requests.exceptions.RequestException as error:
    print("Request problem:", error)
```

The example uses a public HTTP testing endpoint; use an endpoint you
control if it is unavailable.

## 15. Downloading a large response safely

For a large file, stream it rather than loading the entire body into
memory:

``` python
import requests

url = "https://example.com/large-file.zip"

with requests.get(url, stream=True, timeout=(5, 60)) as response:
    response.raise_for_status()

    with open("large-file.zip", "wb") as file:
        for chunk in response.iter_content(chunk_size=8192):
            if chunk:
                file.write(chunk)
```

-   `stream=True` delays downloading the full body.
-   `iter_content()` yields chunks.
-   `chunk_size=8192` is 8 KiB.
-   The `with` block closes the response.
-   Validate downloaded content and filenames before using them.

## 16. Burp Suite: reading a request and response

In an authorized lab:

1.  Open **Proxy → HTTP history**.
2.  Select a request.
3.  Inspect the **Request** pane.
4.  Inspect the **Response** pane.
5.  Compare headers, cookies, status, redirects, and body.

Example request:

``` http
GET /account HTTP/1.1
Host: example.test
User-Agent: Python-requests/2.x
Accept: */*
Cookie: session=example-session-value
```

Example response:

``` http
HTTP/1.1 200 OK
Content-Type: text/html; charset=utf-8
Set-Cookie: preference=dark; Path=/; Secure; SameSite=Lax
Cache-Control: no-store

<html><body>Account</body></html>
```

Notice: - Request `Cookie` sends a cookie already held by the client. -
Response `Set-Cookie` asks the client to store/update a cookie. -
Response `Content-Type` describes the body. - `User-Agent` belongs to
the request.

### HTTP/1.1 and HTTP/2

HTTP/1.1 is commonly displayed as a textual status line and headers.
HTTP/2 uses binary framing and HPACK header compression on the wire.
Burp can display HTTP/2 traffic in a readable representation.

Python Requests/urllib3 normally uses HTTP/1.1. An error such as
`UnknownProtocol('HTTP/2')` can indicate that HTTP/2 protocol handling
reached a component expecting HTTP/1.x. Inspect the proxy and identify
the failing request; it is not normally fixed by changing `Content-Type`
or `User-Agent`.

## 17. Common mistakes

### Mistake: looking for User-Agent in response headers

Usually wrong:

``` python
print(response.headers.get("User-Agent"))
```

Inspect what your client sent:

``` python
print(response.request.headers.get("User-Agent"))
```

### Mistake: treating Content-Length as character count

Do not assume:

``` python
len(response.text) == int(response.headers["Content-Length"])
```

Encoding, compression, and transfer handling make this comparison
unreliable.

### Mistake: assuming every response is JSON

Check the media type and still handle parse errors:

``` python
content_type = response.headers.get("Content-Type", "")

if "application/json" in content_type:
    data = response.json()
else:
    print("Response is not identified as JSON.")
```

### Mistake: confusing Cookie and Set-Cookie

-   `Cookie`: client to server.
-   `Set-Cookie`: server to client.

### Mistake: confusing Content-Type and Accept

-   `Content-Type`: describes content in this message.
-   `Accept`: client preference for response media types.

### Mistake: treating HTTP errors as connection errors

A server can return a valid HTTP `404` or `500` response. Requests
raises for those only when `raise_for_status()` is called.

## 18. Security and privacy best practices

-   Use HTTPS for sensitive data.
-   Do not log passwords, bearer tokens, session IDs, or full sensitive
    cookie values.
-   Avoid `verify=False` outside controlled testing; it disables TLS
    certificate verification.
-   Treat response bodies and server-provided filenames as untrusted
    input.
-   Set connection and read timeouts.
-   Limit download sizes when dealing with untrusted servers.
-   Be careful following redirects when credentials or sensitive data
    are involved.
-   Keep tokens out of source code and version control.
-   Test only systems you own or are authorized to assess.

## 19. Quick reference

``` python
# Status
response.status_code
response.reason
response.ok

# URL and redirects
response.url
response.history
response.is_redirect

# Response headers
response.headers
response.headers.get("Content-Type")
response.headers.get("Content-Length")
response.headers.get("Content-Encoding")
response.headers.get("Set-Cookie")
response.headers.get("Location")

# Request associated with the response
response.request
response.request.headers
response.request.headers.get("User-Agent")
response.request.headers.get("Cookie")

# Body
response.text
response.content
response.json()

# Cookies
response.cookies
response.cookies.get_dict()

# Timing and encoding
response.elapsed
response.encoding
response.apparent_encoding

# Errors and cleanup
response.raise_for_status()
response.close()
```

## 20. Checklist for inspecting any HTTP response

1.  What status code did the server return?
2.  What is the final URL?
3.  Was there a redirect? What were the earlier responses?
4.  What is the response `Content-Type`?
5.  Is `Content-Length` present? Is the body compressed?
6.  What does the response body contain?
7.  Did the server send `Set-Cookie`?
8.  What caching headers are present?
9.  Are browser security headers present?
10. What request headers did my client send?
11. What User-Agent did I send?
12. Did the server return an HTTP response, or did the connection fail
    before one arrived?

If you can answer these questions, you can read most ordinary HTTP
exchanges confidently.
