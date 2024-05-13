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
