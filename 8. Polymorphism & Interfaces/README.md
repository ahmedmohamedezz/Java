# Polymorphism & Interfaces

- polymorphism enables you to “program in the general” rather than “program in the specific” 
  - allows you to process object that share the superclass, as if they were all objects of the superclass

- suppose we have superclass animal, and subclasses fish, and frog. the program will send the same message to all animals. each of which will perform the same task, but in it's own way
  - fish -> swim
  - frog -> jump

- polymorphism enables you to code for general, and let execution time environment handle the specifics

- polymorphism promotes extensibility, as the same msg is sent to all objects, and each behaves in it's own way
  - so, for new objects the interface won't change. they only implement their own behavior

- when you have a superclass reference that points to subclass object, the type of the referenced object, not the type of the variable,determines
which method is called (method of subclass is called)

- when a superclass variable references a subclass object, the compiler looks at the method calls. if they exist on the class, or in it's superclasses (direct, or indirect), it compiles the method call (no errors). and at runtime the object type determines which method to use.
    - parent method version, if it doesn't exist in the subclass
    - most specific version, if it exists in the subclass
    - this is called **_dynamic binding_** 

---

### Abstract Classes

- when we think of a class, we assume that programs will create objects of that type

- but sometimes, we create classes that will never be used to create objects. it's only purpose is to represent superclass in inheritance hierarchy. so other classes can inherit from it to share a common design

- those classes are called **_abstract classes_**

- abstract classes are incomplete, so their subclasses must declare the missing pieces to become **_concrete classes_**, from which we can create objects

- abstract classes usually contain one, or more abstract methods that define the behavior passed to all it's subclasses
  - it can contain NO abstract methods at all

- constructors & non-private static methods can't be declared abstract
  - **_constructors are not inherited_** at the first place, so no need to make abstract
  - static methods can't be overriden, so even that they can be inherited, we still can't modify them. so it doesn't make sense to make them abstract

- despite we can't instantiate objects of superclasses, we still can have variables of them that reference concrete classes, so we can manipulate subclasses polymoriphically. and we can also call static methods in abstract classes using abstract classes variables


