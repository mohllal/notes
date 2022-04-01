# TypeScript Documentation

[Documentation Link](https://www.typescriptlang.org/index.html).

Date Started: Friday, October 25, 2019
Date Finished: TBD

## Tutorials

- ***Type annotations*** in TypeScript are lightweight ways to record the intended contract of the function or variable. TypeScript can offer ***static analysis*** based on both the structure of the code, and the type annotations.
- `tsconfig.json` is the default file name where the TypeScript compiler manages the project’s options, such as which files to include, and what sorts of checking to perform.
- Here are some configuration options examples:
  - `onEmitOnError`: Do not emit outputs if any errors were reported.
  - `noImplicitAny`: Raise error on expressions and declarations with an implied any type.
  - `sourceMap`: Generates corresponding .map file.
  - `target`: Specify ***ECMAScript*** target version to which newer JavaScript constructs will be translated down.
  - `compileOnSave`: Recompile the code automatically when it changes.
  - `outDir`: Emits all the output build files in the specified path.
  - `allowJs`: Accepts JavaScript files as inputs.
  - `noImplicitReturns`: Prevents from forgetting to return at the end of a function.
  - `noFallthroughCasesInSwitch`: Prevents from forgetting a `break` statement between `case`s in a switch block.
- When `strictNullChecks` is enabled, `null` and `undefined` get their own types called `null` and `undefined` respectively. Whenever anything is possibly `null`, use a union type with the original type.
- When `noImplicitThis` is enabled, TypeScript will issue an error when `this` is used without an explicit (or inferred) type. The fix is to use a `this`-parameter to give an explicit type in the interface or in the function itself.

## Handbook

### Basic Types:
- Basic types in TypeScript:
  - `boolean`: Simple true/false value.
  - `number`: Float-point number value.
  - `string`: Textual value.
  - `elemType[]`: Array of values such as `number[]`, `string[]`, etc..
  - `[elemType, elemType]`: Array (tuple) with a fixed number of elements whose types are known.
  - `enum`: Set of numeric values.
  - `any`: Any value, opt-out of type checking and let the values pass through compile-time checks.
  - `Object`: Object value, allow assigning any value to it. However will raise an error if a call to an arbitrary method has happened, even ones that actually exist.
  - `void`: Represents the absence of having any type at all. Only `null` or `undefined` can be assigned to `void` type.
  - `null` and `undefined`: Subtypes of all other types. That means assigning `null` and `undefined` to something like `number` is permitted.
  - `never`: Represents the type of values that never occur. The `never` type is a subtype of, and assignable to, every type; however, no type is a subtype of, or assignable to, `never` (except `never` itself).
  - `object`: Represents the non-primitive type, i.e. anything that is not `number`, `string`, `boolean`, `symbol`, `null`, or `undefined`.

- When using the `--strictNullChecks` flag, `null` and `undefined` are only assignable to any and their respective types (the one exception being that `undefined` is also assignable to `void`). This helps avoid many common errors. In cases where you want to pass in either a `string` or `null` or `undefined`, using the union type is the optimal solution as in `string | null | undefined`.

- Type assertions are a way to tell the compiler ***“trust me, I know what I’m doing”***. A type assertion is like a type cast in other languages, but performs no special checking or restructuring of data. Type assertions have two forms. One is the ***angle-bracket*** syntax and the other is the `as`-syntax.

### Variable Declarations:

- `var` declarations are accessible anywhere within their containing function, module, namespace, or global scope regardless of the containing block. This is called ***var-scoping*** or ***function-scoping***. Parameters are also function scoped.
- Variable declared using `let`, uses what some call ***lexical-scoping*** or ***block-scoping***. Unlike variables declared with `var` whose scopes leak out to their containing function, block-scoped variables are not visible outside of their nearest containing block or `for`-loop.
- Block-scoped variables is that they can’t be read or written to before they’re actually declared.
- The block-scoped variable just needs to be declared within a distinctly different block. The act of introducing a new name in a more nested scope is called ***shadowing***.
- `let` declarations have drastically different behavior when declared as part of a loop. Rather than just introducing a new environment to the loop itself, these declarations sort of create a new scope per iteration.
- `const` declarations are like `let` declarations but, as their name implies, their value cannot be changed once they are bound. Although the internal state of a `const` variable is still modifiable.
- There are three types of destructuring assignment:
  - ***Array destructuring***.
  - ***Tuple destructuring***.
  - ***Object destructuring***.
- The ***spread*** operator is the opposite of destructuring. Spreading creates a shallow copy of the N arrays/objects into a single one.
- Like array spreading, object spreading proceeds from ***left-to-right***, but the result is still an object. This means that properties that come later in the spread object overwrite properties that come earlier.

### Interfaces:

- One of TypeScript’s core principles is that *type checking focuses on the shape that values have*. This is sometimes called ***duck typing*** or ***structural subtyping***. 
- Interfaces with *optional properties* are written similar to other interfaces, with each optional property denoted by a `?` at the end of the property name in the declaration.
- Readonly properties are written by putting `readonly` before the name of the property. These properties will only be modifiable when an object is first created.
- *Object literals* get special treatment and undergo ***excess property checking*** when ***assigning them to other variables***, or ***passing them as arguments***. If an object literal has any properties that the *target type* doesn’t have, and error will be raised.
- Getting around ***excess property checking*** can be done by *adding a string index signature* if you’re sure that the object can have some extra properties that are used in some special way.
- Interfaces are capable of describing the wide range of shapes that JavaScript objects can take. In addition to describing an object with properties, interfaces are also capable of describing function types.
- For function types to correctly type check, the names of the parameters do not need to match. Function parameters are checked one at a time, with the type in each corresponding parameter position checked against each other.
- ***Indexable types*** have an *index signature* that describes the types we can use to index into the object, along with the corresponding return types when indexing. There are two types of supported index signatures: ***string*** and ***number***. It is possible to support both types of indexers, but ***the type returned from a numeric indexer must be a subtype of the type returned from the string indexer***. This is because when indexing with a number, JavaScript will actually convert that to a string before indexing into an object.
- When working with classes and interfaces, it helps to keep in mind that a class has two types: the type of the ***static side*** and the type of the ***instance side***. This is because when a class implements an interface, *only the instance side of the class is checked*. Since the constructor sits in the static side, it is not included in this check.
- An interface can extend multiple interfaces, creating a combination of all of the interfaces using the `extend` keyword.

### Classes:

- In TypeScript, we can use common object-oriented patterns. One of the most fundamental patterns in class-based programming is being able to extend existing classes to create new ones using inheritance.
- Each derived class that contains a constructor function must call `super()` which will execute the constructor of the base class. What’s more, before we ever access a property on this in a constructor body, we have to call `super()`.
- When a member is marked `private`, it cannot be accessed from outside of its containing class.
- The `protected` modifier acts much like the private modifier with the exception that members declared `protected` can also be accessed within deriving classes.
- Properties can be marked as *readonly* by using the `readonly` keyword. Readonly properties must be initialized at their declaration or in the constructor.
- ***Parameter properties*** are declared by prefixing a constructor parameter with an *accessibility modifier* or `readonly`, or both. Using `private` for a parameter property declares and initializes a *private* member; likewise, the same is done for `public`, `protected`, and `readonly`.
- ***Abstract classes*** are base classes from which other classes may be derived. They may not be instantiated directly. Unlike an interface, an abstract class may contain implementation details for its members. The `abstract` keyword is used to define *abstract classes* as well as *abstract methods* within an abstract class.
- Methods within an abstract class that are marked as *abstract do not contain an implementation and must be implemented in derived classes*. Abstract methods share a similar syntax to interface methods. Both define the signature of a method without including a method body.