# Arrays & ArrayLists

- array objects are data structures consisting of related items of the same type

- note that arrays index must be int, or (type promoted to integer)

```java
long i = 0;
int[] arr = {1, 2, 3, 4, 5};
for(; i<arr.length; ++i) {
    // compilation error (required int, found long)
    System.out.println(arr[i]);
}
```

- array object are created with the keyword `new`, each element has a default value
  - 0 for numeric primitive types
  - false for boolean
  - null for references

```java
int[] arr;   // declaration
arr = new int[10];  // creation (now `arr` stores an array refernce)

// multiple references once
String[] s1 = new String[3], s2 = new String[5];

// you can put the [] after the array name
boolean vis[] = new boolean[100];

int[] a, b, c; // all of them are array references
int d[], e, f; // only d is an array reference (e and f are integers)

// array initializer
int[] n = {10, 20};
```

> note that only final variable can't be modified after their first initialization

```java
public class Test {
  static int x;

  public void fun() {
    final int f;
    f = 5;    // ok
    f = 6;    // compilation error

    // the following statement won't cause an error
    System.out.println(Test.x);   // takes a default value (not an error)
    Test.x = 3;
    Test.x = 4;
    Test.x = 5;
  }


  final int f2;
  System.out.println(f2);   // compilation error: may not be initialized
}
```

- accessing an index outside the array will through `ArrayIndexOutOfBoundsException`

> to make your code fault-tolerant & robust, you may handle exceptions that may happen using try{} catch(Exception e){} block

> 1 try block can be followed by several catch blocks

- if you call a method on a null reference, you get `NullPointerException`

- you can iterate over array elements using enhanced for statement

```java
int[] freq = {1, 2, 3, 4, 5};

for(int e: freq)
    System.out.println(e);  // 1 2 3 4 5

for(int e: freq)
    e = -1;   // won't affect array elements
```


