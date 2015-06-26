#JavaScript: The Good Parts 

JavaScript

---

##Good Parts
###Analyzing JavaScript
- JavaScript is a loosely type language, so JavaScript compilers are unable to detect type errors.
- JavaScript depends on global variables for linkage. All of the top-level variables of all compilation units are tossed together in a common namespace called the `global object`.

###Whitespace
- Comments should be used liberally to improve the readabilty of your programs. Take care that the comments always accurately describe the code. Obsolete comments are worse than no comments.

###Names
Reserved words:
`abstract
boolean break byte
case catch char class const continue
debugger default delete do double
else enum export extends
false final finally float for function
goto
if implements import in instanceof int interface
long
native new null
package private protected public
return
short static super switch synchronized
this throw throws transient true try typeof
var volatile void
while with`

###Statements
falsy value:

`false
null
undefined
'' //empty string
0
NaN`

##Chapter 3 Objects
- simple types: numbers, strings, booleans, null , undefined. All other values are object.
- Numbers, strings, and booleans are object-like in that they have methods, but they are immutable. Objects in JavaScript are mutable keyed collections.
- An property name can be any string, including the empty string.

###Retrieval
- If a property name has a legal JavaScript name  that is not a reserved word, we can use the `.` notation to access the property.

###Reference
- Objects are passed around by reference. They are never copied.

###Prototype
- All objects created from object literals are linked to `Object.prototype`

```javascript
if (typeof Object.create !== 'function') {
    Object.create = function (o) {
        var F = function () {};
        F.prototype = o;
        return new F();
    };
}
```

- The prototype link is used only in retrieval. If we try to retrieve a property value from an object, and if the object lacks the property name, then JavaScript attempts to retrieve the property value from the prototype object. And if that object is lacking the property, then it goes to its prototype, and so on until the process finally bottoms out with Object.prototype . If the desired property exists nowhere in the prototype chain, then the result is the undefined value. This is called delegation.

###Enumeration
- The `for in` statement can loop over all of the property names in an object(enumerable properties). The enumeration will include all of the properties' including functions and prototype properties(There is no guarantee on the order of the names).

###Delete
- The `delete` operator can be used to remove a property from an object. It will remove a property from the object if it has one. It will no touch any of the objects in the prototype linkage.

##Chapter 4 Functions
###Function Objects
- Every function object is also created with a `prototype` property. Its value is an object with a `constructor` property whose value is the function.
- If a function is not given a name, it is said to be `anonymous`.
###Invocation
- Every function receives two additional parameters: `this` and `arguments`. the `this`'s value is determined by the **invocation pattern**.

Pattern of invocation:
1. the method invocation pattern
2. the function invocation pattern
3. the constructor invocation pattern
4. the apply invocation pattern

- There is no runtime error when the number of arguments and the number of parameters do not match. If there are too many argument values, the extra argument value will be ignored. If there are too few argument values, the `undefined` value will be substituted for the missing values. There is no type checking on the arguments value: any type of value can be passed to any parameter.

####The Method Invocation Pattern
- When a function is stored as a property of an object, we call it a `method`
- When a method is invoked, `this` is bound to that object.
- A method can use `this` to access the object so that it can retrieve values from the object or modified the object.

###The Function invocation pattern
- When a function is not the property of an object, then it is invoked as a function
- When a function is invoked with this pattern, `this` is bound to the global object.
- When the inner function is invoked, `this` would still be bound to the `this` variable of the outer function. A consequence of this error is that a method cannot employ an inner function to help it do its work because the inner function does not share the method's access to the object as its `this` is bound to the wrong value.
- If the method defines a variable and assigns it the value of `this`, the inner function will have access `this` through that variable. 

###The Constructor Invocation Pattern
- If a function is invoked with the `new` prefix, then a new object will be created with a hidden link to the value of the function's `prototype` member, and `this` will be bound to that new object
- Functions that are intended to be used with the `new` prefix are called `constructor` (recomend:function with capitalized name).

###The Apply Invocation Pattern
- The `apply` method lets us construct an array of arguments to use to invoke a function.
- `apply` method takes to arguments: the first is the value that should be bound to `this`.the second is an array of parameters.


###Arguments
- `arguments` is a array-like object. `arguments` has a `length` property, but it lacks all of the array method.

###Return
- When a `return` is executed, the function returns immediately without executing the remaining statement.
- A function always return a value. If the `return` is not specified, the `undefined` is returned.
- If the function was invoked with the `new` prefix and the `return` is not an object, then `this`(the new object) is returned instead.

###Exceptions
- The `throw` statement interrupts execution of the function. It should be given an exception object containing a `name` property that identifies the type of the exception, and a descriptive `message` property.
- The `exception` object will be delivered to the `catch` clause of a `try` statement.
- If an exception is thrown within a `try` block, control will go to its `catch` clause.

###Argumenting Types
```javascript
Function.prototype.method = function (name, func){
    this.prototype[name] = func;
    return this;
};
```

###Recursion
- A `recursive` function is a function that calls itself, either directly or indirectly.

###Scope
- `Scope` in a programming language controls the visibility and lifetimes of variables and parameters.
- JavaScript does have function scope. This means that the parameters and variables defined in a function are not visible outside of the function, and that a variable defined anywhere within a function is visible everywhere within the function. So it is best to declare all of the variables used in a function at the top of the function body.

###Closure
- Inner functions get access to the parameters and variables of the functions they are defined within (with the exception of `this` and `arguments`)
```javascript
//BAD EXAMPLE
var add_the_handlers = function(nodes){
    var i;
    for(i=0; i < node.length; i += 1;){
        nodes[i].onclick = function(e){
            alert(i);
        };
    }
};

//BETTER EXAMPLE
var add_the_handlers = function(nodes){
    var i;
    for(i = 0; i < node.length; i += 1 ){
        nodes[i].onclick = function(i){
            return function(e){
                alert(e);
            };
        }(i);
    }
};
```

###Callbacks

###Module
- A module is a function or object that presents an interface but that hides its state and implemention.

###Curry

###Memoization


##Chapter 5 Inheritance
###Pseudoclassical
- When a function object is created, the `Function` constructor that produces the function object runs some code like this:`this.prototype = {constructor:this}` . The new function object is given a `prototype` property whose value is an object constaining a `constructor` property whose value is the new object.

##Chapter 6 Arrays
###Enumeration
- `for in` makes no guarantee about the order of the properties.

###Confusion
- the `typeof` operator reports that the type of an array is 'object'
```javascript
var is_array = function(value){
    return value && typeof value === 'object' &&
    typeof value.length === 'number' &&
    typeof value.splice === 'function' &&
    !(value.propertyIsEnumeralbe('length')); //length property is unumerable in array.
};
```

##Chapter 7 Regular Expressions
methods: 
`regexp.exec`
`regexp.test`
`string.match`
`string.replace`
`string.search`
`string.split`

- In a JavaScript program, the regular expression must be on a single line.


##Chapter 8 Method
###Array
- array.concat(item...)  : produce new array
- array.join(seperator) : default sperator is ','
- array.pop()
- array.push(item...)
- array.reverse()
- array.shift() : slower that pop
- array.slice(start,end)    : The first element copied will be `array[start]`. It will be **stop before** copying `array[end]`(opional)
- array.sort(comparefn)
- array.splice(start,deleteCount,item...)
- array.unshift(item....)   : returns the array's new length.

###Function
- function.apply(thisArg, argArray)

###Number
- number.toExponential(fractionDigits)
- number.toFixed(fractionDigits)
- number.toPrecision(precision)
- number.toString(radix)

###Object
- object.hasOwnProperty(name)

###RegExp
- regexp.exec(string)
- regexp.test(string)

###String
- string.charAt(pos)
- string.charCodeAt(pos)
- string.concat(string...)
- string.indexOf(searchString, position)
- string.lastIndexOf(searchString, position)
- string.localeCompare(that)
- string.match(regexp)
- string.replace(searchValue, replaceValue)
- string.search(regexp)
- string.slice(start,end)
- string.split(separator, limt)
- string.substring(start, end)
- string.toLocaleLowerCase()
- string.toLocaleUpperCase()
- string.toLowerCase()
- string.toUpperCase()
- String.fromCharCode(char...)



