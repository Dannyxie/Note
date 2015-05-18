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
- When outer function is executed, it becomes the first object in the execution context's scope chain
