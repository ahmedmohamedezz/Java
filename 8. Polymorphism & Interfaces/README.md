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

```java
abstract class Parent {
  public abstract void introduceYourSelf();
}

class Student extends Parent {
  public void introduceYourSelf () {
      sout("Hello, world");
  }
}
```

- you can use the instanceof operator to know if an object lies in the inheritance tree of class

```java
if(employee instanceof Human) {
  // ...
}
```

- dynamic binding: the process of assinging the object subtype at execution time
- you can't assign an object of parent class to an object of child class
  - down casting ?

```java
abstract class Parent {
  // ....
}

class Child {
  // ....
}

psvm () { // public static void main
  Parent parentRef = new Child();

  // Child childRef = parentRef;   // compilation error
  Child childRef = (Child) parentRef;
}
```

- although the method call depende on the runtime type of the object, the variable can be used only to call methods of it's type

```java
public static void main(String[] args) {
        Shape[] arr = new Shape[2];
        arr[0] = new Circle();
        arr[1] = new Square();

        // variable of 'Shape' type can't call specific child classes methods without casting
        for (Shape sh : arr) {
            sh.draw();
            if (sh instanceof Square sq) {
                sq.squareOnlyMethod();
            }

            if (sh instanceof Circle c) {
                c.circleOnlyMethod();
            }
        }
    }
```

- we can make member variables `final`, so they can't be changed after they're populated with values
- we can also make methods `final`, which can't be overriden by subclasses

  - this guarantees that the method implementation will be used by all subclasses without changing it

- since the method code never changes, so all subclasses call the same code
- calls to `final` methods are resolved at compile time, aka **_static binding_**

- we can make classes `final`, final classes can't be extended as all methods are declared `final` implicitely

  - ex. `String` class is a final class

- the compiler can optimize programs by removing calls to final methods and inlining their ex
  panded code at each method-call location.

- don't call ovveridable methods in constructor, object is not fully initilazed yet

- it's common to call validation methods in constructors

  - if the validation code is short => it's okay to duplicate it
  - if not, define a static validation method (private helper method)

- you can call static methods from objects too

- we can change the value of static class member through a static member function

### Interfaces

- interfaces define and standardize the ways in which things such as people and systems can
  interact with one another.

- interfaces specify what operations can be done, but not how

- all methods of an interface are implicitely **_public_** and **_abstract_**
- all fields are implicitely **_public_**, **_final_**, and **_static_**

> so, when you name your function of variable in interface, you don't need to use keywords like public, abstract, final because they are redundant

- if a class doesn't declare all mehtods of an interface => must be declare **_abstract_**

- interfaces are often used with **_disparate classes_** (classes that are not related by a class hierarchy)

- interface is like a public class, and **_must_** be declared in a file with the same name

- the relationship between a class and an interface is called **_realization_** relationship. A class is said to implement or realize the methods of an interface

- in UML, interface are marked with `<<interface>>`, and abstract classes are named in italics _Employee_

- a class can extend only **_one_** parent class, but can implement **_many_** interfaces

```java
public class Child extends Parent implements One, Two {
  // ...
}
```

- some popular interfaces in the JAVA API

  - Comparable: used to add the functionality of comparing objects, not just primitive types
  - Serializable: used with object that can be written (serialized), or read (de-serialized) from a storage. or transmitted across network
  - Runnable: object of classes that implement this interface can execute in parallel using **_multi-threading_**

- java SE8 interface enhacements
  - method default implementation: not all methods abstract anymore
    - this can help in adding methods to existing interface without breaking the clients code
  - static interface methods
  - functional interfaces: interfaces with only 1 abstract method (Comparator, Runnable)

```java
interface Payable {
  double getPaymentAmount();
  default void def() {
    // ...
  }
}

class Invoice implements Payable{
  public double getPaymentAmount() {
    // ....
  }

  // no need to implement the default method 'def'
}
```

---

### Readings

- difference between **_static_** & **_final_** variables, methods, classes
