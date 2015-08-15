##Chapter 3: Types, Values, and Variables

two types:

	1.reference types
	2.primitive types

![](https://cloud.githubusercontent.com/assets/3389862/9289389/71520f58-43a0-11e5-8f90-d3c9423aa267.png)
##CHAPTER 18 Scripted HTTP


###3.6 Wrapper Objects
When the value of a property is a function, we call it a method.
Wrapper Objects differ from objects in that their properties are read-only and that you can’t define new properties on them.

###3.7 Immutable Primitive Values and Mutable Object References

two objects are not equal even if they have the same properties and values. And two arrays are not equal even if they have the same elements
in the same order:
```javascript
var o = {x:1}, p = {x:1}; // Two objects with the same properties
o === p // => false: distinct objects are never equal
var a = [], b = []; // Two distinct, empty arrays
a === b // => false: distinct arrays are never equal
```

- two object values are the same if and only if they refer to the same underlying object.
- assigning an object (or array) to a variable simply assigns the reference.

####3.8.1 Conversions and Equality
```javascript
null == undefined // These two values are treated as equal.
"0" == 0 // String converts to a number before comparing.
0 == false // Boolean converts to number before comparing.
"0" == false // Both operands convert to numbers before comparing.
```

####3.8.2 Explicit Conversions
- any value other than  `null` or  `undefined` has a  `toString()` method
- global functions(not methods of any class): parseInt() & parseFloat()


###3.9 Variable Declaration
If you don’t specify an initial value for a variable with the  var statement, the variable is declared, but its value is  undefined until your code stores a value into it.


####3.10.1 Function Scope and Hoisting

- JavaScript uses function scope: variables are visible within the function in which they are defined and within any functions that are nested within that function.
- JavaScript’s function scope means that all variables declared within a function are visible throughout the body of the function.
- hoisting: JavaScript code behaves as if all variable declarations in a function (but not any associated assignments) are “hoisted” to the top of the function

####3.10.2 Variables As Properties

When you declare a global JavaScript variable, what you are actually doing is defining a property of the global object (§3.5). If you use  var to declare the variable, the property that is created is nonconfigurable (see §6.7), which means that it cannot be deleted with the  delete operator.

if you’re not using strict mode and you assign a value to an undeclared variable, JavaScript automatically creates a global variable for you. Variables created in this way are regular, configurable properties of the global object and they can be deleted

####3.10.3 The Scope Chain

Global variables are defined throughout the program. Local variables are defined throughout the function in which they are declared, and also within any functions nested within that function.

###4.1 Primary Expressions

When any identifier appears by itself in a program, JavaScript assumes it is a variable and looks up its value. If no variable with that name exists, the expression evaluates to the  undefined value.In the strict mode of ECMAScript 5, however, an attempt to evaluate a nonexistent variable throws a ReferenceError instead.

###4.4 Property Access Expressions
- null and undefined can not have property

###4.5 Invocation Expressions

If the function uses a  return statement to return a value, then that value becomes the value of the invocation expression. Otherwise, the value of the invocation expression is  undefined .

In method invocations, the object or array that is the subject of the property access becomes the value of the  this parameter while the body of the function is being executed.

In ECMAScript 5, however, functions that are defined in strict mode are invoked with  undefined as their  this value rather than the global object.


###4.6 Object Creation Expressions

An object creation expression creates a new object and invokes a function (called a constructor) to initialize the properties of that object.(If no parameter,the empty pair of parentheses can be omitted):new Object();new Object.

When an object creation expression is evaluated, JavaScript first creates a new empty object, just like the one created by the object initializer  {} . Next, it invokes the specified function with the specified arguments, passing the new object as the value of the this keyword. The function can then use  this to initialize the properties of the newly created object. Functions written for use as constructors do not return a value, and the value of the object creation expression is the newly created and initialized object. If a constructor does return an object value, that value becomes the value of the object creation expression and the newly created object is discarded.


###4.8 Arithmetic Expressions

- In JavaScript,however, all numbers are floating-point.

The conversions rules for + give priority to string concatenation: if either of the operands is a string or an object that converts to a string, the other operand is converted to a string and concatenation is performed.

####4.9.1 Equality and Inequality Operators

JavaScript objects are compared by reference, not by value. Anobject is equal to itself, but not to any other object. If two distinct objects have the samenumber of properties, with the same names and values, they are still not equal. Twoarrays that have the same elements in the same order are not equal to each other.

- The strict equality operator  === evaluates its operands, and then compares the two	values as follows, performing no type conversion:

	- If the two values have different types, they are not equal.
	- If both values are  null or both values are  undefined , they are equal.  
	- If both values are the boolean value  true or both are the boolean value  false , they are equal.
	- If one or both values is  NaN , they are not equal. The  NaN value is never equal to any other value, including itself! To check whether a value  x is  NaN , use  x !== x .  NaN is the only value of  x for which this expression will be true.
	- If both values are numbers and have the same value, they are equal. If one value is 0 and the other is  -0 , they are also equal.
	- If both values are strings and contain exactly the same 16-bit values (see the sidebar in §3.2) in the same positions, they are equal. If the strings differ in length or content, they are not equal. Two strings may have the same meaning and the same visual appearance, but still be encoded using different sequences of 16-bit values. JavaScript performs no Unicode normalization, and a pair of strings like this are not considered equal to the  === or to the  == operators. See String.localeCompare() in Part III for another way to compare strings.
	- If both values refer to the same object, array, or function, they are equal. If they refer to different objects they are not equal, even if both objects have identical properties.


- The equality operator  == is like the strict equality operator, but it is less strict. If the values of the two operands are not the same type, it attempts some type conversions and tries the comparison again:
	- If the two values have the same type, test them for strict equality as described above.
	- If they are strictly equal, they are equal. If they are not strictly equal, they are not equal.
	- If the two values do not have the same type, the  == operator may still consider them equal. Use the following rules and type conversions to check for equality:
 	- If one value is  null and the other is  undefined , they are equal.
	- If one value is a number and the other is a string, convert the string to a number and try the comparison again, using the converted value.
	- If either value is  true , convert it to 1 and try the comparison again. If either value is  false , convert it to 0 and try the comparison again.
	- If one value is an object and the other is a number or string, convert the object to a primitive using the algorithm described in §3.8.3 and try the comparison again. An object is converted to a primitive value by either its  toString() method or its  valueOf() method. The built-in classes of core JavaScript attempt valueOf() conversion before  toString() conversion, except for the Date class, which performs  toString() conversion. Objects that are not part of core Java Script may convert themselves to primitive values in an implementation-defined way.
	- Any other combinations of values are not equal.

####4.9.2 Comparison Operators

The operands of these comparison operators may be of any type. Comparison can be performed only on numbers and strings, however, so operands that are not numbers or strings are converted. Comparison and conversion occur as follows:
	     ? If either operand evaluates to an object, that object is converted to a primitive value
	     as described at the end of §3.8.3: if its  valueOf() method returns a primitive value,
	     that value is used. Otherwise, the return value of its  toString() method is used.
	     ? If, after any required object-to-primitive conversion, both operands are strings, the
	     two strings are compared, using alphabetical order, where “alphabetical order” is
	     defined by the numerical order of the 16-bit Unicode values that make up the
	     strings.
	     ? If, after object-to-primitive conversion, at least one operand is not a string, both
	     operands are converted to numbers and compared numerically.  0 and  -0 are con-
	     sidered equal.  Infinity is larger than any number other than itself, and
	     -Infinity is smaller than any number other than itself. If either operand is (or
	               converts to)  NaN , then the comparison operator always returns  false .

####4.9.4 The instanceof Operator

in JavaScript, classes of objects are defined by the constructor function that initializes them. Thus, the right-side operand of  instanceof should be a function.If not , it throws a  TypeError .

all objects are instances of Object ;all arrays are objects

####4.10.1 Logical AND (&&)

The falsy values are  false:null ,  undefined ,  0 ,  -0 ,  NaN , and  "" . All other values, including all objects, are truthy.

In JavaScript, any expression or statement that expects a boolean value will work with a truthy or falsy value, so the fact that  && does not always return  true or  false does not cause practical problems.

"&&" This operator starts by evaluating its first operand, the expression on its left. If the value on the left is falsy, the value of the entire expression must also be falsy, so  && simply returns the value on the left and does not even evaluate the expression on the right. self remark: else, it seek to the next operand and do the same job till the last operand ,return its value.All in all, "&&" return first falsy value or last value(no matter truthy or falsy)


####4.10.2 Logical OR (||)

"||" returns the first truthy value .you should avoid right-side operands that include side effects,unless you purposely want to use the fact that the right-side expression may not be evaluated.

####4.10.3 Logical NOT (!)

the ! operator converts its operand to a boolean value before inverting the converted value. This means that  ! always returns  true or  false , and that you can convert any value  x to its equivalent boolean value by applying this operator twice:  !!x


####4.11.1 Assignment with Operation

In most cases, the expression:
	          a op= b

where  op is an operator, is equivalent to the expression:
	          a = a op b

In the first line, the expression  a is evaluated once. In the second it is evaluated twice.The two cases will differ only if  a includes side effects such as a function call or an increment operator. The following two assignments, for example, are not the same:

```javascript
data[i++] *= 2;
data[i++] = data[i++] * 2;
```

####4.13.2 The typeof Operator

	 typeof returns “object” if the operand value is  null

####4.13.3 The delete Operator

delete expects its operand to be an lvalue. If it is not an lvalue, the operator takes no action and returns  true . Otherwise,  delete attempts to delete the specified lvalue. delete returns  true if it successfully deletes the specified lvalue. Not all properties can be deleted, however: some built-in core and client-side properties are immune from deletion, and user-defined variables declared with the  var statement cannot be deleted. Functions defined with the  function statement and declared function parameters cannot be deleted either.


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
