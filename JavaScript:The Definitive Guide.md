
##Chapter 9 Classes and Modules
####Constructors and Class Identity

- two objects are instances of the same class if and only they inherit from the same prototype object.
- the **instanceof** operator does not actually check whether an instance was initialized by the constructor function. It checks whether it inherits from the constructor function's prototype.


####9.5.1 The Instanceof Operator
- The expression ***o instanceof c*** evaluates to true if o inherits from c.prototype. (The inheritance need not be direct)
- **isPrototypeOf()**: test the prototype chain of an object for a specific prototype object.

