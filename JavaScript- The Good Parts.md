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
- The `for in` statement can loop over all of the property names in an object(enumerable properties). The enumeration will include all of the properties—including functions and prototype properties(There is no guarantee on the order of the names).

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


