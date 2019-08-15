# JavaScript: The Good Parts, 1st Edition

By: Douglas Crockford. [Purchase the book](https://www.amazon.com/JavaScript-Good-Parts-Douglas-Crockford/dp/0596517742)!

Date Started: Tuesday, August 13, 2019

Date Finished: ongoing



### Chapter 5: Inheritance

- In classical languages, objects are instances of classes, and a class can inherit from another class. JavaScript is a prototypal language, which means that objects inherit directly from other objects.

- In classical languages, class inheritance is the only form of code reuse. Much of the complexity of class hierarchies is motivated by the constraints of static type checking.
  
  JavaScript, being a loosely typed language, never casts. The lineage of an object is irrelevant. What matters about an object is what it can do, not what it is descended from.

- When a function object is created, the new function object is given a `prototype` property whose value is an object containing a `constructor` property whose value is the new function object.
  
  The function constructor that produces the function object runs some code like this:
  
  ```javascript
  this.prototype = {constructor: this};
  ```

- One problem of the constructor invocation pattern is that forgetting to include the `new` prefix when calling a constructor function, will bind the `this` to the global object instead of the new object.
  
  To mitigate this problem, there is a convention that all constructor functions are named with an initial capital, and that nothing else is spelled with an initial capital.

- Constructor functions can be written to accept a single object specifier that contains the specification of the object to be constructed, instead of accepting a large number of parameters.

- In the *pretend privacy* pattern, a private property has an odd looking name hoping that other users of the code will pretend that they cannot see the odd looking members.

- Functional constructor (module pattern) contains four steps:
  
     1. Create a new object via one of the following methods: object literal, constructor function with the `new` prefix, `Object.create()`, or call any function that returns an object.
  
     2. Define private instance variables and methods. These are just ordinary vars of the function.
  
     3. Augments that new object with methods. Those methods will have privileged access to the parameters and the vars defined in the second step.
  
     4. Return that new object.

- A durable object is simply a collection of functions that act as capabilities. A durable object cannot be compromised. Access to a durable object does not give an attacker the ability to access the internal state of the object except as permitted by the methods.
