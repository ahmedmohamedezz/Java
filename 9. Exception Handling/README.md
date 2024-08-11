# Exception Handling

- an exception is an indication of problem occurred during the program execution

- all classes that extend `Throwable` can be used with exception handling

- if we have a program that takes 2 numbers, and divide them

```java
  Scanner scanner = new Scanner(System.in);
  int n = scanner.nextInt();
  int m = scanner.nextInt();

  System.out.println(n/m);
```

- if you tried to enter (m = 0), you will get a `ArithmeticException: / by zero` exception

- if you entered a string values for any of the numbers, you will get `InputMismatchException`

- **_ArithmeticException_** can arise from a different problems, so a descriptive message is added to be specific

- java allows division by zero with floating point values, such a division will lead to +ve or -ve `Infinity`

- when dividing `0.0/0.0`, you get a `NaN`

- **_Infinity_** and **_NaN_** are represented in java as floating point values, but displays as those strings

- the following is the same example, but uses exception handling

- notice how we put the code that may through an exception, and the code that shouldn't be executed if exception occured in the `try`

- `throws` keyword indicates the type of exceptions the method may throw

```java
public static int div(int a, int b) throws ArithmeticException {
    return a / b;
}

public static void main(String[] args) {
    Scanner scanner = new Scanner(System.in);
    boolean notProperInput = true;
    do {
        try {
            int x = scanner.nextInt();
            int y = scanner.nextInt();
            System.out.println(div(x, y));
            notProperInput = false;
        } catch (InputMismatchException inputMismatchException) {
            System.err.printf("%nException: %s%n", inputMismatchException);
            // invalid input is still in the scanner's buffer
            scanner.nextLine(); // discard invalid input so user can try again
            System.out.printf("You must enter integers. Please try again.%n%n");
        } catch (ArithmeticException arithmeticException) {
            System.err.printf("%nException: %s%n", arithmeticException);
            System.out.printf("Zero is an invalid denominator. Please try again.%n%n");
          }
    } while (notProperInput);
}
```

- in java 7 and later, you can use the multi-catch feature if the blocks of catches is identical

- syntax: `catch(Type1 | Type2 | Type3 e)`

```java
try {
  // ...
} catch (InputMismatchException | ArithmeticException e) {
  // ...
}
```

- in a program with 1 thread, uncaught exception will terminate the program
- in a multi-threaded program, uncaught exception will **_only_** terminate the thread that caused the exception. so, if threads depend on each other, this may cause problems

- **_termination model of exception handling_**: in a try block, when an exception occurrs the try block **_terminates_** and control goes to catch blocks. and after that control **_doesn't_** return to the through point because the try block has **_expired_**

- when a try block expires, it's local variables have been lost and go out of scope (no longer accessible). and if there are references to objects, they are also marked for garbage collection

- other langaugse use another model called _resumption model_: after handling exception, control resumes after the throw point

### Hierarchy

- java class `Throwable` is the parent class of both `Error` and `Exception`

- `Error` is concerned with abnormal situations that happen in the JVM (not our business in exception handling)

- `Exception` is the parent class of all exceptions in java

- all `RuntimeException` subclasses are called **_unchecked exceptions_**, java ditinguishes between checked and un-checked exceptions because the compiler enforces a special requirements for cheched exceptions

- classes that inherit directly or indirectly from class Error (Fig. 11.4) are unchecked, because Errors are such serious problems that your program should not even attempt to deal with them.

- a common error is that when a subclass method overrides a superclass method that the subclass methods can't add more **_checked exceptions_** in it's `throws` clause than the superclass method

```java
class Parent {
    public int div(int a, int b) throws ArithmeticException {
        return a / b;
    }
}

class Child extends Parent {
  // Error
  //  @Override
    // public int div(int a, int b) throws ArithmeticException, IOException {
    //     return a / b;
    // }

  // however the following will not cause an error
  public int div(int a, int b) throws ArithmeticException, ArrayIndexOutOfBoundsException {
        return a / b;
    }
}
```

- because `ArrayIndexOutOfBoundsException` is an **_unchecked exception_**, it doesn't follow the same rules as checked exceptions

- unchecked exceptions are not required to be included in `throws` clause

- the 2 following are the same

```java
  // unchecked 'ArithmeticException' can be prevented by proper coding
  public int div(int a, int b) throws ArithmeticException {
      return a / b;
  }

  public int div(int a, int b) {
      return a / b;
  }
```

- you can't catch the same exception in more than 1 catch clause

- only the first matching catch block is executed

- catching `Exception` is discouraged, throw and catch more specific exceptions

```java
  // Compilation Error
  catch (ArithmeticException arithmeticException) {
    // ...
  }
  catch (ArithmeticException arithmeticException) {
    // ...
  }


  // Error since InputMismatchException is caught in the first catch block
   catch (Exception e) {

  } catch (InputMismatchException inputMismatchException) {

  }

  // Valid & recommended for error prevention (if you forgot to catch another subclass the general Exception will caught it)
  catch (InputMismatchException inputMismatchException) {

  } catch (Exception e) {

  }
```

---

### Finally block & Resource release

- programs must return resources acquired to the system to avoid **_resource leak_**

- in c & c++ the most common leak is memory leak. in java, the JVM performs auto garbage collection of memory no longer used

- other types of resources leak can occur like files, DB, and network connections that are not closed properly may not be available for use in other programs

- the finally block is block comes after all catch blocks, and always execute

  - no problem -> executes after try block
  - catched exception -> executes after catch block
  - not-catched exception -> still executes

- the finally block is an ideal place for resource releasing

- you can throw exceptions if you need for futher processing

- in the following code both `finally` clauses will always execute
- even when we throw exception in m1, the finally block in m1 still executes

- when `toStirng()` method is called on any `Throwable` object, it displays the string passed to its constructor, or the class name if no string supplied

```java
public static void m1() throws Exception {
    try {
        System.out.println("1");
        throw new Exception("My Custom Exception in m1");
        } catch (Exception e) {
            System.out.println(e.toString());
        throw e;    // throw checked exception (must be included in method signature)
    } finally {
        System.out.println("Finally m1");
    }
}

public static void m2() {
    try {
        m1();
    } catch (Exception e) {
        System.out.println("Exception caught in m2");
    } finally {
        System.out.println("Finally m2");
    }
}
```

- you may throw exceptions from constructors if the supplied parameters are not valid. this can prevent creating objects with invalid state

- re-throwing an exception defers the handling of it to another catch block associated with outer try block

---

### Stack Unwinding

- when an exception is thrown but not handled in a method, an attempt to catch this exception is made in the next outer scope.

- this is known as **_stack unwinding_**, in which the method terminates and all its local variables became out of scope. and control goes to the statement that invoked the method with exception

```java
public static void method1() throws Exception {
  method2();
}

public static void method2() throws Exception {
  throw new Exception("Exception thrown from method2");
}


public static void main() {
  try {
    method1();
  } catch(Exception e) {
    // ...
  }
}
```

- because we're working with **_checked exception_**, you must either to declare in the method signature that it can throw exception, or catch it properly

- with **_unchecked exceptions_**, the program will compile. but it will run with unexpected results

---

### Chained Exceptions

- sometimes a method responds to an exception by throwing a different exception type that’s specific to the current application.

- when a catch block throws a new exception, the original one is lost

- java provides a way to keep track of the exception caused the new exception to be thrown by a mechanism called **_chained exceptions_**

```java
    public static void method1() throws Exception {
        try {
            method2();
        } catch (Exception e) {
            throw new Exception("Exception in method1", e);
        }
    }

    public static void method2() throws Exception {
        throw new Exception("Exception in method2");
    }

    public static void main(String[] args) {
        try {
            method1();
        } catch (Exception e) {
          // this line will tell you what caused the exception to be thrown
            e.printStackTrace();
        }
    }
```

- you can define your own exception. before you do, you need to check whether it should be a **_checked exception_** (clients are required to handle the exception), or not
  - by convention, all exception-classes names should end with `Exception`

---

### Preconditions and Postconditions

- **_preconditions_** of a method is all the requirements on

  - method parameters
  - any other expectations about the current state of a program

- such that, if all those requirements are met. the method should operate as expected and produce the expected result.

- **_postconditions_** describe the constraints on the return value and any other side effect the method may have

- when you define a method, you should document all its _preconditions_ & _postconditions_. and you should make sure that are the postconditions are met, if the preconditions are satisfied

- when one of the conditions is not met, methods usually throw exceptions

```java
// pre-conditions: given a index integer [0, len - 1]
// post-conditions: return char at index
sout(str.charAt(0));

// if the index condition is not met, the method will throw 'indexOutOfBoundsException'
```

---

### Assertions

- assertions are conditions that should be true at a particular point of time

- pre-conditions are assertions about the program state, when a method is called

- post-conditions are assertions about the program state, when a method returns

- the assert statement evaluate a boolean experssion. if it's false, it throws an `AssertionError`

- assertions reduce performance. in order to use them, you need first to enable them

```java
// form 1
assert (condition)

// form 2
assert (condition) : "message to be passed to the exception thrown"
```

- assertions is usually used during program development only. so users shouldn't encounter an exception like this when using your code and also you shouldn't catch `AssertionError` in your code

---

### Try-with-resource

- an alternative way to release the resources in the `finally` block is to use try-with-resources method

- the class object used must implement the interface `AutoCloseable`. only then before the try block terminates the object's `close()` method will be called to release any resources aquired by this object

```java
try (ClassName theObject = new ClassName()) {
  // use theObject here
} catch (Exception e) {
  // catch exceptions that occur while using the resource
}
```

- you can allocate multiple resource in the try statement separating them with `;`

- in a single-threaded application, an exception may lead to program termination, but in a multi-threaded application **_only_** the thread with exception will be terminated