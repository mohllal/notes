# Clean Code: A Handbook of Agile Software Craftsmanship

By: Robert C. Martin. [Purchase the book](https://www.amazon.com/Clean-Code-Handbook-Software-Craftsmanship/dp/0132350882)!

Date Started: Friday, August 30, 2019

Date Finished: ongoing

> Writing clean code is what you must do in order to call yourself a professional.
> There is no reasonable excuse for doing anything less than your best.

## Chapter 1: Clean Code

- The LeBlanc’s law: Later equals never.
- Spending time keeping your code clean is not just cost effective; it’s a matter of
  professional survival.
- > We complain that the requirements changed in ways that thwart the original design. 
  >
  > We bemoan the schedules that were too tight to do things right.
  > We blather about stupid managers and intolerant customers and useless marketing types and telephone sanitizers.
  >
  > But the fault, dear Dilbert, is not in our stars, but in ourselves.
  > We are unprofessional.
- Most managers want good code. They may defend the schedule and requirements with passion; but that’s their job. It’s your job to defend the code with equal passion.  It is unprofessional for programmers to bend to the will of managers who don’t understand the risks of making messes.
- The only way to make the deadline—the only way to go fast—is to keep the code as clean as possible at all times.
- Bad code tempts the mess to grow! When others change bad code, they tend to
  make it worse.
- > A building with broken windows looks like nobody cares about
  > it. So other people stop caring. They allow more windows to become broken. Eventually they actively break them. They despoil the facade with graffiti and allow garbage to collect. One broken window starts the process toward decay.
- Clean code is code that has been taken care of. Someone has taken the time to keep it simple and orderly. They have paid appropriate attention to details. They have cared.

## Chapter 2: Meaningful Names

- Names should reveal intent. Names should tell us why it exists, what it does, and how it is used.
- Avoid using words whose entrenched meanings vary from our intended meaning.
- Avoid using *lower-case L* or *uppercase O* as variable names, especially in combination. The problem, of course, is that they look almost entirely like the constants one and zero, respectively.
- Noise words are redundant. The word variable should never appear in a variable name. The word table should never appear in a table name.
- Single-letter names may only be used as local variables inside short methods. The length of a name should correspond to the size of its scope.
- > One difference between a smart programmer and a professional programmer is that the professional understands that *clarity is king*. Professionals use their powers for  good and write code that others can understand.
- Classes and objects should have noun or noun phrase names. Methods should have verb or verb phrase names.
- Shorter names are generally better than longer ones, so long as they are clear. Add no
more context to a name than is necessary.

- The golden rules for naming:
  - Use intention-revealing names
  - Avoid disinformation
  - Make meaningful distinctions
  - Use pronounceable names
  - Use searchable names
  - Avoid encodings
  - Avoid mental mapping
  - Pick one word per concept
  - Use solution domain names
  - Use problem domain names
  - Add meaningful context
  - Don’t add gratuitous context

## Chapter 3: Functions

- The first rule of functions is that they should be small. Functions should hardly ever be 20 lines long.
- The indent level of a function should not be greater than one or two.
- > FUNCTIONS SHOULD DO ONE THING. THEY SHOULD DO IT WELL. THEY SHOULD DO IT ONLY.
- The top-down set of TO (To Include) paragraphs is an effective technique for keeping the abstraction level of a function consistent.
- Bury the switch statement in the basement of an abstract factory and never let anyone see it.
- The smaller and more focused a function is, the easier it is to choose a descriptive name.
- Arguments are hard from a testing point of view. Testing that all the various combinations of arguments are working properly can be a mess.
- In the *event* form of a function there is an input argument but no output argument. The overall program is meant to interpret the function call as an event and use the argument to alter the state of the system.
- If a function is going to transform its input argument, the transformation should appear as the return value.
- Functions that take more than two or three arguments, they are likely that some of those arguments ought to be wrapped into a class of their own.
- Passing a boolean into a function is a truly terrible practice. It's loudly proclaiming that this function does more than one thing. It does one thing if the flag is true and another if the flag is false!
- > Anything that forces you to check the function signature is equivalent to a double-take. It’s a cognitive break and should be avoided.
- Functions should either do something or answer something, but not both.
- *Command Query Separation* is a good mechanism to separate the command from the query so that the function's ambiguity cannot occur.
- Functions should do one thing. Error handing is one thing. Thus, a function that handles errors should do nothing else.
- Edsger Dijkstra’s rules of structured programming: every function, and every block within a function, should have one entry and one exit.

## Chapter 4: Comments

- The proper use of comments is to compensate for our failure to express ourself in code. Note that I used the word failure.
- Comments don’t always follow the changes of code-they can’t always follow them.
- It's a way too better to put energy go toward making the code so clear and expressive that it does not need the comments in the first place.
- > Truth can only be found in one place: the code. Only the code can truly tell you what it does. It is the only source of truly accurate information.
- Some good comments examples:
  - Legal comments
  - Informative comments
  - Explanation of intent comments
  - Warning of consequences comments
  - TODO comments
  - Amplification comments
  - Public API comments

- Some bad comments examples:
  - Mumbling comments
  - Redundant comments
  - Misleading comments
  - Mandated comments
  - Journal comments
  - Noise comments
  - Position markers comments
  - Closing brace comments
  - Commented-Out code comments
  - Nonlocal information comments

## Chapter 5: Formatting

- Code formatting is important. It is too important to ignore and it is too important to treat religiously. Code formatting is about communication, and communication is the professional developer’s first order of business.
- > Your style and discipline survives, even though your code does not.
- Blank lines that separate concepts or modules are extremely helpful. Each blank line is a visual cue.
- Lines of code that are tightly related should appear vertically dense.
- Concepts that are closely related should be kept vertically close to each other. Closely related concepts should not be separated into different files.
- Some rules to minimize the vertical distance of code:
  - Variables should be declared as close to their usage as possible.
  - Instance variables, on the other hand, should be declared at the top of the class.
  - If one function calls another, they should be vertically close, and the caller should be above the callee, if at all possible

## Chapter 6: Objects and Data Structures

- Hiding implementation is about abstractions! A class does not simply push its variables out through getters and setters, it exposes abstract interfaces that allow its users to manipulate the essence of the data, without having to know its implementation.
- > Procedural code (code using data structures) makes it easy to add new functions without changing the existing data structures. OO code, on the other hand, makes it easy to add new classes without changing existing functions.
- > Procedural code makes it hard to add new data structures because all the functions must change. OO code makes it hard to add new functions because all the classes must change.
- In case the system needs to add new data types rather than new functions, objects and OO are most appropriate. On the other hand, there will also be times when we’ll want to add new functions as opposed to data types. In that case procedural code and data structures will be more appropriate.
- *Law of Demeter*: a module should not know about the innards of the objects it manipulates. An object should not expose its internal structure through accessors because to do so is to expose, rather than to hide, its internal structure.
- Data transfer object is a class with public variables and no functions. It's very useful structures, especially when communicating with databases or parsing messages from sockets.
- Active Records are special forms of DTOs. They are data structures with public variables; but they typically have navigational methods like save and find.
- Never put business rule methods inside active records or DTOs because it creates a hybrid between a data structure and an object.

## Chapter 7: Error Handling

- In a way, `try` blocks are like *transactions*. Your `catch` has to leave your program in a consistent state, no matter what happens in the `try`. For this reason it is good practice to start with a `try-catch-finally` statement when you are writing code that could throw exceptions.
- ***Checked Exceptions***: The signature of every method would list all of the exceptions that it could pass to its caller. Moreover, these exceptions were part of the type of the method.
- Consider the calling hierarchy of a large system. *Functions at the top call functions below them, which call more functions below them, ad infinitum*. Now let’s say one of the lowest level functions is modified in such a way that it must throw an exception. *If that exception is checked, then the function signature must add a throws clause*. But this means that every function that calls our modified function must also be modified either to catch the new exception or to append the appropriate throws clause to its signature. Ad infinitum. It is an *Open/Closed Principle* violation.
- *Wrapping* third-party APIs is a best practice. When you wrap a third-party API, you minimize your dependencies upon it: You can choose to move to a different library in the future without much penalty. Wrapping also makes it easier to mock out third-party calls when you are testing your own code.
- The ***Special Case Pattern***. You create a class or configure an object so that it handles a special case for you. When you do, the client code doesn’t have to deal with exceptional behavior. That behavior is encapsulated in the special case object.
- Some things we do that invite errors:
  - Returning a `Null`.
  - Passing a `Null`.
- In most programming languages there is no good way to deal with a `null` that is passed by a caller accidentally. Because this is the case, the rational approach is to forbid passing `null` by default. When you do, you can code with the knowledge that a `null` in an argument list is an indication of a problem, and end up with far fewer careless mistakes.

## Chapter 8: Boundaries

- If you use a *boundary interface* like `Map`, keep it inside the class, or close family of classes, where it is used. Avoid returning it from, or accepting it as an argument to public APIs.
- ***Learning tests*** verify that the third-party packages we are using work the way we expect them to. Once integrated, there are no guarantees that the third-party code will stay compatible with our needs. Without these ***boundary tests*** to ease the migration we might be tempted to stay with the old version longer than we should.
- The ***Adapter Pattern*** encapsulated the interaction with the API and provides a single place to change when the API evolves.
- Code at the boundaries needs *clear separation* and tests that define expectations. We should avoid letting too much of our code know about the third-party particulars. *It’s better to depend on something you control than on something you don’t control*, lest it end up controlling you.
- We manage third-party boundaries by *having very few places in the code that refer to them*. We may *wrap them* as we did with Map, or we may use an *Adapter Pattern* to convert from our perfect interface to the provided interface.