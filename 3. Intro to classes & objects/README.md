# 3. Introduction to Classes & Objects

- access modifiers controls whether other java objects & classes can access our classes & objects

- every public class in java must be in a file with the same name (with .java ext), otherwise a compilation error will occur

- naming conventions: all identifiers in java are named in the camelCase naming (classes start with uppercase letter)

- static methods can be called without making objects from the class

- static methods can only contain calls of other _static methods_

- you don't have to import a class in the same package to use it (they are imported implicitly)

- classes of the same package are considered to be in thier default package

- using private data members allows us to achieve **_data hiding_**
this can reduce errors & make you code more robust

- providing public setters & getters can protect your data, and provide extra functionality
    - setters can do checks against bad values
    - getters can return data in different forms (grade 90 => "A")

- instance variables (not primitive types) of a class has a default value of `null`
    - you can define them with initail values

> all primitive types are initialized to 0 (except boolean => false) automatically

- primitive types store 1 value at a time of the same type of the variable

- reference types store the object's location in computer memory

```java
// all reference variables are initialized to null
Account myAcc;  // null
String myName;  // null
int[] nums;     // null
boolean flag;   // false
char ch;        // 0 as int value, NOT character '0'

myAcc = new Account(); // now, it holds the memory address of the created object
```

- new keyword requests memory from the system to store new objects

- java's compiler will generate a default constructor for you, only if your class has **_no constructors at all_**

- constructors don't return values (not even `void`)

```java
public class Account {
    String name;
    double balance;

    // not treated as a constructor
    public void Account(String name, double balance) {
        this.name = name;
        this.balance = balance;
    }
}

public class Main {
  // Compilation error (compiler can't find a matching constructor)
  Account account = new Account("John Doe", 250.0);
}
```

- method's (local variables & parameters) are not initialized automatically, unlike class's instance variables

- instance variable of a class are compiled (exist) before methods are called, during execution, and after it's called

- declaring the `main` methods with the `static` keyword helps the **_JVM_** in locating & calling the main method


--- 

### Notes & Practises
- Replacing duplicated code with calls to a method that contains one copy of that code can
reduce the size of your program and improve its maintainability


- The below code is duplicated, and if we need to change the way we represent an account's info, we will modify several places

```java
public class Main {
  public static void main(String[] args) {
      Account a1 = new Account("John", 260.36);
      Account a2 = new Account("Mary", 1040.36);

      // a1 info
      System.out.printf("Account owner is %s", a1.getName());
      System.out.printf("Account balance is %.2f", a1.getBalance());


      // a2 info
      System.out.printf("Account owner is %s", a2.getName());
      System.out.printf("Account balance is %.2f", a2.getBalance());
  }
}
```

- Here is a better way to do the same thing, now we encapsulated the code within the `printAccountInfo` method

```java
public class Main {
  public static void main(String[] args) {
      Account a1 = new Account("John", 260.36);
      Account a2 = new Account("Mary", 1040.36);

      // a1 info
      printAccountInfo(a1);

      // a2 info
      printAccountInfo(a2);
  }
}
```

---

### Questions & Readings

- what's the difference between local & instance variables ?
    - local variables are declared inside the body of a method, and it's accessed only inside the method from the point it's declared until the end of the methods (or end of scope it's defined at)
    - instance variables are variable defines inside the class (out of it's methods bodies), and it's accessible by all the class methods

- what's the difference between 'parameter' & 'argument' ?
    - parameter is some information a method needs to perform it's task
    - argument is the actual value of the parameter (passed values) to the method

- what different ways can we prevent other methods from changing object data ?

- should we call the setters in the constructors or not ?