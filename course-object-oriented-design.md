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
- Class attributes are written as *attributeName*:*attributeDataType* = *attributeDefaultValue*
- Class operations are written as *operationName>* (*parameterDataType*): *returnDataType*
- Controlling the visibility of attributes or operations by putting `-` or `+` in front of the attribute or operation name. - means private accessibility and `+` means public accessibility.
- Avoid defining class that are entirely devoid of any behaviors.
- Class attributes are converted to instance variables and operations/behaviours are converted to instance methods.
- Instantiation is the process of creating an object from a class, allocating a memory block for it, initializing its attributes and then returning a reference to it.
- Constructor is a special method that exists to construct the object and it's called when the object is created.
- Destructor/finalizer is a special method that is called when an object is no longer need and it's being disposed off. It's often used to release resources attached to that object before it's being destroyed.
- Static/shared member of class means that a variable or a method that is shared across all objects of that class. It's sometimes called class-level variable or method and it can be accessed by the class name as *className*.*staticMemberName*.
- Static methods can only accessed static variables.
- Static members are shown in UML diagrams with an underline.

### Section 6: Inheritance and Composition

- Inheritance describes an *is-a* relationship.
- In UML, inheritance is represented with the *solid open arrow*. If one class (child class, or subclass) is pointed to the other class (parent class, or superclass) it means that it's inherited from it.
- Overriding refers to changing one of the inherited behaviors from the superclass.
- Inheritance announces itself, don't go looking for parent classes to your classes.
- Abstract class is exist purely for the sake of being inherited, so it is never instantiated.
- Abstract class is used to provide shared attributes and behaviors to other classes.
- Classes that can be instantiated are named concrete class.
- Interfaces can only contain method signatures but no method definitions. 
- Classes that implement an interface must define methods with the same names as these that are in the interface.
- In UML, interfaces are shown the same way as classes but with the *«interface»* tag.
- A *dotted arrow* from the class, that implements the interface, to the interface represents an interface implementation rather than inheritance.
- Favor using interfaces to provide formal list of methods to support rather than using inheritance.
- Program to an interface, not an implementation.
- Drawing any kind of line between objects represents some kind of action or interaction.
- Aggregation describes an *has-a* relationship. In UML, aggregation is represented with the *unfilled diamond*.
- Multiplicity indicators can be used to represent a *has-many* relationship.
- Composition is more specific form of aggregation, it implies ownership.
- In composition, when the owner object is destroyed so are the contained object.
- In UML, composition is represented with the *filled diamond*.
