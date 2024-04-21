# Chapter 1 : Java Fundamentals

### Protability

Java introduced an important feature that was missed at the time
it's created, which is the portability

Other languages like **c**, and **c++** are designed to compile for a specific machiene (target CPU), and _compilers_ are expensive & time consuming to create

Java is a platform-independent language that is used to create software to be embedded in various electronic devices

---

### Java & Internet

Java introduced **_applets_**, which is a programs that can be transmitted over the internet & automatically executed (download + run) by _java-compatible browser_

Applets are used to display server msgs, or provide simple functionality

> You may have heard about **_servlets_**, as applets runs & provides functionality on the client side, servlets also provide functionalities _but_ on the **_server side_**

---

### Bytecode

The java compiler doesn't provide executable file as other languages compilers, instead it generates a **_bytecode_**

> Bytecode is an optimized set of instructions executed by java virtaul machine (**_JVM_**)

---

### Object Oriented Programming (OOP)

One of the most common programming paradigms, that deals with data units (objects) instead of separating data of behavior

OOP covers 3 main concepts

1. Encapsulation

   - mechanism that binds together code & data, and separate them from outside interference (other code), those members(variables + methods) could be private to the object, or public
   - public methods usually used to provide interface to deal with
     private members

2. Polymorphism

   - used to provide a general interface for a set of related activities
   - expressed by **_one interface, multiple methods_**

3. Inheritance
   - process by which one object acquires the properties of another object
   - express hierarchical classification
   - without inheritance, each object would have to implement all it's properties (can't re-use parent's properties)

---

### Variables & Identifiers

A variable is named memory location, it's content can be changed dynamically

Names user gives to variables, methods, or any user-defined item

Identifier name can start with [a-zA-Z_$], then any char

- Can't start with number

---

Let's look at basic statement that is used to print something in the console & understand it:

```java
System.out.println("Hello, world!");
```

- System: is a predefined class that provides access to the system
- out: output stream that is connected to the console
- println(arg): used to print arg in console + line ending

> `System.out` is an object that encapsulates console output

