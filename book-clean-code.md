# Clean Code: A Handbook of Agile Software Craftsmanship

By: Robert C. Martin. [Purchase the book](https://www.amazon.com/Clean-Code-Handbook-Software-Craftsmanship/dp/0132350882)!

Date Started: Friday, August 30, 2019

Date Finished: ongoing

> Writing clean code is what you must do in order to call yourself a professional.
> There is no reasonable excuse for doing anything less than your best.

### Chapter 1: Clean Code

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

### Chapter 2: Meaningful Names

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

### Chapter 3: Functions

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
