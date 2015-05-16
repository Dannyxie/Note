#High Performance JavaScript
##Chapter 1 Loading and Execution
- In fact, most browsers use a single process for both user interface(UI) updates and JavaScript execution ,so any one can happen at any given moment in time.

###Script Positioning
- Browsers don’t start rendering anything on the page until the opening  <body> tag is encountered. 
- Putting scripts at the top of the page in this way typically leads to a noticeable delay, often in the form of a blank white page, before the user can even begin reading or otherwise interacting with the page.
- Internet Explorer 8, Firefox 3.5, Safari 4, and Chrome 2 all allow parallel downloads of JavaScript files. 
- Unfortunately, JavaScript downloads still block downloading of other resources, such as images. Downloading a script doesn't block other scripts from downloading. The page must still wait for the JavaScript code to be downloaded and executed before continuing. 
- Because scripts block downloading of all resource types on the page, it’s recommended to place all  <script> tags as close to the bottom of the  <body> tag as possible so as not to affect the download of the entire page : put scripts at the bottom.

###Grouping Scripts
- Since each `<script>` tag blocks the page from rendering during intial download, it's helpful to limit the total number of `<script>` tags contained in the page. This applied to both inline script as well as those in external files. Everytime a `<script>` tag is encountered during the parsing of a HTML page, there is going to a delay while the code is executed.
- an inline script placed after a  <link> tag referencing an external stylesheet caused the browser to block while waiting for the stylesheet to download. Souders recommends never putting an inline script after a <link> tag for this reason.
- it’s helpful to limit the number of external script files that your page references.You can minimize the performance impact by concatenating several Javascript files together into a single file and then calling that single file with a single  <script> tag. 

###Deferred Scripts
- The  defer attribute indicates that the script contained within the element is not going to modify the DOM and therefore execution can be safely deferred until a later point in time. (supported only in Internet Explorer 4+ and Firefox 3.5+). In other browsers, the  defer attribute is simply ignored and so the  <script> tag is treated in the default (blocking) manner. 
- defer:The JavaScript file will begin downloading at the point that the  <script> tag is parsed, but the code will not be executed until the DOM has been completely loaded (before the  onload event handler is called).
- Any  <script> element marked with  defer will not execute until after the DOM has been completely loaded; 

###Dynamic Script Elements
- When a file is downloaded using a dynamic script node,it is downloaded and executed without blocking other page processes,
- regardless of where the download is initiated ,and the retrieved code is typically executed immediately (except in Firefox and Opera, which will wait until any previous dynamic script nodes have executed).This works well when the script is self-executing but can be problematic if the code contains only interfaces to be used by other scripts on the page.

###XMLHttpRequest Script Injection
- The  onreadystatechange event handler checks for a  readyState of 4 and then verifies that the HTTP status code is valid (anything in the 200 range means a valid response, and 304 means a cached response). 

##Chapter 2: Data Access

- Scope Chains and Identifier Resolution
- An execution context defines the environment in which a function is being executed. Each execution context is unique to one particular execution of the function, and so multiple calls to the same function result in multiple execution contexts being created. The execution context is destroyed once the function has been completely executed.

