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

### Chapter 6: Arrays

- JavaScript provides an object that has some array-like characteristics. It converts array subscripts into strings that are used to make properties. It is significantly slower than a real array data structure.

- Array literal objects inherit from `Array.prototype` so they have access to a larger set of useful methods as well as the `length` property.

- In JavaScript the array's `length` property is not an upper bound. If you store an element with a subscript that is greater than or equal to the current length, the length will increase to contain the new element. There is no array bounds error.

- There are two ways to append a new element to an array:
  
  - Assigning the new element to the array's current length: `arr[arr.length] = element`
  
  - Using the `push` method: `arr.push(element)`

- There are two ways to delete an existing element from an array:
  
  - Using the `delete` operator: `delete arr[elementIndex]`
  
  - Using the `splice` method: `arr.splice(startIndex, numberOfElements)`

- The `typeof` operator reports that the type of an array is `'object'`. So to distinguish between arrays and objects:
  
  ```javascript
  var is_array = function (value) {
      return value && 
          typeof value === 'object' &&
          typeof value.length === 'number' &&
          typeof value.splice === 'function' &&
          value.constructor === Array &&
          !(value.propertyIsEnumerable('length'));
  };
  ```

### Chapter 7: Regular Expressions

- A regular expression is the specification of the syntax of a simple language. Regular
  expressions are used with methods to search, replace, and extract information from strings.

- The rules for writing regular expressions can be surprisingly complex because
  they interpret characters in some positions as operators, and in slightly different
  positions as literals. Worse than being hard to write, this makes regular expressions hard to read and dangerous to modify.

- The `^` character indicates the beginning of the string.

- The `(?:...)` indicates a noncapturing group. The suffix `?` indicates that the group is optional. It means repeat zero or one time.

- The `[...]` indicates a character class. This character class, `A-Za-z`, contains 26 uppercase letters and 26 lowercase letters. The hyphens `-` indicate ranges, from A to Z. 

- The `+` character indicates that the character class will be matched one or more times

- The suffix `{0,3}` indicates that the character(s) preceding it will be matched 0 or 1 or 2 or 3 times.

- The `\d` represents a digit character. And the `$` represents the end of the string.

- Using `^` and `$` to anchor the regular expression causes all of the characters in the text to be matched against the regular expression. Omitting the anchors, the regular expression would tell us if a string contains the regexp. Including just the `^`,  it would match strings starting with the regexp. Including just the `$`, it would match strings ending with the regexp.

- The `i` flag causes case to be ignored when matching letters.

- There are two ways to make a RegExp object:
  
  - Using the regular expression literal notation which is enclosed in slashes.
  
  - Using the `RegExp` constructor which takes a string and compiles it into a RegExp object.

- The `\f` is the formfeed character, `\n` is the newline character, `\r` is the carriage return character, `\t` is the tab character, and `\u` allows for specifying a Unicode character as a 16-bit hex constant.
