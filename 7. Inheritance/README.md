# Inheritance

- using inheritance lets you use the members of existing classes, and also allows you to add new or modify thire capabilities

- the class you inherit from is called the superclass, and the derived is called the subclass

- the superclass can be a subclass of another class, which makes a class hierarchy

- in java, all classes extend the `Object` class directly, or indirectly

- java support only **_single inheritance_**, that is each class can have only **1** superclass

- the subclass can override the superclass methods to make them behave as desired in his context
  - and we can access both versions of the method

---

### Protected Access Modifier

- up-till now, we are familiar with `private`, and `public` modifiers.

- suppose the superclass want to make members that can be accessed in it's subclasses only

  - if they are private => can't be accessed
  - public => acccessed from anywhere

- here comes the role of protected members, which can be accessed in subclasses

- as the public members are inherited as public members, protected members follow the same pattern & they are also inherited as protected subclass members

- constructors are not inherited, however java require that the constructor of the direct superclass to be called as the **_first statement_** in the subclass constructor (explicitely, or implicitely)

  - explicitely: to pass parameters of the superclass
  - implicitely: if you don't call the constructor, java calls the default constructor (no argument constructor) if exists, otherwise => **_error_**

> note that you should't call a instance method in the constructor of the same method class

```java
public class Subclass extends Superclass {
  private double salary;
  Subclass(String name, int age, double salary) {
    super(name, age);   // call to the Superclass constructoe
    this.salary = salary;
  }

  @Override
  public String getName() {
    return super.getName() + " from subclass\n";
  }
}
```

- note that protected members can be accessed also in classes of the same package

```java
public class Otherclass {
    public static void main(String[] args) {
        Superclass superclass = new Superclass("lol");
        Subclass subclass = new Subclass("sub", 20);

        // Otherclass can access 'name' protected member in superclass
        System.out.println(superclass.name);
        System.out.println(subclass.name);
    }
}
```

- usually when you override a superclass method, preferred to use the `@Override` annotation to indicate that the method should override an existing superclass method

- using the annotation helps the compiler catch common error, like:

  - wrong method name (don't exist in superclass)
  - wrong no. & type of params => overloaded by mistake

- in either cases you will endup having new methods (different name, or overloaded version), and this is not what you want in this case

- if you tried to override a `public` method as `protected` or `private` => `compilation error`
  - methods should be overriden in the same access level

---

### Notes using protected members

- despite that **_protected_** members are useful in some cases, they may also cause problems in other cases. so it's preferred to use **_private_** members as needed

1. using protected members, the subclass can set it's value directly without a set method. which can cause _illegal argument value_

2. another problem is that subclasses are tied to superclass code, while they should depend only on superclass services (non-private members)

   - suppose the superclass changed the name of the protected member 'firstName' to 'name'
   - this change will break the code of all the subclasses as they depend on member with the old name (making the subclass **_fragile_**)

3. classes of the same package are able to access those protected members, which is not desirable (no need to give them such permissions)

---

### Notes

> advice, don't make protected variables in superclass as this will tie them to subclasses. make them private & provide non-private methods to manipulate them

> use protected methods when you want to provide a service to package classes & subclasses, but not for other clients

---

### Object class

- as mentioned, all classes inherit from `Object` class
- next we will know some of the Object class methods

1. equals: boolean method, compares contents of 2 objects
2. hashCode: hashcodes are int values used for high-speed storage and retrieval of information stored in a data structure that’s known as a hashtable

- this method is called within other methods like `toString()` method

3. toString: returns a String representation of an object

- by default return the package name + class name + hash value of object

4. wait, notify, notifyAll
5. getClass
6. finalize: protected method that is called by the garbage collector
  - used to reclaim object memory
  - it's not clear wheather or when this method will be called, so
    programmers avoid using it
7. clone: makes a **_shallow copy_** of the object
