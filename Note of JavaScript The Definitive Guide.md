
##CHAPTER 18 Scripted HTTP

XMLHttpRuquest:
	
	1. The object supports any text based format, including XML.
	2. It can be used to ake requests over both HTTP and HTTPS (some implementations support protocols in addition to HTTP and HTTPS, but that functionality is not covered by this specification)
	3. It supports "requests" in a broad sense of the term as it pertains to HTTP; namely all activity involved with HTTP requests or responses for the defined HTTP methods.


- Comet require the client to establish ( and re-establish as necessary) a connection to the server, and require the server to keep that connection open so that it can send asynchronous messages over it


###18.1 Using XMLHttpRequest
- You can reuse an existing XMLHttpRequet object, but note that doing so will abort any request pending through that object.


```javascript
	//Emulate the XMLHttpRequest() constructor in IE5 and IE6
	if(window.XMLHttpRequest === undefined){
			window.XMLHttpRequest = function(){
					try{
						//Use the latest vesion of the ActiveX object if available
						return new ActiveXObject("Msxml2.XMLHTTP.6.0");
						}
					catch(e1){
							try{
								//Otherwise fall back on an older version
								return new ActiveXObject("Msxml2.XHLHTTP.3.0");
								}
							catch(e2){
									//Otherwise, throw an error
									throw new Error("XMLHttpRuest is not supported");
								}
						}
				};
			}
```

An HTTP request consists of four parts:

	1. the HTTP request method or "verb"
	2. the URL being requested
	3. an optional set of request headers, which may include authentication information
	4. an optional request body

The HTTP response sent by a server has three parts:

	1. a numeric and textual status code that indicates the success or failure of the request
	2. a set of respons headers
	3. the response body


###18.1.1 Specifying the Request
- open() method : specify the two required parts of the request, the method and the URL.
- "GET" is used for most "regular" requests, and it is appropriate when the URL completely specifies the requested resource, when the request has no side effects on the server, and when the server's response is cacheable.
- the "POST" method is what is typically used by HTML forms. It includes additional data (the form data) in the request body and that data is often stored in a database on the server (a side effect). Repeated POSTs to the same URL may result in different responses form the servver, and **requests that use this method should not be cached**.

- POST requests need a "Content-Type" header to specify the MIME type of the request body :  request.setRequestHeader("Content-Type", "text/plain")
- If you call setRequestHeader() multiple times for the same header, the new value does not replace the previously specified value: instead, the HTTP request will include multiple copies of the header or the header will specify multiple values.
- You cannot specify the "Content-Length", "Date", "Referer", or "User-Agent" headers yourself: XMLHttpRequest will add those automatically for you and will not allow you to spoof them. Similarly, XMLHttpRequest object automatically handles cookies, and connection lifetime, charset, and encoding negotiations, so you're not allowed to pass any of these headers to `setRequestHeader()`

- GET requests never have a body (`send(null)`). POST requests do generally have a body, and it should match the "Content-Type" header you specified with `setRequestHeader()`
- The parts of an HTTP request have a specific order: the request method and URL must come first, then the request headers, and finally the request body. XMLHttpRequest implementations generally do not initiate any networking until the `send()` method is called.


XMLHttpRequest readyState values:

| Constant 	| Value 	| Meaning 	|
| ----		| ----- 	| -----		|
| UNSENT 	| 0 		| open() has not been called yet |
| OPENED 	| 1 		| open() has been called |
| HEADERS_RECEIVED | 2 		| Headers have been received |
| LOADING   	| 3 		| The response body is being received |
| DONE 		| 4 		| The response is complete |

####18.1.2.1 Synchronous responses
-  If you pass `false` as the third argument to `open()`, the `send()` method will block until the request completes. 
