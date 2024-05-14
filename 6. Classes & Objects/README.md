# Classes & Objects

- recall that classes exist to simplify programming

- public methods of a class should be **_Client Oriented_**, clients of a class usually concerned about what the class can do, but not how

- private methods of a class are called **_utility methods_** of that class

- accessing a private class member outside the class => compilation error

- every object can access a reference to itself using `this` keyword

> note that in java, you can create more than 1 class / file, but ONLY 1 of them is _public_

> note that java conserves storage by keeping only 1 copy of each class method in memory, this copy is called by every object of that class (each has it's own copy of the class's instance variables)

- a good class is the one that has multiple constructors

- the class contructors can call each other

```java
public Time2(int hour) {
  this(hour, 0, 0); // calls another constructor
}
// Time2 constructor: hour, minute and second supplied
public Time2(int hour, int minute, int second) {
  if (hour < 0 || hour >= 24)
    throw new IllegalArgumentException("hour must be 0-23");

  if (minute < 0 || minute >= 60)
    throw new IllegalArgumentException("minute must be 0-59");

  if (second < 0 || second >= 60)
    throw new IllegalArgumentException("second must be 0-59");

  this.hour = hour;
  this.minute = minute;
  this.second = second;
}
```

> note that only constructors can call constructors, and it MUST be the first line in the constructor body

```java
// The following examples will yield a compilation error
public Time1() {
  this.hour = 15;
  this(x, 0, 0);  // must be the first line
}

public void fun() {
  this(0, 0, 0);  // methods can't call constructors
}
```

- if a class object has a refernce to an object of the same class, it can access all it's data even the **_private_** fields

```java
public class Time1 {
  private int hour;
  Time1 temp;
  Time1(int hour) {
    this.hour = hour;
    temp.hour = hour;    // no problem
  }
}
```

- to reduce the effects of change in your classes, let the class methods access setters & getters of the class instead of the data directly

  - if we changed the getters code, no need to change methods that access it's data

- java allows to create methods with the same name as the class, it may look like a constructor with return type (those can't be used as constructors)
  - better to avoid making methods with the same name as the class

> classes should never have **_public non-constant_** data, it's good to mark the public data as `public static final double PI = 3.14`

> don't make your data `final` if it may change later

- classes can have references to objects from another classes (composition)

---

### Enums

Each enum declaration declares an enum class with the following restrictions:

1. enum constants are implicitly final.
2. enum constants are implicitly static.
3. Any attempt to create an object of an enum type with operator new results in a compilation error.

```java
enum Book {
    // declare constants of enum type
    JHTP("Java How to Program", "2015"),  // calls the constructor
    CHTP("C How to Program", "2013"),
    IW3HTP("Internet & World Wide Web How to Program", "2012"),
    CPPHTP("C++ How to Program", "2014"),
    VBHTP("Visual Basic How to Program", "2014"),
    CSHARPHTP("Visual C# How to Program", "2014");

    private final String title;
    private final String copyrightYear;

    // enum constructor
    Book(String title, String copyrightYear) {
        this.title = title;
        this.copyrightYear = copyrightYear;
    }

    public String getTitle() {
        return title;
    }

    public String getCopyrightYear() {
        return copyrightYear;
    }
}
```

> note that **ENUM** Constants **MUST** be decalred before the instance variables, constructors, and methods

---

### Garbage Collection

- the **_JVM_** performs automatic garbage collection to reclaim the memory occupied by objects that are no longer used

- When there are no more references to an object, the object is eligible to be collected

- resource leaks other than memory leaks can also occur. For example, an app may open a file on disk to modify its contents—if the app does not close the file, it must terminate before any other app can use the file

---

### Static class members

- static class variables and methods exist, and can be used, even if no objects of that class have been instantiated

  - they’re available as soon as the class is loaded into memory at execution time

- a static method cannot access a class’s instance variables and instance methods, because a static method can be called even when no objects of the class have been instantiated

- for the same reason, the **_this_** reference cannot be used in a static method. the **_this_** reference must refer to a specific object of the class, and when a static method is called, there might not be any objects of its class in memory

> you can import static members of a class using **_static import_**

```java
import static java.lang.Math.PI;  // single static import
import static java.lang.Math.*;   // static import on demand

public class StaticImport {
  public static void main(String[] args) {
      System.out.println(PI);
  }
}
```

---

### Final Instance Variables

- you can use the keyword final to specify that a variable is not modifiable (i.e., it’s a constant) and that any attempt to modify it is an error

- final variables are usually initialized when they are declared
  - if not, they **must** be initialized in every constructor

---

### BigDecimal

- any application that requires precise floating-point calculations—such as those in financial applications—should instead use class **_BigDecimal_** (from package _java.math_)

```java
public class Interest {
    public static void main(String[] args) {
        BigDecimal money = BigDecimal.valueOf(1050.55);

        money = money.add(BigDecimal.ONE);
        money = money.multiply(BigDecimal.ONE);
        money = money.pow(2);

        System.out.println(NumberFormat.getCurrencyInstance().format(money));
    }
}
```

- BigDecimal also gives you control over how values are rounded—by default all calculations are exact and **no rounding** occurs. if you do not specify how to round BigDecimal values and a given value cannot be represented exactly—such as the result of 1 divided by 3, which is 0.3333333… , an **_ArithmeticException_** occurs.

- if you need a BigDecimal rounded to a specific digit, you can call BigDecimal method setScale
  - `money.setScale(2, RoundingMode.HALF_EVEN)`

