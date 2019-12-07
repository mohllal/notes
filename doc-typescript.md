# TypeScript Documentation

[Docs](https://www.typescriptlang.org/index.html)!

Date Started: Friday, October 25, 2019

Date Finished: ongoing

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