##CHAPTER 1

###MIME types
- A MIME(Multipurpose Internet Mail Extensions) type is a textual lable, represented as a primary object type and a specific subtype, separated by a slash.


###URLs

Most URLs follow a standardized format of three main parts:

	1. The first part of the URL is called the *scheme*, and it describes the protocol used to access the resource. This is usually the HTTP protocol
	2. The second part gives the server Internet address
	3. The rest names a resource on the web server.

###Method

|HTTP method | Description |
|----------|------------|
|GET   |   Send named resource from the server to the client. |
|PUT   |   Store data from client into a named server resource. |
|DELETE   | Delete the named resource from a server. |
|POST  |   Send client data into a server gateway application. |
|HEAD  |  Send just the HTTP headers from the response for the named resource. |

###Status Codes

|	HTTP status code|	Description|
|-----------|----------|
|	200|	OK. Document returned correctly	|
|	302|	Redirect. Go someplace else to get the resource|
|	404|	Not Found. Can't find this resource|

HTTP also sends an explanatory textual "reason phrase" with each numeric status code. The textual phrase is included only for descriptive purposes.

HTTP messages consist of three parts:

 	1. Start line: The first line of the message is the start line, indicating what to do for a request or what happened for a response.
	2. Header fields: Zero of more header fields follow the start line, Each header field consists of a name and a value, separated by a colon (:) for easy parsing. The headers end with a blank line. Adding a header field is as easy as adding another line.
	3. Body: After the blank line is an optional message body containing any kind of adata. Request bodies carry data to the web server; response bodies carry data back to the client. Unlike the start lines and headers, which are textual and structured, the body can contain arbitrary binary data (e.g., images, video, audio tracks, software applications.)

###TCP/IP
- HTTP is an application layer protocol. HTTP doesn't worry about the nitty-gritty details of network communication; instead, it leaves the details of the networking to TCP/IP, the popular reliable Internet transport protocol.


TCP provides:

	- Error-free data transportation
	- In-order delivery(data will always arrive in the order in which it was sent)
	- Unsegmented data stream (can dribble out data in any size at any time)


|------|-------|
|	HTTP|	 application layer|
|	TCP	|	transport layer|
|	IP	|	network layer|
|	Networt-specifiv link interface| Data link layer|
|	Physical network hardware| Physical layer|


###Connections, IP Addresses, and Port Numbers
Before an HTTP client can send a message to a server, it needs to establish a TCP/IP connection between the client and server using Internet protocol (IP) addresses and port numbers.


steps:

	1. The browser extracts the server's hostname from the URL.
	2. The browser converts the server's hostname into the server's IP address.
	3. The browser extracts the port number (if any) from the URL.
	4. The browser establishes a TCP connection with the web server.
	5. The browser sends an HTTP request message to the server.
	6. The server sends an HTTP response back to the browser.
	7. The connection is closed, and the browser displays the document.


###Protocal Versions


###Architectural Components of the web
- Proxies: HTTP intermediaries that sit between clients and servers.
- Caches: HTTP storehouses that keep copies of popular web pages close to clients
- Gateways: Special web servers that connect to other applications.
- Tunnels: Special proxies that blindly forward HTTP communications
- Agents: Semi-intelligent web clients that make automated HTTP requests

##Chapter 2 URLs and Resources

####Relative URLs
URLs come in two flavors: `absolute` and `relative`.

##Chapter 3 HTTP Messages
###Messages Commute Inbound to the Origin Server
HTTP uses the terms `inbound` and `outbound` to describe transactional direction.

###The Parts of a Message
Each message contains either a request from a client or a response from a server. They consist of three parts: a start line describing the message, a block of headers containing attributes, and an optional body containing data.

#####Methods

|Method| Description| Message body|
|-----|-----|------|
|GET| Get a document from the server| No|
|HEAD| Get just the headers for a document from the server| No|
|POST| Send data to the server for processing| Yes|
|PUT| Store the body of the requests on the server| Yes|
|TRACE| Trace the message through proxy servers to the server| No|
|OPTIONS| Determine what methods can operate on a server| No|
|DELETE| Remove a document from the server| No|


#####Status code classes
|Overall range| Defined range| Category|
|---|----|----|
|100-199|100-101|Informatinal|
|200-299|200-206|Successful|
|300-399|300-305|Redirection|
|400-499|400-415|Client error|
|500-599|500-505|Server error|



###headers
- General headers: These are generic headers used by both clients and servers. They serve general purposes that are useful for clients, servers and other applications to supply to one another.
- Request headers: As the name implies, request headers are specific to request messages. They provide extra information to servers, such as what type of data the client is willing to receive.
- Response headers: Response messages have their own set of headers that provide informations to the client.
- Entity headers: Entity headers refer to headers that deal with the entity body
- Extension headers: Extension headers are nonstandard headers that have been created by application developers but not yet added to the sanctioned HTTP specification. HTTP programs need to tolerate and forward extension headers, even if they don't know what the headers mean.


##Chapter 7 Caching

- Revalidate hit : If the server object isn't modified, the server sends the client a small HTTP 304 Not modified response.
- Revalidate miss : If the server object is different from the cached copy, the server sends the client a normal HTTP 200 OK response, with the full content.
- Object delete: If the server object has been deleted, the server sends back a 404 Not Found response, and the cache deletes its copy.


###Expiration Dates and Ages
- Servers specify expiration dates using the HTTP/1.0+ Expires or the HTTP/1.1 Cache-Control: max-age response headers, which accompany a response body. The Expires and Cache-Control: max-age headers do basically the same thing, but the newer Cache-Control header is preferred, because it uses a relative time instead of an absolute date.

|	Header|	Description|
|	-----|	---------|
|	Cache-Control:max-age| The max-age value defines the maximum age of the document--maximum legal elapsed time (in seconds) from when a document is first generated to when it can no longer be considered fresh enough to serve |
|	Expires|	Specifies an absolute expiration date. IF the expiration date is in the past, the document is no longer fresh.|

###Server Revalidation
Just because a cached document has expired doesn't mean it is actually different from what's living on the origin server; it just means that it's time to check. This is called "server revalidation", meaning the cache needs to ask to origin server whether the document has changed:


	- If revalidation shows the content has changed, the cache gets a new copy of the document, stores it in place of the old data, and sends the document to the client.
	- If revalidation shows the contents has not changed, the cache only gets new headers, including a new expiration date, and updates the headers in the cache.

###Revalidation with Conditional Methods
| Header|	Description|
|-------|---------|
|If-Modified-Since:<date>| Perform the requested method if the document has been modified since the specified date. This is used in conjunction with the Last-Modified server response header, to fetch content only if the content has been modified from the cached version.|
|If-None-Match:<tags>|Instead of matching on last-modified date, the server may provide special tags on the document that act like serial numbers. The If-None-Match header Performs the requested method if the cached tags differ from the tags in the server's document|
