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
