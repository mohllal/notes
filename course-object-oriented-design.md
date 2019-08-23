# Foundations of Programming: Object-Oriented Design

By: Simon Allardice. [Purchase the course](https://www.simonallardice.com/courses)!

Date Started: Monday, August 12, 2019

Date Finished: ongoing

### Section 4: Domain Modeling

- Creating a conceptual model is the process of identifying the main system's objects generic objects, and describing the associations and interactions between each other.

- Identifying objects:
  
  - Go through the system's use cases, user stories and any other written requirements and start picking out all the nouns to make a list of them.
  
  - Take a pass through the nouns list to find if there is any duplicates or attributes nouns and remove them.
  
  - Make a quick diagram of these objects by boxing each object's name.

- Describing associations and interactions between different object by just draw lines between the objects' boxes.

- Identifying responsibilities:
  
  - Go through the system's use cases, user stories and any other written requirements and start picking out all the verb phrases to make a list of them.
  
  - Identify not just what has to happen, but whose job it is! It's not about showing who is initiating the action but where the responsibility lies for performing it.

- CRC cards (for Class, Responsibilities, Collaborators) are usually written on index cards. Each CRC card represents one class and it has three seconds class name, class responsibilities, and collaborators.

### Section 5: Creating Classes

- Class diagram is divided into three sections class name, attributes, and operations.
- Classes are named in singular not plural, and they must start with a capital letter.
- Class attributes are written as *<attributeName>*:*<attributeDataType>* =*<attributeDefaultValue>*
- Class operations are written as *<operationName>* (*<parameterDataType>*): *<returnDataType>*
- Controlling the visibility of attributes or operations by putting `-` or `+` in front of the attribute or operation name. - means private accessibility and `+` means public accessibility.
- Avoid defining class that are entirely devoid of any behaviors.
- Class attributes are converted to instance variables and operations/behaviours are converted to instance methods.
- Instantiation is the process of creating an object from a class, allocating a memory block for it, initializing its attributes and then returning a reference to it.
- Constructor is a special method that exists to construct the object and it's called when the object is created.
- Destructor/finalizer is a special method that is called when an object is no longer need and it's being disposed off. It's often used to release resources attached to that object before it's being destroyed.
- Static/shared member of class means that a variable or a method that is shared across all objects of that class. It's sometimes called class-level variable or method and it can be accessed by the class name as *<className>*.*<staticMemberName>*.
- Static methods can only accessed static variables.
- Static members are shown in UML diagrams with an underline.
