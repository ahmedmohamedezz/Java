# Methods

- methods help you to divide your program into small & simple pieces, so you can **_divide & conquer_** your program

- methods are declared within classes, which are grouped into packages that can be imported & used

- each method should be have 1 purpose (perform a single task efficiently)

- the name of the method should be expressive to what it does

> if you can't find a suitable name for a methods, maybe it needs to be broken down into smaller ones

- each method can use other methods to do it's job, those methods can use other methods too

  - each method don't know how the methods it calls do the job,
    all that matters that they do it efficiently, and give the result back (if any)

- some methods don't have to belong to specific object, instead to the class itself, they are called **_static_** methods
  - static methods are called directly from the class

```java
// all Math class methods are static
System.out.println(Math.sqrt(125.0));   // static method

// math class has a plenty of other methods like:
System.out.println(Math.abs(x));
System.out.println(Math.max(x, y));
System.out.println(Math.min(x, y));
System.out.println(Math.pow(b, p));
System.out.println(Math.floor(x));
System.out.println(Math.ceil(x));
```

- not only methods that can be defined static, variables can too `Math.PI`
- constant variables are defined to be `public final static`
  - `public`: can be accessed by any object outside the class
  - `final`: can't change it's value after initialization
  - `static`: can be accessed directly from the class without making objects

#### Why is method main declared static ?

- when the JVM executes, it attempts to call the main method, and at this point no objects have been created

---

#### why static methods can **_only_** access static methods & attributes of the same class ?

- recall that static methods are meant to belong to the class as a whole, while instance methods & variables should belong to specific object

- so if this could be done, we may face a situation in which a static method is called, but there is no object of this class is made to use the instance methods on it.

---

- methods in java can return only 1 value

> note: java evaluate expressions from left to right, this can lead to unexpected results when the expression has strings & numeric values

```java
  int y = 5;
  System.out.println("y + 2 is " + y + 2);  //  y + 2 is 52
  // to get right answer use (y + 2)
```

- non-static methods are called _instance methods_, while static ones are called _class methods_

- when a program calls a method, the method should know how to return to it's caller, so the return address of the calling method is pushed into the stack

- also each function call should have a place in memory for it's local variable (including parameters), this is also pushed to the stack as a portion of the method-call stack known as (**_stack frame_**, or **_activation record_**)

- memory is finit, so if too much method calls exist in the stack, a common error may happen known as **_stack overflow_**

- when a method expects a wider type of argument (double), and the caller passes a smaller compatible type (int) java automatically convert `int` to `double`, however the opposite situation will cause a compilation error so you must convert it manually

- if a local variable has the same name of a data member in the class, it **_shadows_** the data member

```java
public class Test {
  private int x = 0;
  private static final x = 20;
  public void shadowX() {
    int x = 5;
    System.out.println(x);  // 5
    System.out.println(this.x); // 0
    System.out.println(Text.x);  // 20
  }
}
```

---

#### Overloading

- you can declare several methods with the same name in the same class as long as they have different parameter sets

- when a call happen, the compiler chooses the appropriate version to execute

> 2 functions with the same name & parameter sets, but different return type is **_NOT_** acceptable in java

- the compiler distinguish between overloaded methods based on their **_signature_**

> method signature: method's name, (no., type, order) of parameters, but NOT it's return type

> note that if static member variables are not final, any object can override them, this may make unexpected errors as this affects other objects

```java
public class Fun {
  public static int x = 0;
  Fun(int x) {
    this.x = x;
  }
}

public class Main {
  public static void main() {
    System.out.println(Fun.x);   // 0
    
    Fun f = new Fun(20);
    System.out.println(Fun.x);   // 20

    Fun f = new Fun(50);
    System.out.println(Fun.x);   // 50
  }
}
```
