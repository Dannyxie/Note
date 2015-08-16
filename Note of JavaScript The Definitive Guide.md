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



#Chapter 5 Statements

Expressions with side effects, such as assignments and function invocations, can stand alone as statements, and when used this way they are known as expression statements.

By default, the JavaScript interpreter executes statements one after another in the order they are written.

###5.3.1 var

 Global variables are properties of the global object. Unlike other global properties, however, properties created with  var cannot be deleted.

If no initializer is specified for a variable with the  var statement, the variable’s initial value is  undefined.

###5.3.2 function

Function definitions may not appear within  if statements,  while loops, or any other statements(top-level).

With function declaration statements, however, both the function name and the function body are hoisted: all functions in a script or all nested functions in a function are declared before any other code is run. This means that you can invoke a JavaScript function before you declare it.

Like the  var statement, function declaration statements create variables that cannot be deleted.

###5.4.3 switch

The matching case is determined using the  === identity operator,not the  == equality operator, so the expressions must match without any type conversion.

###5.5.1 while

To execute a  while statement, the interpreter first evaluates  expression . If the value of the expression is falsy, then the interpreter skips over the  statement that serves as the loop body and moves on to the next statement in the program.

###5.5.2 do/while

The  do/while loop is like a  while loop, except that the loop expression is tested at the bottom of the loop rather than at the top. This means that the body of the loop is always executed at least once

###5.5.4 for/in

JavaScript arrays are simply a specialized kind of object and array indexes are object properties that can be enumerated with a  for/in loop.

The  for/in loop does not actually enumerate all properties of an object, only the enumerable properties.The various built-in methods defined by core JavaScript are not enumerable. User-defined inherited properties (see §6.2.2) are also enumerated by the  for/in loop.

If the body of a  for/in loop deletes a property that has not yet been enumerated, that property will not be enumerated. If the body of the loop defines new properties on the object, those properties will generally not be enumerated. (Some implementations may enumerate inherited properties that are added after the loop begins, however.)

###5.5.4.1 Property enumeration order

JavaScript implementations from all major browser vendors enumerate the properties of simple objects in the order in which they were defined, with older properties enumerated first.

(incomprehension)Typically (but not in all implementations), inherited properties (see §6.2.2) are enumerated after all the noninherited “own” properties of an object, but are also enumerated in the order in which they were defined. If an object inherits properties from more than one “prototype” (see §6.1.3)—i.e., if it has more than one object in its “prototype chain”—then the properties of each prototype object in the chain are enumerated in creation order before enumerating the properties of the next object. Some (but not all) implementations enumerate array properties in numeric order rather than creation order, but they revert to creation order if the array is given other non-numeric properties as well or if the array is sparse (i.e., if some array indexes are missing).

###5.6.4 return

A  return statement may appear only within the body of a function. It is a syntax error for it to appear anywhere else.

With no  return statement, a function invocation simply executes each of the statements in the function body in turn until it reaches the end of the function, and then returns to its caller. In this case, the invocation expression evaluates to  undefined .

###5.6.6 try/catch/finally

If an exception occurs in the  try block and there is an associated  catch block to handle the exception, the interpreter first executes the  catch block and then the  finally block. If there is no local  catch block to handle the exception, the interpreter first executes the  finally block and then jumps to the nearest containing  catch clause.

If a  finally block itself causes a jump with a  return ,  continue ,  break , or  throw statement, or by calling a method that throws an exception, the interpreter abandons whatever jump was pending and performs the new jump

###5.7.3 “use strict”

It does not include any language keywords: the directive is just an expression statement that consists of a special string literal (in single or double quotes).

It can appear only at the start of a script or at the start of a function body, before any real statements have appeared.
The differences between strict mode and non-strict mode are the following (the first three are particularly important):

 The  with statement is not allowed in strict mode.

 In strict mode, all variables must be declared: a ReferenceError is thrown if youassign a value to an identifier that is not a declared variable, function, function parameter,  catch clause parameter, or property of the global object. (In non-strictmode, this implicitly declares a global variable by adding a new property to theglobal object.)

- In strict mode, functions invoked as functions (rather than as methods) have athis value of  undefined . (In non-strict mode, functions invoked as functions arealways passed the global object as their  this value.) This difference can be used todetermine whether an implementation supports strict mode:var hasStrictMode = (function() { "use strict"; return this===undefined}());Also, in strict mode, when a function is invoked with  call() or  apply() , the  thisvalue is exactly the value passed as the first argument to  call() or  apply() . (Innonstrict mode,  null and  undefined values are replaced with the global object andnon-object values are converted to objects.)
- In strict mode, assignments to nonwritable properties and attempts to create newproperties on nonextensible objects throw a TypeError. (In non-strict mode, theseattempts fail silently.)
- In strict mode, code passed to  eval() cannot declare variables or define functionsin the caller’s scope as it can in non-strict mode. Instead, variable and functiondefinitions live in a new scope created for the  eval() . This scope is discarded whenthe  eval() returns.
- In strict mode, the  arguments object (§8.3.2) in a function holds a static copy ofthe values passed to the function. In non-strict mode, the  arguments object has“magical” behavior in which elements of the array and named function parametersboth refer to the same value.
- In strict mode, a SyntaxError is thrown if the  delete operator is followed by anunqualified identifier such as a variable, function, or function parameter. (In non-strict mode, such a  delete expression does nothing and evaluates to  false .)
- In strict mode, an attempt to delete a nonconfigurable property throws aTypeError. (In non-strict mode, the attempt fails and the  delete expression eval-uates to  false .)
- In strict mode, it is a syntax error for an object literal to define two or more prop-erties by the same name. (In non-strict mode, no error occurs.)
- In strict mode, it is a syntax error for a function declaration to have two or moreparameters with the same name. (In non-strict mode, no error occurs.)
- In strict mode, octal integer literals (beginning with a 0 that is not followed by an          x) are not allowed. (In non-strict mode, some implementations allow octal literals.)
- In strict mode, the identifiers  eval and  arguments are treated like keywords, andyou are not allowed to change their value. You cannot assign a value to these iden-tifiers, declare them as variables, use them as function names, use them as function parameter names, or use them as the identifier of a  catch block.
- In strict mode, the ability to examine the call stack is restricted.  arguments.caller and  arguments.callee both throw a TypeError within a strict mode function. Strict mode functions also have  caller and  arguments properties that throw TypeError when read. (Some implementations define these nonstandard properties on non-strict functions.)



#Chapter 6

##6.1 Creating Objects

Creating objects:with object literals, with the new keyword, and (in ECMAScript 5) with the Object.create() function.

###6.1.2 Creating Objects with new

The  new operator creates and initializes a new object. The  new keyword must be followed by a function invocation. A function used in this way is called a constructor and serves to initialize a newly created object.

###6.2.2 Inheritance

Property assignment examines the prototype chain to determine whether the assignment is allowed. If  o inherits a read-only property named  x , for example, then the assignment is not allowed. (Details about when a property may be set are in §6.2.3.) If the assignment is allowed, however, it always creates or sets a property in the original object and never modifies the prototype chain. The fact that inheritance occurs when querying properties but not when setting them is a key feature of JavaScript because it allows us to selectively override inherited properties:

```javascript
var unitcircle = { r:1 }; // An object to inherit from
var c = inherit(unitcircle); // c inherits the property r
c.x = 1; c.y = 1; // c defines two properties of its own
c.r = 2; // c overrides its inherited property
unitcircle.r; // => 1: the prototype object is not affected
```

 If  o inherits the property  x , and that property is an accessor property with a setter method (see §6.6), then that setter method is called rather than creating a new property  x in  o .Note, however, that the setter method is called on the object  o , not on the prototype object that defines the property, so if the setter method defines any properties, it will do so on  o , and it will again leave the prototype chain unmodified.

The rules that specify when a property assignment succeeds and when it fails are intuitive but difficult to express concisely. An attempt to set a property  p of an object  o fails in these circumstances:

o has an own property  p that is read-only: it is not possible to set read-only properties. (See the  defineProperty() method, however, for an exception that allows configurable read-only properties to be set.)

o has an inherited property  p that is read-only: it is not possible to hide an inherited read-only property with an own property of the same name.

o does not have an own property  p ;  o does not inherit a property  p with a setter method, and  o ’s  extensible attribute (see §6.8.3) is  false . If  p does not already exist on  o , and if there is no setter method to call, then  p must be added to  o . But if  o is not extensible, then no new properties can be defined on it.


##6.3 Deleting Properties

The  delete operator only deletes own properties, not inherited ones. (To delete an inherited property, you must delete it from the prototype object in which it is defined.Doing this affects every object that inherits from that prototype.)

A  delete expression evaluates to  true if the delete succeeded or if the delete had no effect (such as deleting a nonexistent property).  delete also evaluates to  true when used (meaninglessly) with an expression that is not a property access expression

delete does not remove properties that have a configurable attribute of  false .

##6.4 Testing Properties

The  in operator expects a property name (as a string) on its left side and an object on its right. It returns  true if the object has an own property or an inherited property by that name(note :for/in loop just list the enumerable property (own or inherited) of the specified object )

The  hasOwnProperty() method of an object tests whether that object has an own property with the given name. It returns  false for inherited properties

The  propertyIsEnumerable() refines the  hasOwnProperty() test. It returns  true only if the named property is an own property and its  enumerable attribute is  true . Certain built-in properties are not enumerable.Properties created by normal JavaScript code are enumerable unless you’ve used one of the ECMAScript 5 methods shown later to make them nonenumerable

There is one thing the  in operator can do that the simple property access technique shown above cannot do.  in can distinguish between properties that do not exist and properties that exist but have been set to  undefined .

 !== and  === distinguish between  undefined and  null .

##6.5 Enumerating Properties

Built-in methods that objects inherit are not enumerable, but the properties that your code adds to objects are enumerable (unless
you use one of the functions described later to make them nonenumerable).

```javascript
  Object.keys() :     returns an array of the names of the enumerable own properties of an object.
  Object.getOwnPropertyNames() :     returns the names of all the own properties of the specified object, not just the enumerable properties.
```

##6.6 Property Getters and Setters

Properties defined by getters and setters are sometimes known as accessor properties to distinguish them from data properties that have a simple value.

Accessor properties are defined as one or two functions whose name is the same as the property name, and with the  function keyword replaced with  get and/or  set .

##6.7 Property Attributes

 The four attributes of a data property are value, writable, enumerable, and configurable.

 The four attributes of an accessor property are get, set, enumerable, and configurable.

To obtain the property descriptor for a named property of a specified object, call Object.getOwnPropertyDescriptor()          note:works only for own properties.(Object.getOwnPropertyDescriptor() returns undefined for inherited properties and properties that don't exist.)

To set the attributes of a property, or to create a new property with the specified attributes, call  Object.defineProperty()

![](https://cloud.githubusercontent.com/assets/3389862/9292645/7bb142f0-4438-11e5-844c-728eff3bff75.png)


- Object.defineProperty(): If you’re creating a new property, then omitted attributes are taken to be  false or  undefined .Note that this method alters an existing own property or creates a new own property, but it will not alter an inherited property.

- Modify multiple properties:Object.defineProperties()

![](https://cloud.githubusercontent.com/assets/3389862/9292658/bd8143c4-4438-11e5-98c5-d302e9b91a43.png)


Calls to Object.defineProperty() or  Object.defineProperties() that attempt to violate them throw TypeError:

	1. If an object is not extensible, you can edit its existing own properties, but you cannot add new properties to it.
	2. If a property is not configurable, you cannot change its configurable or enumerable attributes.
	3. If an accessor property is not configurable, you cannot change its getter or setter method, and you cannot change it to a data property.
	4. If a data property is not configurable, you cannot change it to an accessor property.
	5. If a data property is not configurable, you cannot change its writable attribute from false to  true , but you can change it from  true to  false .
	6. If a data property is not configurable and not writable, you cannot change its value. You can change the value of a property that is configurable but nonwritable, however (because that would be the same as making it writable, then changing the value, then converting it back to nonwritable).


###6.7.1 Legacy API for Getters and Setters

__lookupGetter__() and  __lookupSetter__() return the getter or setter method for a named property. And  __defineGetter__() and  __defineSetter__() define a getter or setter: pass the property name first and the getter or setter method second

###6.8.1 The prototype Attribute

Objects created with  new use the value of the  prototype property of their constructor function as their prototype. And objects created with  Object.create() use the first argument to that function (which may be  null ) as their prototype.

To determine whether one object is the prototype of (or is part of the prototype chain of) another object, use the  isPrototypeOf() method.

Objects created with a  new expression usually inherit a constructor property that refers to the constructor function used to create the object.

###6.8.3 The extensible Attribute

To determine whether an object is extensible, pass it to  Object.isExtensible() . To make an object nonextensible, pass it to Object.preventExtensions() . Note that there is no way to make an object extensible again once you have made it nonextensible. Also note that calling  preventExtensions() only affects the extensibility of the object itself. If new properties are added to the prototype of a nonextensible object, the nonextensible object will inherit those new properties.

Object.seal() works like  Object.preventExtensions() , but in addition to making the object nonextensible, it also makes all of the own properties of that object nonconfigurable

Object.isSealed() :determine whether an object is sealed.

Object.freeze() In addition to making the object nonextensible and its properties nonconfigurable, it also makes all of the object’s own data properties read-only. (If the object has accessor properties with setter methods,these are not affected and can still be invoked by assignment to the property.)

Object.isFrozen(): to determine if an object is frozen.

Object.seal() and  Object.freeze() affect only the object they are passed: they have no effect on the prototype of that object. If you want to thoroughly lock down an object, you probably need to seal or freeze the objects in the prototype chain as well.

##6.9 Serializing Objects

JSON.stringify() and JSON.parse() to serialize and restore JavaScript objects.

JSON.parse() leaves these in string form and does not restore the original Date object. Function, RegExp, and Error objects and the  undefined value cannot be serialized or restored.  JSON.stringify() serializes only the enumerable own properties of an object. If a property value cannot be serialized, that property is simply omitted from the stringified output


#Chapter 7

##7.1 Creating Arrays

Array literal syntax allows an optional trailing comma, so  [,,] has only two elements,not three

Array() :Call it with a single numeric argument, which specifies a length: var a = new Array(10);

##7.3 Sparse Arrays

when you omit value in an array literal, you are not creating a sparse array.The omitted element exists in the array and has the value  undefined .

##7.4 Array Length

the length property specifies the number of elements in the array. Its value is one more than the highest index in the array

if you set the  length property to a non-negative integer  n smaller than its current value, any array elements whose index is greater than or equal to  n are deleted from the array

##7.5 Adding and Deleting Array Elements

Note that using  delete on an array element does not alter the  length property and does not shift elements with higher indexes down to fill in the gap that is left by the deleted property. If you delete an element from an array, the array becomes sparse.

You can delete array elements with the  delete operator, just as you can delete object properties. using  delete on an array element does not alter the  length property and does not shift elements with higher indexes down to fill in the gap that is left by the deleted property

##7.8 Array Methods

 Array.join() method converts all the elements of an array to strings and concatenates them, returning the resulting string. the inverse of the  String.split() method.Array.join() :If no separator string is specified, a comma is used.

 Array.reverse() method reverses the order of the elements of an array and returns the reversed array.It doesn’t create a new array with the elements rearranged but instead rearranges them in the already existing array.

 Array.sort() sorts the elements of an array in place and returns the sorted array. If an array contains undefined elements, they are sorted to the end of the array. When sort() is called with no arguments, it sorts the array elements in alphabetical order (temporarily converting them to strings to perform the comparison, if necessary).If an array contains undefined elements, they are sorted to the end of the array.

To sort an array into some order other than alphabetical, you must pass a comparison function as an argument to

```javascript
sort() .var a = [33, 4, 1111, 222];
a.sort(); // Alphabetical order: 1111, 222, 33, 4
a.sort(function(a,b) { // Numerical order: 4, 33, 222, 1111
        return a-b; // Returns &lt; 0, 0, or &gt; 0, depending on order
        });
a.sort(function(a,b) {return b-a}); // Reverse numerical order
```

Array.concat() method creates and returns a new array that contains the elements of the original array on which  concat() was invoked,followed by each of the arguments to  concat()

Array.slice() method returns a slice, or subarray, of the specified array. slice() does not modify the array on which it is invoked.

Array.splice() method is a general-purpose method for inserting or removing elements from an array. The first argument to  splice() specifies the array position at which the insertion and/or deletion is to begin. The second argument specifies the number of elements that should be deleted from (spliced out of) the array. If this second argument is omitted, all array elements from the start element to the end of the array are removed. These arguments may be followed by any number of additional arguments that specify elements to be inserted into the array, starting at the position specified by the first argument.

###7.8.8 unshift() and shift()

unshift() adds an element or elements to the beginning of the array, shifts the existing array elements up to higher indexes to make room, and returns the new length of the array.

shift() removes and returns the first element of the array, shifting all subsequent elements down one place to occupy the newly vacant space at the start of the array.

###7.9.1 forEach()

The  forEach() method iterates through an array, invoking a function you specify for each element. forEach() then invokes your function with three arguments: the value of the array element, the index of the array element, and the array itself. forEach() does not provide a way to terminate iteration before all elements have been passed to the function. (no equivalent of the  break statement you can use with a regular  for loop,but you can throw an exception, and place the call to  forEach() within a  try block to terminate the loop)

###7.9.2 map()

map() method passes each element of the array on which it is invoked to the function you specify, and returns an array containing the values returned by that function.

##7.10 Array Type

```javascript
     var isArray = Function.isArray || function(o) {
          return typeof o === "object" &&
          Object.prototype.toString.call(o) === "[object Array]";
};
```

##7.11 Array-Like Objects

JavaScript arrays have some special features that other objects do not have:

The  length property is automatically updated as new elements are added to the list.

Setting  length to a smaller value truncates the array.

Arrays inherit useful methods from  Array.prototype .

Arrays have a class attribute of “Array”.


#Chapter 8 Function

If a function is assigned to the property of an object, it is known as a method of that object. When a function is invoked on or through an object, that object is the invocation context or  this value for the function.

##8.1 Defining Functions

Functions are defined with the  function keyword, which can be used in a function definition expression  or in a function declaration statement.

function definitions needs an identifier that names the function. The name is a required part of function declaration statements: it is used as the name of a variable, and the newly defined function object is assigned to the variable. For function definition expressions, the name is optional: if present, the name refers to the function object only within the body of the function itself.

If a function definition expression includes a name, the local function scope for that function will include a binding of that name to the function object. In effect, the function name becomes a local variable within the function.  (For function definition expressions, the name is optional: if present, the name refers to the function object only within the body of the function itself.)

###8.2.1 Function Invocation

if the function is the property of an object or an element of an array—then it is a method invocation expression.

For function invocation in ECMAScript 3 and nonstrict ECMAScript 5, the invocation context (the  this value) is the global object. In strict mode, however, the invocation context is  undefined .

###8.2.2 Method Invocation

When methods return objects, you can use the return value of one method invocation as part of a subsequent invocation.

```javascript
// Find all headers, map to their ids, convert to an array and sort them
$(":header").map(function() { return this.id }).get().sort();
```

If a nested function is invoked as a method, its this value is the object it was invoked on.

If a nested function is invoked as a function then its  this value will be either the global object (non-strict mode) or  undefined (strict mode).

Note that  this is a keyword, not a variable or property name. JavaScript syntax does not allow you to assign a value to  this .

![](https://cloud.githubusercontent.com/assets/3389862/9292680/bc4483da-4439-11e5-92a8-6f6bd61ef9e7.png)



###8.2.3 Constructor Invocation

If a function or method invocation is preceded by the keyword  new , then it is a constructor invocation.

if a constructor has no parameters, then JavaScript constructor invocation syntax allows the argument list and parentheses to be omitted entirely.

```javascript
var o = new Object();
var o = new Object;
```

A constructor invocation creates a new, empty object that inherits from the prototype property of the constructor. Constructor functions are intended to initialize objects and this newly created object is used as the invocation context, so the constructor function can refer to it with the  this keyword.Note that the new object is used as the invocation context even if the constructor invocation looks like a method invocation. That is, in the expression  new o.m() ,  o is not used as the invocation context.

Constructor functions do not normally use the  return keyword.They typically initialize the new object and then return implicitly when they reach the end of their body. In this case, the new object is the value of the constructor invocation expression. If, however, a constructor explicitly used the  return statement to return an object, then that object becomes the value of the invocation expression. If the constructor uses  return with no value, or if it returns a primitive value, that return value is ignored and the new object is used as the value of the invocation.

###8.2.4 Indirect Invocation

The  call() method uses its own argument list as arguments to the function

The apply() method expects an array of values to be used as arguments.

###8.3.1 Optional Parameters

When a function is invoked with fewer arguments than declared parameters, the additional parameters are set to the  undefined value.

###8.3.2 Variable-Length Argument Lists: The Arguments Object

Remember that  arguments is not really an array; it is an Arguments object. Each Arguments object defines numbered array elements and a  length property, but it is not technically an array;

##8.5 Functions As Namespaces
Variables declared within a function are visible throughout the function (including within nested functions) but do not exist outside of the function.

We work around an IE bug here: in many versions of IE, the for/in loop won't enumerate an enumerable property of o if the prototype of o has a nonenumerable property by the same name. This means that properties like toString are not handled correctly unless we explicitly check for them.

##8.6 Closures

functions are executed using the scope chain that was in effect when they were defined.

Each time a JavaScript function is invoked, a new object is created to hold the local variables for that invocation, and that object is added to the scope chain. When the function returns, that variable binding object is removed from the scope chain. If there were no nested functions, there are no more references to the binding object and it gets garbage collected. If there were nested functions defined,then each of those functions has a reference to the scope chain, and that scope chain refers to the variable binding object. If those nested functions objects remained within their outer function, however, then they themselves will be garbage collected, along with the variable binding object they referred to. But if the function defines a nested function and returns it or stores it into a property somewhere, then there will be an external reference to the nested function. It won’t be garbage collected, and the variable binding object it refers to won’t be garbage collected either.

Nested functions do not make private copies of the scope or make static snapshots of the variable bindings.

```javascript
// Return an array of functions that return the values 0-9
function constfuncs() {
var funcs = [];
for(var i = 0; i < 10; i++)
           funcs[i] = function() { return i; };
           return funcs;
}
var funcs = constfuncs();
funcs[5]() // What does this return?  10 : When  constfuncs() returns, the value of the variable  i is 10, and all 10 closures share this value.
```

every function invocation has a  this value, and a closure cannot access the  this value of its outer function unless the outer function has saved that value into a variable

Since a closure has its own binding for arguments , it cannot access the outer function’s arguments array unless the outer function has saved that array into a variable by a different name

Scope chain associated with a closure is “live.” Nested functions do not make private copies of the scope or make static snapshots of the variable bindings.

###8.7.3 The call() and apply() Methods

call() and  apply() allow you to indirectly invoke a function as if it were a method of some other object.
Any arguments to  call() after the first invocation context argument are the values that are passed to the function that is invoked.
The  apply() method is like the  call() method, except that the arguments to be passed to the function are specified as an array


##Chapter 18
An HTTP request consists of four parts:

	1. the HTTP request method or "verb"
	2. the URL being requested
	3. an optional set of request headers, which may include authentication information
	4. an optional request body

The HTTP response sent by a server has three parts:

	1. a numeric and textual status code that indicates the success or failure of the request
	2. a set of respons headers
	3. the response body



#Chapter 9 Classes and Modules

In JavaScript, classes are based on JavaScript’s prototype-based inheritance mechanism. If two objects inherit properties from the same prototype object, then we say that they are instances of the same class.

One of the important features of JavaScript classes is that they are dynamically extendable

##9.1 Classes and Prototypes

In JavaScript, a class is a set of objects that inherit properties from the same prototype object.

##9.2 Classes and Constructors

Constructor invocations using  new automatically create the new object, so the constructor itself only needs to initialize the state of that new object.

The critical feature of constructor invocations is that the prototype property of the constructor is used as the prototype of the new object. This means that all objects created with the same constructor inherit from the same object and are therefore members of the same class.

Constructor invocation automatically creates a new object, invokes the constructor as a method of that object, and returns the new object.

###9.2.1 Constructors and Class Identity

two objects are instances of the same class if and only if they inherit from the same prototype object.

The  instanceof operator does not actually check whether  r was initialized by the Range constructor.It checks whether it inherits from  Range.prototype .

###9.2.2 The constructor Property

Therefore, every JavaScript function automatically has a  prototype property. The value of this property is an object that has a single nonenumerable  constructor property.The value of the  constructor property is the function object.

objects typically inherit a  constructor property that refers to their constructor.

##9.3 Java-Style Classes in JavaScript


In JavaScript, there are three different objects involved in any class definition (see Figure 9-1), and the properties of these three objects act like different kindsof class members:

Constructor object:     As we’ve noted, the constructor function (an object) defines a name for a JavaScript class. Properties you add to this constructor object serve as class fields and class methods (depending on whether the property values are functions or not).

Prototype object:     The properties of this object are inherited by all instances of the class, and properties whose values are functions behave like instance methods of the class.

Instance object:     Each instance of a class is an object in its own right, and properties defined directly on an instance are not shared by any other instances. Nonfunction properties defined on instances behave as the instance fields of the class.

Note that JavaScript instance methods must use the this keyword to access the instance fields.

###9.5.1 The instanceof operator

The expression  o instanceof c evaluates to  true if  o inherits from  c.prototype . The inheritance need not be direct.

constructors act as the public identity of classes, but prototypes are the fundamental identity a web application uses more than one window or frame. Each window or frame is a distinct execution context, and each has its own global object and its own set of constructor functions. Two arrays created in two different frames inherit from two identical but distinct prototype objects, and an array created in one frame is not  instanceof the Array() constructor of another frame.


###9.5.3 The Constructor Name
	The  Array constructor in one window is not equal to the  Array constructor in another window, but their names are equal.
	NaN is the only value not equal to itself:



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
