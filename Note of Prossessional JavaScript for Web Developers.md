##Chapter 5 Reference Types##
- New objects are created by using the **new** operator follow by a *constructor*.

####The Object Type####
- Numeric property name are automatically converted      to string.

####The Array Type####
providing a single argument that is a number always creates an array with the given number of items, whereas an argument of any other type creates a one-item array that contains specific value:

	var colors = new Array(3); //create an array with three items
	var names = new Array(“Greg”); //create an array with one item, the string “Greg”

we can also do this:
	
	var colors = new Array(“red”, “blue”, “green”);

the **new** operator can be omitted when using the  Array constructor:

	var colors = Array(3); //create an array with three items
	var names = Array(“Greg”); //create an array with one item, the string “Greg”

- **length** is not read-only. Buy setting the **length** property, you can easily remove items from or add items to the end of the array.

####Detecting Array####
**Array.isArray()** (Internet Explorer 9+, Firefox 4+, Safari 5+, Opera 10.5+, and Chrome)

####Conversion Method####
- **toLocaleString()**
- **toString()**
- **valueOf()**
- **join()**

####Stack Methods####
- The **push()** method accepts *any number of arguments* and adds them to the end of the array

####Queue Methods####
- shift(): removes the first item in the array and returns it, decrementing the length of array by one.
- unshift(): adds any number of items to the front of any array and returns the new array length (Internet Explorer 7 and earlier always return  **undefined** , instead of the new length of the array, for  unshift() . Internet Explorer 8 returns the length correctly when not in compatibility mode.)

####Reordering Methods####
- **reverse()** simply reverses the order of items in any array.
- sort() method allows you to pass in a *comparision function* that indicates which value should come before which.

A comparision function accepts two arguments and returns a negative number if the first argument should come before the second, a zero if the arguments are equal, or a positive number if the first argument should come after the second

	function compare(value1, value2){
		return value2 - value1;
	}

####Manipulation Methods####
concat() When no arguments are passed in, concat() simply clones the array and returns it. If one or more arrays are passed in, contat() appends each item in the arrays to the end of the result. If the values are not arrays, they are simply appended to the end of the resulting array. 

	var colors = [“red”, “green”, “blue”];
	var colors2 = colors.concat(“yellow”, [“black”, “brown”]);
	alert(colors); //red,green,blue
	alert(colors2); //red,green,blue,yellow,black,brown

slice(): creates an array that contains one or more items already contained in an array. It accepts two arguments: the starting and stopping positions of items to return. if only one arguments, the method returns all items between that position and the end of the array.(**This operation does not affect the original array in any way**)

	var colors = [“red”, “green”, “blue”, “yellow”, “purple”];
	var colors2 = colors.slice(1);
	var colors3 = colors.slice(1,4);
	alert(colors2); //green,blue,yellow,purple
	alert(colors3); //green,blue,yellow

If either the start or end position of  slice() is a negative number, then the number is subtracted from the length of the array to determine the appropriate locations. For example, calling  slice(-2, -1) on an array with fi ve items is the same as calling  slice(3, 4) . If the end position is smaller than the start, then an empty array is returned.

splice() 

- Deletion — Any number of items can be deleted from the array by specifying just two arguments: the position of the fi rst item to delete and the number of items to delete. For example,  splice(0, 2) deletes the fi rst two items.
- Insertion — Items can be inserted into a specifi c position by providing three or more arguments: the starting position, 0 (the number of items to delete), and the item to insert. Optionally, you can specify a fourth parameter, fifth parameter, or any number of other parameters to insert. For example,  splice(2, 0, “red”, “green”) inserts the strings
“red” and  “green” into the array at position 2.
- Replacement — Items can be inserted into a specifi c position while simultaneously
deleting items, if you specify three arguments: the starting position, the number of items
to delete, and any number of items to insert. The number of items to insert doesn’t have to
match the number of items to delete. For example,  splice(2, 1, “red”, “green”)
deletes one item at position 2 and then inserts the strings  “red” and  “green” into the array
at position 2


##Chapter 6 OBJECT-ORIENTED PROGRAMMING##
###Object Creation###
####the constructor pattern####


- Object created by the constructor function that using **new** key will automatically have a **constructor** property(non-enumerable) which points to the constructor

------------
####How prototype work####
- Whenever a function is created,its **prototype** property is also created. The **prototype** has a **constructor** property that points back to the function
- *Each time the constructor is called to create a new instance, that instance has a internal pointer to the constructor's prototype.In ECMA-262 fifth edition, this is called [[Prototype]]. There is no standard way to access [[Prototype]] from script, but Firefox, Safari and Chrome all support a property on every object called **__proto__**.; in other implementations, this property is completely hidden from script. **The important thing to understand is that a direct link exists between the instance and the constructor's prototype but not between the instance and the constructor.***
![relationship between prototype, constructor function and instance](http://i.imgur.com/O2U1RAr.png)

-----------------
**Object.defineProperties()**:

	Object.defineProperties(obj, {
		"property1": {
    					value: true,
     					writable: true},
		"property2": {
    					value: "Hello",
    					writable: false  
						}
    // etc. etc.
    });
**isPrototypeOf()** method used to determine if [[Prototype]] points to the constructor's prototype

    alert(Person.prototype.isPrototypeOf(person1)); //true
	alert(Person.prototype.isPrototypeOf(person2)); //true

------------
 ECMAScript 5 adds a method called **Object.getPrototypeOf()**, which returns the value of [[Prototype]].*This method is supported in Internet Explorer 9+,Firefox 3.5+, Safari 5+, Opera 12+, and Chrome.*

	alert(Object.getPrototypeOf(person1) == Person.prototype); //true
	alert(Object.getPrototypeOf(person1).name); //”Nicholas”

---------------

- It's possible to read values on the prototype from object instance but it is not possible to overwrite them. If you add a property that has the same name  as a property on the prototype, the new property just masks the property on the prototype. We can use **delete** operator to remove the property on the instance so that the property on the prototype that with the same name can be access again.
- the **hasOwnProperty** method used to determine if a property exists on the instance or the prototype(inherited from Object)
- The ECMAScript 5 **Object.getOwnPropertyDescriptor()** works on own properties.

####Prototypes and the *in* operator####
- the **in** operator return true when the property of given name can be access from the object(*no matter it is on the object itself or on the prototype*).
- The **for-in** returns all properties that are accessible and enumerable by the object (include properties on the prototype)

--------------
- ECMAScript 5 **Object.keys()** method:accepts as its argument and returns an array of strings containing the names of all and enumerable properties.
- **Object.getOwnPropertyNames()**returns all instance properties, whether enumerable or not
- (*methods are supported in Internet Explorer 9+, Firefox 4+, Safari 5+, Opera 12+, and Chrome.*)

-----------
####Alternate Prototype Syntax####

	function Person() {}
	Person.prototype = {
	    name: “Nicholas”,
	    age: 29,
	    job: “Software Engineer”,
	    sayName: function() {
	        alert(this.name);
	    }
	};
	
	var friend = new Person();
	alert(friend instanceof Object); //true
	alert(friend instanceof Person); //true   **confusing here! **
	alert(friend.constructor == Person); //false
	alert(friend.constructor == Object); //true

----------------
####Dynamic Nature of Prototype####
- overwriting the prototype on the constructor means that new instance will reference the new prototype while any previously existing object instances still reference the old prototype.


####Native Object Prototype####


####Default Prototypes####
- The default prototype of any function is an instance of **Object**, meaning that its internal prototype pointer points to **Object.prototype**.

####Prototype and Instance Relationships####
- the **instanceof** operator: return **true** whenever an instance is used with a constructor that appears in its prototype chain.
- **isPrototypeOf**:returns **true** for an instance in the prototype chain

---------------


##Function Expression##
- Firefox, Safari, Chrome, and Opera all feature a nonstandard **name** property on function exposing the assigned name.
- Function expressions must be assigned before usage.
- **arguments.callee** is a pointer to the function being executed and, as such, can be used to call the function recursively.

###Closures###
- **closures** are functions that have access to variables from other function's scope. 
- When a function is called, an execution context is created, and its scope chain is created. The activation object for the function is initialized with the values for **arguments** and any named arguments. **The outer function's activation object is the second object in the scope chain.** This process continues for all containing functions until the scope chain terminates with the global execution context. 



####Closures and Variables####
- The closure always get the last value of any varable from the containing function.

####The **this** object####
- when used inside global functions, ***this*** is equal to **window** in nonstrict mode and **undefined** in strict mode, whereas **this** is equal to the object when called as an object method.
- Anonymous functions are not bound to an object in

####Memory leaks####


####Mimicking block scope####
- Javascript has no concept of block-level scoping, meaning variables defined inside of statement are actually created in the containing  function, not within the statement.


JavaScript will never tell you if you’ve declared the same variable more than once; it simply ignores all subsequent declarations (though it will honor initializations).  

	function outputNumbers(count) {
        for (var i = 0; i < count; i++) {
                alert(i);
        }
        var i; //variable redeclared
        alert(i); //count
	}

The basic syntax of an anonymous function used as a block scope (often called a private scope)

	(function(){
	//block code here
	})();


	
####Private variables####
- Any variable defined inside a function is considered private since it is inaccessable outside that funciton. This includes function arguments, local variables, and functions defined inside other functions.



####Static Private Variables####
Initilizing an undeclare variable always creates a global variable.(assigning a undeclare varialbe in strict mode causes an error)

	(function() {
	        //private variables and functions
	        var privateVariable = 10;
	
	        function privateFunction() {
	                        return false;
	                }
	                //constructor
	        MyObject = function() {};
	        //public and privileged methods
	        MyObject.prototype.publicMethod = function() {
	                privateVariable++;
	                return privateFunction();
	        };
	})();




####The Module Pattern####



####The Module-Augmentation Pattern####



###Summary###
Function:

- Function expressions are different from function declarations. Function declarations require name, while function expressions do not. A function expressing without name is also called anonynmous function.
- Recursive functions in nonstrict mode may use **arguments.callee** to call themselves.

Closures:

- Behind the scenes, the closure's scope chain contains a variable object for itself, the containing function, and the global context.
- Typically a function's scope and all of its variables are destroyed when the function has finished executing.
- When a closure is returned form that function, its scope remains in the momery until the closure no longer exists


mimic block scoping in JavaScript:

- A function can be create and called immediately, executing the code within it but never leaving a reference to that function.
- This results in all of the variables inside the function being destroyed unless they are specifi cally set to a variable in the containing scope.


create private variables in objects:

- Even though JavaScript doesn’t have a formal concept of private object properties, closures can be used to implement public methods that have access to variables defi ned within the containing scope.
- Public methods that have access to private variables are called privileged methods.
- Privileged methods can be implemented on custom types using the  constructor or prototype patterns and on singletons by using the module or module-augmentation patterns.




-------------
##The Browser Object Model##
####The Window Object####
- **window** object: represents an instance of the browser.
- **window** object serves a dual purpose:
	1. acting as the JavaScript interface to the browser window
	2. the ECMAScript **global** object

####The global Scope####
- all variables and functions declared globally become properties and method of the **window** object
- global variables cannot be removed using the **delete** operator, while properties defined directly on **window** can.(despite global variables become properties of the **window** object)
- Properties of **window** that were added via **var** statements have their [[configurable]] attribute set to *false* and so may not be removed by the **delete** operator.Internet Explorer 8 and earlier enforced this by throwing an error when the  **delete** operator is used on  **window** properties regardless of how they were originally created. Internet Explorer 9 and later do not throw an error.

attempting to access an undeclared variable throws an error, but it is possible to check for the existence of a potentially undeclared variable by looking on the  window object. 

	//this throws an error because oldValue is undeclared
	var newValue = oldValue;
	//this doesn’t throw an error, because it’s a property lookup
	//newValue is set to undefined
	var newValue = window.oldValue;

- Internet Explorer for Windows Mobile doesn’t allow direct creation of new properties or methods on the  window object via  ***window.property = value*** . All variables and functions declared globally, however, will still become members of **window**.


####Window Relationships and Frames####
- If a page contains frames, each frame has its own **window** object and is stored in the frames collection.Each **window** object has a name property containing the **name** of the frame.
- The **top** object always points to the very top(outermost) frame, which is the browser window itself.
- The **parent** object always points to the current frame's immediate parent frame. In some cases, **parent** may be equal to **top**, and when there are no frames, **parent** is equal to **top**(and both are equal to **window**)
- Any code written within a frame that references the  **window** object is pointing to that frame’s unique instance rather than the topmost one.
- the topmost  window will never have a value set for  **name** unless the window was opened using  **window.open()**
- There is one final **window** object, call **self**, which always points to **window**.
- since each **window** object contains the native type constructors, each frame has its own version of constructor, which are not equal.

####Window Position####

####Navigating and Opening Windows####




##Chapter 21 Ajax and Comet##

	function createXHR(){
		if (typeof XMLHttpRequest != “undefi ned”){
			return new XMLHttpRequest();
		} else if (typeof ActiveXObject != “undefi ned”){
			if (typeof arguments.callee.activeXString != “string”){
				var versions = [“MSXML2.XMLHttp.6.0”, “MSXML2.XMLHttp.3.0”,
				    “MSXML2.XMLHttp”],
				    i, len;
				for (i=0,len=versions.length; i < len; i++){
					try {
						new ActiveXObject(versions[i]);
						arguments.callee.activeXString = versions[i];
						break;
					} catch (ex){
						//skip
					}
				}
			}
			return new ActiveXObject(arguments.callee.activeXString);
		} else {
			throw new Error(“No XHR object available.”);
		}
	}


####XHR Usage####
- the call to  **open()** does not actually send the request; it simply prepares a request to be sent.
- The  **send()** method accepts a single argument, which is data to be sent as the body of the request. If no body data needs to be sent, you must pass in  null , because this argument is required for some browsers. 


- *responseText*: The text that was returned as the body of the response.
- *responseXML*: Contains an XML DOM doucment with the response data if the response has a content type of "text/xml" or "application/xml"
- *status*: The HTTP status of the response.
- *statusText*: The Description of the HTTP status


#####readyState#####
- 0 : Unititialized. The *open()* method hasn't been call yet;
- 1 : Open. The *open()* method has been called but *send()* has not been called.
- 2 : Sent. The *send()* method has been called but no response has been received.
- 3 : Receiving. Some response data has been retrieved.
- 4 : Compleite. All of the response data has been retrieved and is avaiable.





- cancel an asynchronous request before a response is received: **abort()**

####HTTP Headers####
- Accept: The content types that the browser can handle.
- Accept-Charset: The character sets that the browser can display.
- Accept-Encoding: The compression encodings handles by the browser.
- Accept-Language: The languages the browser is running in.
- Connenction: The type of connection the browser is making with the server.
- Cookie: Any cookies set on the page.
- Host: The domain of the page making the request.
- Referer: the URI of the page making the request 
- User-Agent: The browser's user-agent string


- **setRequestHeader()** accepts two arguments: the name of the header and the value of the header.
- **setRequestHeader()** must be call after **open()** but before **send()**
- **getResponseheader()**
- **getAllResponseHeaders()**

####GET Requests####
- query-string arguments can be appended to the end of the URL to pass information to the server.
- query string must be present and encoded correctly on the URL that is passed into the  open() method.
- Each query-string name and value must be encoded using  **encodeURIComponent()** before being attached to the URL, and all of the name-value pairs must be separated by an ampersand

