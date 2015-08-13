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

|	|	|
|---|---|
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

###Hits and Misses

![](https://cloud.githubusercontent.com/assets/3389862/9252844/1dfab8bc-420d-11e5-854f-2aab03b2bca9.png)

###Revalidation
- Because the origin server content can change, caches have to check every now and then their copies are still up-to-date with the server. These "freshness checks" are called HTTP revalidations.
- When a cache needs to revalidate a cached copy, it sends a small revalidation request to the origin server. If the content hasn't changed, the server responds with a tiny 304 Not Modified response. As soon as the cache learns the copy is still valid, it marks the copy temporarily fresh again and serves the copy to the client. This is called a revalidate hit or a slow hit. It's slower than a pure cache hit, because it does need to check with the origin server, but it's faster than a cache miss, because no object data is retrieved from the server.
![](https://cloud.githubusercontent.com/assets/3389862/9253055/73d2d67e-420e-11e5-9416-5b55a777bfd7.png)


- Revalidate miss: If the server object is different from the cached copy, the server send the client a normal HTTP 200 OK response, with the full content
- Object deleted: If the server object has been deleted, the sever sends back a 404 Not Found response, and the cached deletes its copy

###Distinguishing Hits and Misses

- HTTP provides no way for a client to tell if a response was a cache hit or an origin server access. In both cases, the response code will be 200 OK, indicating that the response has a body. Some commercial proxy caches attach additional information to Via headers to describe what happened in the cache.

- One way that a client can usually detect if the response came form a cache is to use the Date header. By comparing the value of the Date header in the response to the current time, a client can often detect a cached response by its older date value. Another way a client can detect a cached response it the Age header, which tells how old the response is.

###Cache Topologies
- Caches can be dedicated to a single user or shared between thousands of users. Dedicated caches are called *private caches*. Private caches are personal caches, containing popular pages for a single user. Shared caches are called *public caches*. Public caches contain the pages popular in the user community.

###Cache Processing Steps

A basic cache-processing sequence for an HTTP GET message consists of seven steps:

	1. Receiving--Cache reads the arriving request message from the network
	2. Parsing--Cache parses the message, extracting the URL and headers.
	3. Lookup--Cache checks if a local copy is available and, if not, fetches a copy(and stores it locally)
	4. Freshness check--Cache checks if cached copy is fresh enough and, if not, asks server for any updates.
	5. Response creation--Cache makes a response message with the new headers and caches body.
	6. Sending--Cache sends the response back to the client over the network
	7. Logging--Optionally, cache creates a log file entry describing the transaction.

![](https://cloud.githubusercontent.com/assets/3389862/9254553/b7c63170-4216-11e5-931d-46a51fab5f30.png)

###Cache Processing Flowchart
![](https://cloud.githubusercontent.com/assets/3389862/9254602/ffa95d96-4216-11e5-8359-486d7f22c87f.png)

###Document Expiration
HTTP lets an origin server attach an "expiration date" to each document, using special HTTP Cache-Control and Expires headers. These headers dictate how long content should be viewed as fresh.
Until a cache document expires, the cache can serve the copy as often as it wants, without ever contacting the server--unless, of course, a client request includes headers that prevent serving a cached or unvalidated resource. But, once the cached document expires, the cache must check with the server to ask if the document has changed and, if so, get a fresh copy (with a new expiration date).


###Expiration Dates and Ages
- Servers specify expiration dates using the HTTP/1.0+ Expires or the HTTP/1.1 Cache-Control: max-age response headers, which accompany a response body. The Expires and Cache-Control: max-age headers do basically the same thing, but the newer Cache-Control header is preferred, because it uses a relative time instead of an absolute date. Absolute dates depend on computer clocks being set correctly.

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
|If-Modified-Since:\<date\>| Perform the requested method if the document has been modified since the specified date. This is used in conjunction with the Last-Modified server response header, to fetch content only if the content has been modified from the cached version.|
|If-None-Match:\<tags\>|Instead of matching on last-modified date, the server may provide special tags on the document that act like serial numbers. The If-None-Match header Performs the requested method if the cached tags differ from the tags in the server's document|
