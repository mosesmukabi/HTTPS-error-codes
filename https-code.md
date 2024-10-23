# HTTP response status codes

HTTP response status codes are a standard set of codes that indicate the result of a client's request to a server. They are divided into five classes based on their first digit:

## 1xx Informational

These codes indicate that the request was received and understood, and that the process is continuing.

### 100 Continue

The server received the initial part of the request and the client should continue.

#### Status

```
HTTP/1.1 100 Continue
```

### 101 Switching Protocols

The server is switching to a different protocol as requested by the client.

#### syntax

```
res.writeHead(101, { "Connection": "Upgrade", "Upgrade": "websocket" });
res.end();
```

### 102 Processing

The server is processing the request, but no response is available yet.

#### syntax

```
res.status(102).send("Processing request...");

```

### 103 Early Hints

The server is using a proxy to deliver the request and is waiting for the response.

#### syntax

```
103 Early Hint
Link: <https://cdn.example.com>; rel=preconnect, <https://cdn.example.com>; rel=preconnect; crossorigin
```

## Successful responses

These codes indicate that the client's request was successfully received, understood, and accepted.

### 200 OK

The request was successful, and the server is returning the requested resource.

#### Status

```
HTTP/1.1 200 OK
```

#### syntax

```
res.status(200).json({ message: "Request successful", data: data });

```

## 201 Created

The request was successful, and a new resource has been created. This is typically the response sent after POST requests, or some PUT requests.

#### status

```
201 Created
```

#### syntax

```
res.status(201).json({ message: "Resource created successfully", id: newResourceId });

```

## 202 Accepted

The request has been accepted for processing but is not yet complete. It is intended for cases where another process or server handles the request, or for batch processing.

#### status

```
202 Accepted
```

#### syntax

```
res.status(202).json({ message: "Resource created successfully", id: newResourceId });

```

## 203 Non-Authoritative Information

This response code means the returned metadata is not exactly the same as is available from the origin server, but is collected from a local or a third-party copy.

#### status

```
203 Non-Authoritative Information
```

#### syntax

```
res.status(203).json({ message: "Non-authoritative information", data: thirdPartyData });

```

## 204 No Content

The server successfully processed the request and is not returning any content. used in `delete` and `put`

#### status

```
204 No Content
```

#### syntax

```
res.status(204).end();
```

## 205 Reset Content

The request was successful, and the client should reset the document view.
This response is intended to support use cases where the user receives content that supports data entry, submits user-edited data in a request, and the content needs to be reset for the next entry. The instruction to "reset content" can mean clearing the contents of a form, resetting a canvas state, or refreshing a UI; the implementation depends on the client.

#### status

```
205 Reset Content
```

#### syntax

```
res.status(205).send("Reset your form after this response.");
```

## 206 Partial Content

The server is delivering only part of the resource due to a range header sent by the client.

#### status

```
206 Partial Content
```

#### syntax

```
res.status(206).send("Here is part of the resource you requested.");
```

## 207 Multi-Status

The message body contains XML, which can contain multiple separate responses.

#### status

```
207 Multi-Status
```

#### syntax

```
res.status(207).send("Here is part of the resource you requested.");
```

## 208 Already Reported

Used inside a `<dav:propstat>` response element to avoid repeatedly enumerating the internal members of multiple bindings to the same collection.
used when The members of a DAV binding have already been enumerated in a previous reply.

#### status

```
208 Already Reported
```

#### syntax

```
res.status(208).send("Here is part of the resource you requested.");

<D:status>HTTP/1.1 208 Already Reported</D:status>

```

## 226 IM Used

IM stands for instance manipulation, which refers to the algorithm generating a `delta`. In delta encoding, a client sends a `GET` request with two headers: `A-IM:`  
 indicates that the server is returning a `delta` in response to a `GET` request. It is used in the context of HTTP `delta` encodings.

#### status

```
226 IM Used
```

#### syntax

```
res.status(226).send("Here is part of the resource you requested.");
```

## Redirection messages

These codes indicate that the client needs to take an action based on the response received from the server, typically involving a redirection.

### 300 Multiple Choices

The request resource has more than one representation, each with its own more specific location, and agent-specific accept information, and none of them can be returned by the server.

### status

```
300 Multiple Choices
```

#### syntax

```
res.status(300).json({ options: ["http://example.com/resource1", "http://example.com/resource2"] });
```

### 301 Moved Permanently

The HTTP `301 Moved Permanently `redirection response status code indicates that the requested resource has been permanently moved to the `URL` in the Location `header`.

#### status

```
301 Moved Permanently
```

#### syntax

```
res.redirect(301, "http://new-url.com");
```

### 302 Found

the HTTP `302 Found `redirection response status code indicates that the requested resource has been temporarily moved to the `URL` in the Location `header`.

#### status

```
302 Found
```

#### syntax

```
res.redirect(302, "http://new-url.com");
```

### 303 See Other

The server is redirecting the client to a different resource using a GET request.

#### status

```
303 See Other
```

#### syntax

```
res.redirect(303, "http://new-url.com");
```

### 304 Not Modified

The HTTP `304 Not Modified` redirection response status code indicates that there is no need to retransmit the requested resources.
This response code is sent when the request is a conditional `GET or HEAD` request with an If-None-Match or an If-Modified-Since `header` and the condition evaluates to `'false'`. It confirms that the resource cached by the client is still valid and that the server would have sent a `200 OK `response with the resource if the condition evaluated to `'true'`. See HTTP caching for more information.

#### status

```
304 Not Modified
```

#### syntax

```
res.status(304).send();  // No need to send new data as the resource hasn't changed
```

### 307 Temporary Redirect

The resource is temporarily located at a different URL, but the same method should be used. A browser receiving this status will automatically request the resource at the URL in the Location header, redirecting the user to the new page. Search engines receiving this response will not attribute links to the original URL to the new resource, meaning no `SEO` value is transferred to the new URL.

#### status

```
307 Temporary Redirect
```

#### syntax

```
res.redirect(307, "http://temporary-location.com");
```

### 308 Permanent Redirect

The resource has been permanently moved to a different URL, and future requests should use the new URL. A browser receiving this status will automatically request the resource at the URL in the Location `header`, redirecting the user to the new page. Search engines receiving this response will attribute links to the original URL to the redirected resource, passing the SEO ranking to the new URL.

#### status

```
308 Permanent Redirect
```

#### syntax

```
res.redirect(308, "http://new-permanent-location.com");
```

## client errors responses

These codes indicate that the client made a request that this server could not understand.

### 400 Bad Request

The server couldn't understand the request due to invalid syntax, such as a misspelling or a syntax error in the request, invalid request message framing, or deceptive request routing.

#### status

```
400 Bad Request
```

#### syntax

```
res.status(400).json({ error: "Invalid request parameters" });
```

### 401 Unauthorized

This status code indicates that a request was not successful because it lacks valid authentication credentials for the requested resource.

#### status

```
401 Unauthorized
```

#### syntax

```
res.status(401).json({ error: "Unauthorized access - please log in" });
```

### 402 Payment Required

Reserved for future use. Intended for digital payment systems.
This status code was created to enable digital cash or (micro) payment systems and would indicate that requested content is not available until the client makes a payment.

#### status

```
402 Payment Required
```

#### syntax

```
res.status(402).json({ error: "Payment required - please make a payment" });
```

### 403 Forbidden

The server understood the request but refuses to authorize it.

#### status

```
403 Forbidden
```

#### syntax

```
res.status(403).json({ error: "Access forbidden - please log in" });
```

### 404 Not Found

The server has not found anything matching the Request-URI. No indication is given of whether the condition is temporary or permanent.

#### status

```
404 Not Found
```

#### syntax

```
res.status(404).json({ error: "Resource not found" });
```

### 405 Method Not Allowed

The request method is known by the server but is not supported by the target resource. The server must generate an Allow header in a 405 response with a list of methods that the target resource currently supports.

#### status

```
405 Method Not Allowed
```

#### syntax

```
res.status(405).json({ error: "Method not allowed" });
```

### 406 Not Acceptable

The resource identified by the request is only capable of generating content not acceptable according to the Accept headers sent in the request.

#### status

```
406 Not Acceptable
```

#### syntax

```
res.status(406).json({ error: "Resource not acceptable" });
```

### 407 Proxy Authentication Required

The server requires the client to first authenticate itself with the proxy.

#### status

```
407 Proxy Authentication Required
```

#### syntax

```
res.status(407).json({ error: "Proxy authentication required" });
```

### 408 Request Timeout

The server timed out waiting for the request. The client should repeat the request without modifications at any later time.

#### status

```
408 Request Timeout
```

#### syntax

```
res.status(408).json({ error: "Request timed out" });
```

### 409 Conflict

The request could not be processed because of conflict in the request, such as an edit conflict.

#### status

```
409 Conflict
```

#### syntax

```
res.status(409).json({ error: "Conflict in the current state of the resource" });
```

### 410 Gone

The requested resource is no longer available at the server and no forwarding address is known. This condition is expected to be used for caching mechanisms.

#### status

```
410 Gone
```

#### syntax

```
res.status(410).json({ error: "Resource no longer available" });
```

### 411 Length Required

The request did not specify the length of its content, which is required by the requested resource.

#### status

```
411 Length Required
```

#### syntax

```
res.status(411).json({ error: "Content length required" });
```

### 412 Precondition Failed

The server does not meet one of the preconditions that the requester put on the request.

#### status

```
412 Precondition Failed
```

#### syntax

```
res.status(412).json({ error: "Precondition failed" });
```

### 413 Payload Too Large

The request is larger than the server is willing or able to process.

#### status

```
413 Payload Too Large
```

#### syntax

```
res.status(413).json({ error: "Request payload too large" });
```

### 414 URI Too Long

The URI requested by the client is longer than the server is willing to interpret.

#### status

```
414 URI Too Long
```

#### syntax

```
res.status(414).json({ error: "Request URI too long" });
```

### 415 Unsupported Media Type

The request entity has a media type which the server or resource does not support.

#### status

```
415 Unsupported Media Type
```

#### syntax

```
res.status(415).json({ error: "Unsupported media type" });
```

### 416 Range Not Satisfiable

The client has asked for a portion of the file but the server cannot supply that portion. For example, if the client asked for a part of the file that lies beyond the end of the file.

#### status

```
416 Range Not Satisfiable
```

#### syntax

```
res.status(416).json({ error: "Range not satisfiable" });
```

### 417 Expectation Failed

The server cannot meet the requirements of the Expect request-header field.

#### status

```
417 Expectation Failed
```

#### syntax

```
res.status(417).json({ error: "Expectation failed" });
```

### 418 I'm a teapot

This error indicates that the server refuses to brew coffee because it is, permanently, a teapot.
This error is a reference to Hyper Text Coffee Pot Control Protocol defined in April Fools' jokes in 1998 and 2014.

#### status

```
418 I'm a teapot
```

#### syntax

```
res.status(418).json({ error: "I'm a teapot" });
```

### 421 Misdirected Request

The request was directed at a server that is not able to produce a response.

#### status

```
421 Misdirected Request
```

#### syntax

```
res.status(421).json({ error: "Misdirected request" });
```

### 422 Unprocessable Entity

The request was well-formed but was unable to be followed due to semantic errors.

#### status

```
422 Unprocessable Entity
```

#### syntax

```
res.status(422).json({ error: "Unprocessable entity" });
```

### 423 Locked

The resource that is being accessed is locked.

#### status

```
423 Locked
```

#### syntax

```
res.status(423).json({ error: "Locked resource" });
```

### 424 Failed Dependency

The request failed because it depended on another request and that request failed.

#### status

```
424 Failed Dependency
```

#### syntax

```
res.status(424).json({ error: "Failed dependency" });
```

### 426 Upgrade Required

The client should switch to a different protocol.

#### status

```
426 Upgrade Required
```

#### syntax

```
res.status(426).json({ error: "Upgrade required" });
```

### 428 Precondition Required

The origin server requires the request to be conditional.

#### status

```
428 Precondition Required
```

#### syntax

```
res.status(428).json({ error: "Precondition required" });
```

### 429 Too Many Requests

The user has sent too many requests in a given amount of time ("rate limiting").

#### status

```
429 Too Many Requests
```

#### syntax

```
res.status(429).json({ error: "Too many requests" });
```

### 431 Request Header Fields Too Large

The server is unwilling to process the request because its header fields are too large. The request may be resubmitted after reducing the size of the request header fields.

#### status

```
431 Request Header Fields Too Large
```

#### syntax

```
res.status(431).json({ error: "Request header fields too large" });
```

### 451 Unavailable For Legal Reasons

A server operator has received a legal demand to deny access to a resource or to a service and believes that access may be necessary to satisfy one or more of its legal requests.

#### status

```
451 Unavailable For Legal Reasons
```

#### syntax

```
res.status(451).json({ error: "Unavailable for legal reasons" });
```

## 5xx Server Error

### 500 Internal Server Error

The server encountered an unexpected condition that prevented it from fulfilling the request.

#### status

```
500 Internal Server Error
```

#### syntax

```
res.status(500).json({ error: "Internal server error" });
```

### 501 Not Implemented

The server does not support the functionality required to fulfill the request.

#### status

```
501 Not Implemented
```

#### syntax

```
res.status(501).json({ error: "Not implemented" });
```

### 502 Bad Gateway

The server, while acting as a gateway or proxy, received an invalid response from an inbound server it accessed.

#### status

```
502 Bad Gateway
```

#### syntax

```
res.status(502).json({ error: "Bad gateway" });
```

### 503 Service Unavailable

The server is currently unable to handle the request due to a temporary overloading or maintenance of the server.

#### status

```
503 Service Unavailable
```

#### syntax

```
res.status(503).json({ error: "Service unavailable" });
```

### 504 Gateway Timeout

The server, while acting as a gateway or proxy, did not receive a timely response from an inbound server it accessed.

#### status

```
504 Gateway Timeout
```

#### syntax

```
res.status(504).json({ error: "Gateway timeout" });
```

### 505 HTTP Version Not Supported

The HTTP version used in the request is not supported by the server.

#### status

```
505 HTTP Version Not Supported
```

#### syntax

```
res.status(505).json({ error: "HTTP version not supported" });
```

### 506 Variant Also Negotiates

this status code is returned during content negotiation when there is recursive loop in the process of selecting a resource. indicates The server has an internal configuration error.

#### status

```
506 Variant Also Negotiates
```

#### syntax

```
res.status(506).json({ error: "Variant also negotiated" });
```

### 507 Insufficient Storage

The server is unable to store the representation needed to complete the request.

#### status

```
507 Insufficient Storage
```

#### syntax

```
res.status(507).json({ error: "Insufficient storage" });
```

### 508 Loop Detected

The server detected an infinite loop while processing the request.

#### status

```
508 Loop Detected
```

#### syntax

```
res.status(508).json({ error: "Loop detected" });
```

### 510 Not Extended

Further extensions to the request are required for the server to fulfil it.

#### status

```
510 Not Extended
```

#### syntax

```
res.status(510).json({ error: "Not extended" });
```

### 511 Network Authentication Required

The client needs to authenticate to gain network access.

#### status

```
511 Network Authentication Required
```

#### syntax

```
res.status(511).json({ error: "Network authentication required" });
```
