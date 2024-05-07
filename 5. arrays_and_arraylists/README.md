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

---

### passing arrays to methods

```java
public static void modifyArray(int[] arr) {
  // enhanced for loop does NOT change the array
  for (int elem : arr)
      elem *= 2;

  // array elements are changed
  for (int i = 0; i < arr.length; ++i)
      arr[i] *= 2;
}

public static void main() {
  int[] temp = {1, 2, 3}
  modifyArray(temp);   // copy of temp array reference is sent to the method

  for(int e: temp)
    System.out.print(e);  // 2 4 6
}
```

- if we a method accepts an array element as argument
  - if the element of primitive type => copy of the value is sent
  - if the element of reference type => copy of the address is sent

> In java all arguments are passed by **_value_** (pass by copy)

- note that, with primitive types passed as arguments, a copy of their value is passsed, so any changes to this copy will NOT affect the original parameter

- the same works when you pass a reference, a copy of that reference is passed, but the point is using this copy you still can modify the object it refers to in memory

- the next example maybe confusing, but it will clear it out

```java
public static void modify1(int[] arr) {
  // manipulate the object stored in memory using a copy to it's refernce
  for (int i = 0; i < arr.length; ++i)
      arr[i] *= 2;
}

public static void modify2(int[] arr) {
  // this line MODIFIES the copy => no longer the same reference passed
  // now, we changed the ref copy to point to another array
  // note that the original reference still points to original array
  arr = new int[]{10, 20, 30};

  // any changes will not affect the original array, only the new array
  for (int i = 0; i < arr.length; ++i)
      arr[i] *= 2;
}

public static void main() {
  int[] temp = {1, 2, 3}
  modify1(temp);

  for(int e: temp)
    System.out.print(e);  // 2 4 6

  modify2(temp);

  for(int e: temp)    // no change
    System.out.print(e);  // 2 4 6
}
```

---

### Multi-dimensional arrays

- 2d arrays are treated as an array of arrays, every inner array is independent of the other, which means they can differ in lengths

- each element in the main array is a reference to 1d array

```java
int[][] grid = {{1, 2}, {3, 4, 5}};   // note that second row > first row

int[][] arr = new int[3][4];    // 3 row, each has 4 cols

// we can also control the no. of col in each row
int[][] b = new int[2][];   // 2 rows
b[0] = new int[3];    // 3 cols
b[1] = new int[6];    // 6 cols


// traverse
for(int i=0; i<grid.length; ++i) {
  for(int j=0; j<grid[i].length; ++j) {
    System.out.print(grid[i][j] + " ");
  }
  System.out.println();
}
```

---

### Variable length argument lists

- with variable-length argument lists, you can create methods that receive an unspecified number of arguments.

- this list can occure once in a parameter list & must be placed at the end

- you can try to achieve this functionality with method overloading, but this way is more consice

- java teats variable length arrays as an array (all elements of the same type)

```java
public double average(double ...numbers) {
  double total = 0;

  for(double d: numbers)
    total += d;

  return total / numbers.length;
}

System.out.printf("Average is %.1f%n", average(10, 20));
System.out.printf("Average is %.1f%n", average(10, 20, 30));
System.out.printf("Average is %.1f%n", average(10, 20, 30, 40));
```

> use `Integer.parseInt("50")` to get the integer value of a string

---

### Class `Arrays`

- java provides a class with the basic functionality you will need while using arrays, so you don't re-invent the wheel

- the `Arrays` class in `java.util.Arrays` provides a static methods like `sort`, `fill`, `equals`, `binarySearch`

- use `System.arraycopy(intArray, 0, intArrayCopy, 0, intArray.length)` to copy one array into another

```java
public static void displayArray(double[] array) {
    for (double elem : array)
        System.out.printf("%.2f ", elem);

    System.out.println();
}

public static void main(String[] args) {
    double[] doubleArray = {8.4, 9.3, 0.2, 7.9, 3.4};

    // sort array ACS
    Arrays.sort(doubleArray);

    displayArray(doubleArray);

    // fill with specific value
    Arrays.fill(doubleArray, 50.50);

    int[] intArray = {1, 2, 3, 4, 5, 6};
    int[] intArrayCopy = new int[intArray.length];

    // src array, startIdxInSrcArray, dest array, startIdxInDestArray, length (number of elements to copy)
    System.arraycopy(intArray, 0, intArrayCopy, 0, intArray.length);

    // true ? same elements, in the same order
    boolean eq = Arrays.equals(intArray, intArrayCopy);
    System.out.println(eq);

    int idx = Arrays.binarySearch(intArray, 5);
    int idx2 = Arrays.binarySearch(intArray, 1000);

    System.out.printf("pos of 5 : %d, pos of 1000 : %d", idx, idx2);
}
```

- note that `arr1.equals(arr2)` is the not the same as `Arrays.equal(arr1, arr2)`

  - arr1.equals(arr2): compares the _references_
  - Arrays.equal(arr1, arr2): compares the _contents_

- the `Arrays` class also has methods for sorting big arrays using processor multi-cores like `parallelSort` method

---

### Collections (ArrayLists)

- the Java API provides several predefined data structures, called collections, used to store groups of related objects in memory.

- up till now, you've seen arrays, and we discussed that it has a fixed size

- one of the java collections called `ArrayLists` in package `java.util` provides arrays that can dynamically change it's size at runtime

- collection class can be used with any type, using that _generic_ placeholder, classes like this are called **_generic classes_**

> only nonprimitive types can be used to declare variables and create objects of generic classes.
> However, Java provides a mechanism (known as **_boxing_**) that allows primitive values to be wrapped as objects for use with generic classes

```java
// uses the Integer wrapper to wrap int values to be teated as objects (primitives are not allowed to be used with generics)
ArrayList<Integer> integers;
```

- When you place an int value into `ArrayList<Integer>`, the int value is boxed (wrapped) as an Integer object, and when you get an Integer object from `ArrayList<Integer>`, then assign the object
  to an int variable, the int value inside the object is unboxed.

- here are some of the methods of `ArrayList` collection
  - add(e): add 'e' at the end of the array
  - add(i, e): add 'e' at index 'i'
  - clear(): remove all elements
  - contains(e): return true if 'e' exist
    - time complexity: O(n)
  - get(i): get element at index 'i'
  - indexOf(e): returns index of first occurence of 'e'
  - remove(e): remove first occurence of 'e'
    - if 'e' doesn't exist, the function will do nothing
  - remove(i): remove value at index 'i'
  - size(): returns the size of the array
  - trimToSize(): trims the array size to the current no. of elements

