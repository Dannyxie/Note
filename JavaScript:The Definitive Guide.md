##Chapter 8
####8.7.4 The bind() Method####
- any arguments you pass to **bind()** after the first are bound along with the **this** value.

The  bind() method defined by ECMAScript 5 does have some features that cannot be simulated with the ECMAScript 3 code.

1. the true **bind()** method returns a function object with its **length** property properly set to the arity of the bound function minus the number of bound arguments(but not less than zero).
2. the ECMAScript 5 **bind()** method can be used for partial application of constructor functions. If the function return by **bind()** is used as a constructor, the **this** passed to **bind()** is ignored, and the original function is invoded as a constructor, with some arguments already bound. Functions returned by the **bind()** method do not have a prototype property (the  prototype property of regular functions cannot be deleted) and objects created when these bound functions are used as constructors inherit from the prototype of the original, unbound constructor. Also, a bound constructor works just like the unbound constructor for the purposes of the  instanceof operator.

##Chapter 9 Classes and Modules
####Constructors and Class Identity

- two objects are instances of the same class if and only they inherit from the same prototype object.
- the **instanceof** operator does not actually check whether an instance was initialized by the constructor function. It checks whether it inherits from the constructor function's prototype.

####9.2.2 The constructor Property
- Every JavaScript function (except functions returned by the ECMAScript 5 **Function.bind()** method) automatically has a **prototype** property. The value of this property is an object that has a single nonenumerable **constructor** property.

####9.5.1 The Instanceof Operator
- The expression ***o instanceof c*** evaluates to true if o inherits from c.prototype. (The inheritance need not be direct)
- **isPrototypeOf()**: test the prototype chain of an object for a specific prototype object.


