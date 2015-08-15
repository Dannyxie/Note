#High Performance JavaScript
##Chapter 1 Loading and Execution
- In fact, most browsers use a single process for both user interface(UI) updates and JavaScript execution ,so any one can happen at any given moment in time.

###Script Positioning
- Browsers don’t start rendering anything on the page until the opening  `<body>` tag is encountered.
- Putting scripts at the top of the page in this way typically leads to a noticeable delay, often in the form of a blank white page, before the user can even begin reading or otherwise interacting with the page.
- Internet Explorer 8, Firefox 3.5, Safari 4, and Chrome 2 all allow parallel downloads of JavaScript files.
- Unfortunately, JavaScript downloads still block downloading of other resources, such as images. Downloading a script doesn't block other scripts from downloading. The page must still wait for the JavaScript code to be downloaded and executed before continuing.
- Because scripts block downloading of all resource types on the page, it’s recommended to place all  `<script>` tags as close to the bottom of the  `<body>` tag as possible so as not to affect the download of the entire page : put scripts at the bottom.

###Grouping Scripts
- Since each `<script>` tag blocks the page from rendering during intial download, it's helpful to limit the total number of `<script>` tags contained in the page. This applied to both inline script as well as those in external files. Everytime a `<script>` tag is encountered during the parsing of a HTML page, there is going to a delay while the code is executed.
- an inline script placed after a  `<link>` tag referencing an external stylesheet caused the browser to block while waiting for the stylesheet to download. Souders recommends never putting an inline script after a `<link>` tag for this reason.
- it’s helpful to limit the number of external script files that your page references.You can minimize the performance impact by concatenating several Javascript files together into a single file and then calling that single file with a single  `<script>` tag.

###Deferred Scripts
- The  defer attribute indicates that the script contained within the element is not going to modify the DOM and therefore execution can be safely deferred until a later point in time. (supported only in Internet Explorer 4+ and Firefox 3.5+). In other browsers, the  defer attribute is simply ignored and so the  `<script>` tag is treated in the default (blocking) manner. usage: `<script type="text/javascript" src="file1.js" defer> </script>`
- defer:The JavaScript file will begin downloading at the point that the  `<script>` tag is parsed, but the code will not be executed until the DOM has been completely loaded (before the  onload event handler is called). When a deferred JavaSript file is downloaded, it doesn't block the browser's other process, and so these files can be downloaded in parallel with others on the page.
- Any  `<script>` element marked with  `defer` will not execute until after the DOM has been completely loaded;

###Dynamic Script Elements
- When a file is downloaded using a dynamic script node,it is downloaded and executed without blocking other page processes,
```javascript
var script = document.createElement('script');
script.type = 'text/javascript';
script.src = '';
document.getElementsByTagName('head')[0].appendChild(script);
```
- regardless of where the download is initiated ,and the retrieved code is typically executed immediately (except in Firefox and Opera, which will wait until any previous dynamic script nodes have executed).This works well when the script is self-executing but can be problematic if the code contains only interfaces to be used by other scripts on the page.
- Firefox, Opera, Chrome, and Safari 3+ all fire a `load` event when the `scr` of a `<script>` element has been retrieved. (added: `onload` will be triggered when the script has been loaded and executed.)
- IE supports an alternative implemention that fires a `readystatechage` event. There is a `readyState` property on the `script` element that is changed at various times during the download of an external file. 5 possible values:
1. "uninitialized"	The default state
2. "loading"	Download has begun
3. "loaded"	Download has completed
4. "interactive"	Data is completely downloaded but isn't fully available
5. "complete"	All data is ready to be used.
```javascript
function loadScript(url, callback){
	var script = document.createElement('script');
	script.type = 'text/javascript';

	if(script.readyState){	//IE
		script.onreadystatechange = function(){
			if(script.readyState == "loaded" || script.readyState == "complete"){
				scipt.onreadystatechange = null;
				callback();
			}
		};

	} else {
		script.onload = function(){
			callback();
		};
	}

	script.src = url;
	document.getElementsByTagName("head")[0].appendChild(script);
}
```

###XMLHttpRequest Script Injection
```javascript
	var xhr = XMLHttpRequest();
	xhr.open("get", fileURL, true);
	xhr.onreadystatechange = function(){
		if(xhr.readyState ==4 ){
			if(xhr.status >= 200 && xhr.status < 300 || xhr.status == 304 ){
				var script = document.createElement("script");
				script.type = "text/javascript";
				script.text = xhr.responseText;
				document.body.appendChild(script);
			}
		}
	};
	xhr.send(null);
```
- The  onreadystatechange event handler checks for a  readyState of 4 and then verifies that the HTTP status code is valid (anything in the 200 range means a valid response, and 304 means a cached response).
- Primary advantage: the JavaScript code can be download without exectuing immmediately.
- Limitation: the JavaScript file must be located on the same domain as the page requesting it.
```javascript
function loadScript(url, callback){
	var script = document.createElement("script") script.type = "text/javascript";
	if (script.readyState){ //IE script.onreadystatechange = function(){
		if (script.readyState == "loaded" || script.readyState == "complete"){
			script.onreadystatechange = null;
			callback(); }
	};
	} else { //Others
		script.onload = function(){
			callback(); };
	}
	script.src = url;
	document.getElementsByTagName("head")[0].appendChild(script); }
	loadScript("the-rest.js", function(){
			Application.init(); });
```
##Summary
- Put all `<script>` tags at the bottom of the page, just inside of the closing `</body>` tag. This ensures that the page can be almost completely rendered before script execution begins.
- Group scripts together. The fewer `<script>` tags on the page, the faster the page can be loaded and become interactive. This holds true both for `<script>` tags loading external JavaScript files and those with inline code.
- There are several ways to download JavaScript in a nonblocking fashion:
	1. Use the `defer` attribute of the `<script>` tag (Internet Explorer and Firefox 3.5+ only)
	2. Dynamiclly create `script` elements to download and execute the code
	3. Download the JavaScript code using an XHR object, and then inject the code into the page.

##Chapter 2:Data Access

###Scope Chains and Identifier Resolution
- Every function in JavaScript is represented as an object -- more specifically, as an instance of `Function`.
- The internal `[[scope]]` property contains a collection of objects representing the scope in which the function was created. This collection is called the function's `scope chain` and it determines the data that a function can access. Each object in the function's scope chain is called a `variable object`, and each of these contains entires for variables in the form of key-value pairs. When a function is created, its scope chain is populated with objects representing the data that is accessible in the scope in which the function was created.
- An execution context defines the environment in which a function is being executed. Each execution context is unique to one particular execution of the function, and so multiple calls to the same function result in multiple execution contexts being created. The execution context is destroyed once the function has been completely executed.
- An execution context has its own scope chain that is used for indetifier resolution. When the execution context is created, its scope chian is initialized with the objects contained in the execution function's ``[[scope]]`` property. These values are copied over into the execution context scope chain in the order in which they appear in the function. Once this is complete, a new object called the `activation object` is created for the execution context. The activation object acts as the variable object for this execution and contains entries for all local variables, named arguments, the `arguments` collection, and  `this`. This object is then pushed to the front of the scope chain. When the execution context is destroyed, so is the activation object.  

###Indetifier Resolution Performance
- Local vriables are always the fastest to access inside of a function, whereas global variables will generally be the slowest. Global variables always exist in the last variable object of the execution context's scope chain, so they are always the furthest away to resolve.
- It's advisable to use local variables whenever possible to improve performance in browsers without optimizing JavaScript engines. A good rule of thumb is to always store out-of-scope values in local variables if they are used more than once within a function.

###Scope Chain Augmentation
- The `with` statement is used to create variables for all of an object's properties.
- Problem: When code execution flows into a `with` statement, the execution context's scope chain is temporarily augmented. A new varable object is created containing al of the properties of the specified object. That object is then pushed to the front of the scope chain, meaning that all of the function's local variables are now in the second scope chain object and are therefore more expensive to access.
- The `catch` clause of the `try-catch` statement has the same effect. When an error occurs in the `try` block, execution automatically flows to the `catch` and the exception object is pushed into a variable object that is then palced at the front of the scope chain. Inside of the `catch` block, all variables local to the function are now in the second scope chain object. Note that as soon as the `catch` clause is finished executing, the scope chian returns to its previous state.
- We can minimize the performance impact of the `catch` clause by executing as little code as necessary within it. A good pattern is to have a method for handling errors that the `catch` clause can delegate to.
```javascript
try{
		methodThatMightCauseAnError();
		} catch (ex){
				handleError(ex);
				}
```


###Dynamic Scopes
- Both the `with` statement and the `catch` clause of a `try-catch` statement, as well as a function containing `eval()`, are all considered to be *dynamic scope*. A dynamic scope is one that exists only through execution of code and therefore cannot be determined simply by static analysis(looking at the code structure);


###Closures, Scope, and Memory
- Closures allow a function to access data that is outside of its local scope.
- When outer function is executed, it becomes the first object in the execution context's scope chain, with the global object coming second. When the closure is created, its `[[scope]]`` property is initialized with both of these objects.
- Since the closure's `[[scope]]` property contain references to the same object as the excution context's scope chain, there is a side effect. Typically, a function's activation object is destroyed when the execution context is destroyed. When there's a closure involved, though, the activation object isn't destroyed, because a referenct still exists in the closure's `[[Scope]]` property. This means that closures require more memory overhead in a script that a nonclosure function. Closures can cause memory leaks.
- When the closure is executed, an execution context is created whose scope chain is initialized with the same two scope chain objects referenced in `[[Scope]]`, and then a new activation object is created for the closure itself.
- you're often accessing a lot of out-of-scope identifiers and therefore are incurring a performace penalty with each access.
- advice: store any frequently used out-of-scope variables in local variables, and the access the local variables directly.

###Object Members
- When a named member referenets a function, it's considered a method, whereas a member referencing a nonfunction data type is considered a property.
- obejct member access trends to be slower than accesing data in literal or variables and in some browsers slower than accessing array items.

###Prototypes
- Prototype objects are shared amongst all instances of a given object type, and so all instances also share the prototype object's members.
- An object is tied to its prototype by an internal property. Firefox, Safari, and Chrome expose this property to developers as `__proto__`; other browsers do not allow script acces to this property.
- `hasOwnProperty()`: determine whether an object has an instance member with a given name.
- `in` : determine whether an object has access to a property with a given name.

###Prototype Chains
- The process of looking up an instance member is still more expensive than accessing data from a literal or a local variable, so adding more overhead to traverse the prototype chain just amplifies this effect.

###Nested Members
- the deeper the nested member, the slower the data is accessed.

###Caching Object Member values
- If you're going to read an object property more than one time in a function, it's best to stire that property value in a local variable. The local variable can then be used in place of the property to avoid the performance overhead of another property lookup.
- Many object methods use `this` to determine the context in which they are being called, and storing a method in a local variable causes `this` to be bound to `window`.

###Summary
- places to access data: literal value, variable, array items, and object members.

1. Leteral values and local variables can be accessed very quickly, whereas array items and object memebers take longer.
2. Local variables are faster to access than out-of-scope variables bacause they exist in the first variable object of the scope chain. The further into the scope chain a varible is, the longer it takes to access. Global variables are always the slowest to access because they are always last in the scope chain.
3. Avoid the `with` statement because it augments the execution context scope chain. Also, be careful with the `catch` clause of a `try-catch` statement because it has the same effect.
4. Nested object members incur significant performance impact and should be minimized.
5. The deeper into the prototype chain that a property or method exists, the slower it is to access.
6. Generally speaking, you can improve the performance of JavaScript code by storing frequently used object members, array items, and out-of-scope variables in local variables. You can then access the local variables faster than the originals.


##Chapter 3 DOM Scripting
###DOM in the Browser World
-  The Document Object Model(DOM) is a language-independent application interface(API) for working with XML and HTML document.
- DOM is a language-independent API, in the browser the interface is implemented in JavaScript.

###DOM Access and Modification
- Modifying elements is even more expensive because it often causes the browser to recalculate changes in the page geometry.

###innerHTML Versus DOM methods
- `innerHTML` is faster in all browsers except the latest WebKit-based ones(Chrome and Safari)
- Using an array to concatenate large strings will make `innerHTML` even faster in those browsers.
- Using `innerHTML` will give faster execution in most browsers in performance-critical operations that require updating a large part of the HTML page. But we should consider readability, maintenance, team preferences, and coding conventions when deciding on our approach.

###Cloning Nodes
- Clone existing DOM elements: `element.cloneNode()` (where element is an existing node)
```javascript
function tableClonedDOM() {
	var i, table, thead, tbody, tr, th, td, a, ul, li, oth = document.createElement('th'),
	    otd = document.createElement('td'),
	    otr = document.createElement('tr'),
	    oa = document.createElement('a'), oli = document.createElement('li'),
	    oul = document.createElement('ul'); tbody = document.createElement('tbody'); for (i = 1; i <= 1000; i++) {
		    tr = otr.cloneNode(false);
		    td = otd.cloneNode(false); td.appendChild(document.createTextNode((i % 2) ? 'yes' : 'no')); tr.appendChild(td);
		    td = otd.cloneNode(false); td.appendChild(document.createTextNode(i));
		    tr.appendChild(td);
		    td = otd.cloneNode(false); td.appendChild(document.createTextNode('my name is #' + i)); tr.appendChild(td);
		    // ... the rest of the loop ... }
		    // ... the rest of the table generation ... }
```

###HTML Collections
- `HTMLCollection` object, which are array-like lists, don't have methods such as push() or slice(), but provide a `length` property and allow indexed access to the elements in list.They are automatically updated when the underlying document is updated.

###Expensive collections
-  Accessing a collection's `length` is even slower than accessing a regular array's `length` because it means rerunning the query every time.

###Local variables when accessing collection elements
- In general, for any type of DOM access it's best to use a local variable when the same DOM property or method is accessed more than once. When looping over a collection, the first optimization is to store the collection in a local variable and cache the `length` outside the loop, and then use a local variable inside the loop for element that are accessed more than once.

###Waling the DOM

####Crawling the DOM
- `childNodes`, `nextSibling`
- In IE, `nextSibling` performs much better than `childNodes`

####Element nodes
- DOM properties such as `childNodes`,`firstChild`, and `nextSibling` don't distinguish between element nodes and other node types, such as comments and text nodes(which are often just spaces between two tags)

DOM properties that distinguish element nodes (HTML tags) versus all nodes

| Property	| Use as a replacement for|
|---------------|:-----------------------:|
|children	|childNodes	|
|childElementCount|childNodes.length|
|firstElementChild|firstChild|
|lastElementChild|lastChild|
|nextElementSibling|nextSibling|
|previousElementSibling|previousSibling|

All of the properties listed above are supported as of Firefox 3.5, Safari 4, Chrome 2, and Opera 9.62. IE 6,7 and 8 only support `children`.

- Whitespaces in the HTML source code are actually text nodes, and they are not included in the `children` collection.

####The Selectors API
- `querySelectorAll`: takes a CSS selector string as an argument and returns a `NodeList`-- an array-like object containing matching nodes. The method doesn't return an HTML collection, so the returned nodes do not represent the live structure of the document.
- `querySelector`: returns only the first node matched by the query.

###Repaints and Reflows
- Once the browser has downloaded all the components of a page--HTML markup, JavaScript, CSS, images--it parses through the files and creates two internal data structures:
	1. A DOM tree: a representation of the page structure
	2. A render tree: a representation of how the DOM nodes will be displayed

- The render tree has at least one node for every node of the DOM tree that needs to be displayed( hidden DOM elements don't have a corresponding node in the render tree).
- Nodes in the render tree are called `frames` or `boxes` in accordance with the CSS model that treats page elements as boxes with padding, margins, borders, and position.
- When a DOM change affects the geometry of an element (width or and height) -- such as a change in the trickness of the border or adding more text to a paragraph, resulting in an additional line -- the browser needs to recalculate the geometry of the element as well as the geometry and position of other elements that could have been affected by the change. The browser invalidates the part of the render tree that was affected by the change and reconstructs the render tree. This process is known as a `reflow`. Once the reflow is complete, the browser redraws the affected parts of the screen in a process called `repaint`.

Reflow happen when:
	1. Visible DOM elements are added or removed
	2. Element change position
	3. Element change size ( because of a change in margin, padding, border thickness, width, height, etc);
	4. Content is changed, e.g., text changes or an image is replaced with one of a different size.
	5. Page renders initially
	6. Browser window is resized.
	7. Scolling window may cause a reflow of the whole page.

###Queuing and Flushing Render Tree Changes
- Because of the computation costs associated with each reflow, most browsers optimize the reflow process by queuing changes and performing them in batches. Flusing the queue happens when you want to retrieve layou information, which means using any of the following:
	- `offsetTop`,`offsetLeft`,`offsetWidth`,`offsetHeight`
	- `scrollTop`,`scrollLeft`,`scrollWidth`,`scrollHeight`
	- `clientTop`,`clientLeft`,`clientWidth`,`clientHeight`
	- `getComputedStyle()`(`currentStyle` in IE)

- During the process of changing styles, it's best not to use any of the properties shown in the preceding list.

###Minimizing Repaints and Reflows
-  combine multiple DOM and style changes into a batch and apply them once.

####Style changes
- combine all the changes and apply them at once, modifying the DOM only once. This can be done using the `cssText` property
- Another way to apply style changes only once is to change the CSS class name instead of changing the inline styles.

####Batching DOM change
reduce the number of repaints and reflows:
	1. Take the element off of the document flow.(causes reflow)
	2. Apply multiple changes.
	3. Bring the element back to the document.(causes reflow)

Ways to modify the DOM off the document:
	- Hide the element, apply changes, and show it again.
	- Use a document fragment to build a subtree outside of the live DOM and then copy it to the document.
	- Copy the original element into an off-document node, modify the copy, and then replace the original element once you're done

- A document fragment is a lightweight version of the `document` object, and it's designed to help with exactly this type of task--updating and moving nodes around.

###Caching Layout Infomation

###Take Elements Out of the Flow for Animations
- The less the browser needs to reflow, the more responsive your application will be. The more nodes in the render tree that need recalculation, the worse it becomes.

Avoid reflow of a big part of the page:
	1. Use absolute positioning for the element you want to animate on the page, taking it out of the layout flow of the page.
	2. Animate the element. When it expands, it will temporarily cover part of the page. This is a repaint, but only of a small part of the page instead of a reflow and repaint, but only of a small part of the page instead of a reflow and repaint of a big page chunk.
	3. When the animation is done, restore the positioning, thereby pushing down the rest of the document only once.

###IE and :hover
- If you have significant number of elements with `a:hover`, the responsiveness degrades.

###Event Delegation
- Event delegation based on the fact that events bubble up and can be handled by a parent element. With event delegation, we attach only one handler on a wrapper element to handle all events that happen to the children descendant of that parent wrapper.
- Event phases : `Capturing`, `At target`, `Bubbling`
- Capturing is not supported by IE.

###Summary
- Minimize DOM access, and try to work as much as possible in JavaScript.
- Use local variables to store DOM references you'll access repeatedly
- Be careful when dealing with HTML collections because they represent the live, underlying document. Cache the collection `length` into a variable and use it when iterating, and make a copy of the collection into an array for heavy work on collections.
- Use faster APIs when available, such as `querySelectorAll()` and `firstElementChild`
- Be mindful of repaints and reflows; batch style changes, manipulate the DOM tree "offline," and the cache and minimize access to layout infomation.
- Position absolutely during animations, and use drag and drop proxies.
- Use event delegation to minimize the number of event handlers.


##Chapter 4 Algorithms and Flow Control
- Placing a `var` statement in the initialization part of a `for` loop creates a function-level variable, not a loop-level one. JavaScript has only function-level scope, and so defining a new variable inside of a `for` loop is the same as defining a new function outside of the loop.

###Loop Performance
- the `for-in` loop has considerably more overhead per iteration and is therefore slower than the other loops.
- We should never use `for-in` to iterate over members of an array.

##Chapter 5 Strings and Regular Expressions
- Avoid using temporary string

###Array Joining
- The `Array.prototype.join` method merges all elements of an array into a string and accepts a separator string to insert between each element.

###String.prototype.concat
- The native string `concat` method accepts any number of arguments and appends each to the string that the method is called on.
- `concat` is a little slower that simle `+` and `+=` operators in most cases, and can be substantially slower in IE, Opera, and Chrome.

###Summary
- When concatenating numerous or large strings, array joining is the only method with reasonable performance in IE7 and earlier.
- If you don't need to worry about IE7 and earlier, array joining is one of the slowest ways to concatenate strings. Use simple `+` and `+=` operators instead, and avoid unnecessary intermediate strings
- Backtracking is both a fundamental component of regex matching and a frequent source of regex inefficiency.
- Runaway backtracking can cause a regex that usually finds matches quickly to run slowly or even crash your browser when applied to partially matching strings. Techniques for avoiding this problem include making adjacent tokens mutually exclusive, avoiding nested quantifiers that allow matching the same part of a string more that one way, and eliminating needless backtracking by repurposing the atomic nature of lookahead.
- A variety of techniques exist for improving regex efficency by helping regexes find matches faster and spend less time considering nonmatching positions.
- Regexes are not always the best tool for the job, especially when you are merely searching for literal strings.
- Although there are many ways to trim a string, using two simple regexes(one to remove leading whitespace and another for trailing whitespace) offers a good mix of brevity and cross-browser efficiency with varing string contents and lengths. Looping from the end of the string in search of the first nonwhitespace charcaters, or combining this technique with regexes in a hybrid approach, offers a good alternative that is less affected by overall string length.


##Chapter 6 Responsive Interfaces
- Most browsers have a single process that is shared beteewn JavaScript execution and user interface updates. Only one of these operations can bue performed at a time, meaning that the user interface cannot respond to input while JavaScript code is executd and vice versa. The user interface effectively becomes "locked" when JavaScript is executing;

###Browser Limits
- Two limits: `call stack size limit`, `long-running script limit`
- There are two ways of measuring how long a script is executing.
	- The first is to keep stack of how many statements have been executed since the script began. This approach means that the script may run for different periods of time on different machines, as the available memory and CPU speed can affect how long it takes to execute a single statement.
	- The second approach is to track the total amount of time that the script has been executing. The amount of script that can be processed within a set amount of time also varies based on the user's machine capabilities, but the script is always stopped after a set amount of times. Each browser has a slightly different approach to long-running script detection.

###Web Workers

###Worker enviroment

The worker environment is made up of the following:

	- A `navigator` object: contains only four properties:`appName`,`appVersion`,`userAgent`,`paltform`
	- A `location` object: same as on `window`,except all properties are read-only.
	- A `self` object: points to the global worker obect
	- An `importScripts()` menthod that is used to load external JavaScript for use in the worker
	- All ECMAScript object, such as `Object`,`Array`,`Date`,etc.
	- The `XMLHttpRequest` constructor
	- The `setTimeout()` and `setInterval()` methods
	- A `close()` method that stops the worker immediately


- Because Web workers have a different global environment, you can't create one from any JavaScript code.

###Summary
- No JavaScript task should take longer than 100 milliseconds to execute. Longer execution times cause a noticeable delay in updates to the UI and negatively impact the overall user experience.
- Browsers behave differently in response to user interaction during JavaScript execution. Regardless of the behavior, the user exeperience becomes confusing and disjointed when JavaScript takes a long time to execute.
- Timers can be used to schedule code for later execution, which allows you to split up long-running scripts in to a series of smaller tasks.
- Web workers are a feature in newer browsers that allow you to execute JavaScript code outside of the UI thread, thus preventing UI locking.


- The more comlex the web application, the more critical it is to manage the UI thread in a proactive manner. No JavaScript code is so important that is should adversely affect the user's experience.

##Chapter 7 Ajax

Five general technuques for requesting data from a server:
	- XMLHttpRequest(XHR)
	- Dynamic script tag insertion
	- iframes
	- Comet
	- Multipart XHR

###XMLHttpRequest
- A `readyState of 4 indicates that the entire response has been received and is available for manipulation.
- It is possible to interact with the server response as it is still being transferred by listening for `readyState` 3.
- You cannot use XHR to request data from a domain different from the one the code is currently running under, and older versions of IE do not give you access to `readyState 3`, which prevents streaming. Data that comes back from the request is treated as either a string or an XML object.

####POST versus GET when using XHR
When using XHR to request data, you have a choice between using POST or GET. For requests that don't change the server state and only pull back data(this is called an idempotent action), use GET. GET requests are cached, which can improve performance if you're fetching the same data several times.
POST should be used to fetch data only when the length of the URL and the parameters are close to or exceed 2048 characters. This is because Internet Explorer limits URLs to that length, and exceeding it will cause you request to be truncated.

###Dynamic script tag insertion
- can request data from a server on a different domain.
```javascript
var scriptElement = document.createElement('script');
scriptElement.src = yourSrc;
document.getElementsByTagName('head')[0].appendChild(scriptElement);
```
- dynamic script tag insertion can't send hearder with the request. Parameters can only be passed using GET, not POST. Can't set timeouts or retry the request.
- Because the response is being used as the source for a script tag, it must be executable JavaScript. We cannot use bare XML, or even bare JSON; any data, regardless of the format, must be enclosed in a callback function.

###Multipart XHR
- Multipart XHR(MXHR) allows you to pass multiple resources from the server side to the client side using only on HTTP request.
