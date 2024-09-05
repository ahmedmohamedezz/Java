# Strings, Characters, and Regular Expressions

- **_character literal_**: is an integer value represented as a
  character in single quotes
  - ex.\ `'z'`, `'\t'`
- the value of the character literal is it's integer value in the **_unicode charset_**

- **_string_** is a sequence of characters treated as a single unit

- string literals are stored as a `String` object

- to save memory, java treats all string literals with the same content as **_1 object_** with many references to it

---

### String Class

```java
// constructors
char[] chArr = {'b', 'i', 'r', 't', 'h', ' ', 'd', 'a', 'y'};
String s = new String("hello");

String s1 = new String();  // empty string assigned to string ref 's1'
String s2 = new String(s); // string with provided value
String s3 = new String(chArr);  // "birth day"
String s4 = new String(chArr, 6, 3);  // "day"
```

- it's not necessary to copy an existing string

```java
// methods
String s = "hello";
char[] chArr = new char[5];

sout(s.length()); // 5
sout(s.charAt(0)); // 'h'
s.getChars(0, 5, chArr, 0); // getChars(startIdx, cnt, new place, stIdx)
```

- the following examples demonstrate strings comparison

```java
String s = "hello";

s1.equals("hello");   // true, same content
s1 == "hello";  // false, not the same object (ref comparison)
s1.equalsIgnoreCase("Hello"); // true

s1.compareTo("hello");  // 0 => equal
s1.compareTo("z");    // -ve value => s1 smaller
s1.compareTo("a");    // +ve value => s1 greater

s1.startsWith("he");  // true
s1.startsWith("llo", 2); // true (str, idx)

s1.endsWith("lo");  // true
```

- locating chars & substrings

```java
String s = "hello";

s.indexOf('l'); // 2
s.indexOf("ell"); // 1
s.lastIndexOf('l'); // 3
s.indexOf('z');  // -1
```

### Extraction and Concatination

- `StringIndexOutOfBoundsException` is thrown if an out-of-bounds index is passed

```java
String s = "abcd";

sout(s.substring(2));   // [2, end] 'cd'
sout(s.substring(1, 2)) // [1, 2[   'b'

String r = s.concat("efg");
```

### Other Methods

- the following methods does not change the original string, they return new instances

```java
String s = " name ";

sout(s.replace('n', 'm'));  // mame
sout(s.replace("na", "me"));  // meme
sout(s.toUpperCase());  // NAME
sout(s.toLowerCase());
sout(s.trim());   // 'name': remove spaces at both ends
char[] charArr = s.toCharArray();
```

- as objects in java has a `toSting()` methods, a similar technique could be used with primitive types to be treated as strings using `String.valueOf()` method

```java
int x = 5;
char[] arr = {'h', 'i', '!'}
sout(String.valueOf(x));  // "x"
sout(Stirng.valueof(arr, 0, 2));  // "hi", from idx = 0, take 2 chars
```

- `valueOf()` method has 7 versions, it works with arguments of many types like: `boolean`, `char`, `int`, `long`, `float`, `double`, `object`

> you may see long value written like `long l = 23L` and float values like `float f = 2.5F`, this is because java by default treats integer literals as as 'integers' and floating point literals as 'double'

---

### String Builder Class

- string builder class represents modifiable string
- every string builder object can store a no. of chars know as it's **_capacity_**. if capacity is exceeded, it expands to accomodate new chars

- if your program doesn't involve many string changes, it's better to use `String` class because java can perform certain optimizations on strings (such refering to 1 object from multiple variables) as data will not change

- if you program frequently perform string modifications, use `StringBuilder` class

- `StringBuilder` is **NOT** thread safe, if multiple threads requires access to the same dynamic string information, use `StringBuffer` class which provides identical operations as `StringBuilder` but in a **_thread safe_** manner

### StringBuilder Constructors

- constructors

```java
StringBuilder r = new StringBuilder();   // default capacity = 16
StringBuilder y = new StringBuilder(20);   // capacity = 20
StringBuilder s = new StringBuilder("hello");   // capacity = len + 16

sout(s.length()); // "hello"
sout(s.capacity()); // 5 + 16 = 21
s.ensureCapacity(75);   // expand capacity to max(75, 2 * cur cap + 2)
s.setLength(2);   // "he"
```

- `ensureCapacity(x)` method will not affect the string capacity if the the arg 'x' is less than the current capacity

- dynamically increasing capacity can take relatively long time, which can **_degrade performance_** if it's applied multiple times. So, if your string will increase in size, multiple times, setting it's capacity as a large number at the beginning will **_increase performance_**

- `setLength(x)` method will trim the string if the argument is less than the current length, and will append `null` chars (numeric value = 0) to the end if the new length is greater than the current length

### Manipulations and Methods

```java
StringBuilder s = new StringBuilder("Hi");
sout(s.charAt(0));  // H
s.setCharAt(0, 'h');  // hi

char[] arr = new char[s.length()];
s.getChars(0, s.length(), arr, 0);    // (start1, end1 + 1, new container, start idx in container)

s.reverse();    // Modifies the string "ih"
```

- note that `reverse()` method changes the original string

- `StringBuilder` class uses the method `append()` to simulate '+' string concatination

```java
s.append(5).append(9L).append(2.4F).append(15).append(true)
  .append("str").append(charArr).append(charArr, 0, 3);
```

- since strings are immutable, java may perform the following

```java
String s = "Hello, ";
s += "world";

// java will do
s = new StringBuilder().append(s).append("world").toString();
```

- StringBuilder also provides methods for insertions & deletions

```java
StringBuilder s = new StringBuilder("Hi");

s.insert(2, '!');
s.deleteCharAt(2);  // Hi
s.delete(1, 2);   // H
```

- `insert()` method is overloaded to deal with various data types

---

### Character Class

- java provides a type-wrapper classes for primitive types

```java
char c = new Scanner(System.in).next();   // 'a'

sout(c.isDefined());  // true, if it's defined in the Uni-Code charset
sout(c.isJavaIdentifierStart());  // true, if it can be the first letter in a variable name
sout(c.isJavaIdentifierPart());  // true, if it can be in a variable name
sout(c.isDigit());  // false
sout(c.isLetter()); // true
sout(c.isLetterOrDigit()); // true
sout(c.isUpperCase()); // false
sout(c.toUpperCase());  // A
sout(c.isLowerCase());  // true
sout(c.toLowerCase());  // a
```

> there are common numbering systems like decimal (base 10), binary(base 2), octal (base 8), and hexadecimal(base 16), the base is aka the **_radix_**

- the following methods could be used to convert digits-to-chars & vice versa

  - used to get the corresponding character of a number in numbering systems

- radix must be in range [2, 36]

```java
sout(Character.forDigit(A, 16)); // 10
sout(Character.digit(13, 16));  // d
```

- Java automatically converts these char literals into `Character` objects, a process known as **_autoboxing_**

---

### String Tokenization and Regular Expressions

- when you read a sentence, your mind breaks it into **_tokens_**, a set of words and punctuation marks that convey meaning to you

- compilers also perform tokenization, they break sentences into identifiers, literals, and other programming-language elements

- tokens are separated by **_delimiters_**, typically white space chars (space, tab, new line, carraige return)

- we can define our own delimiters depending on the business need

- the string class `split()` method could be used for such a purpose

```java
String sentence = "how are you!";
sentence.split(" ");  // String[] => ["how", "are", "you!"]
```

### Regex

- a regular expression is a String that describes a search pattern for matching characters in other Strings

  - used for validating input
  - ensure data in proper format
  - validate the syntax of a program

- regex have predifined char classes like:

  - `\d`: any digit
  - `\D`: any non-digit
  - `\w`: any word
  - `\W`: any non-word
  - `\s`: any space char
  - `\S`: any non-space char

- regex quantifiers

  - `[a-zA-Z]`: char ranges from a-z and A-Z
  - `*`: 0, or more
  - `+`: 1, or more
  - `?`: 0, or 1
  - `.`: any char
  - `{x}`: exaclty x times
  - `{x, m}`: from x to m times
  - `{x,}`: at least x times
  - `x|y`: x, or y

- the simplest way to check if a string matches a pattern is to use the `matches()` method in the `String` class

```java
// zip code must be 5 digits
String zip = "12345";
sout(zip.matches("\\d{5}"));  // true

// vowel letters only
String vowel = "aaaaiiiooouu";
sout(vowel.matches([aeiou]+));  // true
```

- all quantifiers are greedy. they will match the longest possible sequence they can

- if a quantifier is followd by '?', it becomes **_reluctant_** (aka lazy)

### Replacing and Splitting

- use `String` class methods `replaceFirst()`, `replaceAll()`, `split()` for splitting and replacing strings using regex

```java
String sentence = "*** ***";
String res = sentence.replaceAll("\\*", "^");  // (regex, new str)
System.out.println(res);  // "^^^ ^^^"
```

- note that method `replace(str|char, str|char)` accepts a string not a regex, but `replaceFirst(regex, str)`

- `CharSequence` (package java.lang) is an interface that allows read access to a sequence of characters. The interface requires that the methods charAt, length, subSequence and toString be declared

  - Both `String` and `StringBuilder` implement interface

- a regular expression can be tested against an object of any class that implements interface `CharSequence`, but the regular expression must be a `String`. attempting to create a regular expression as a `StringBuilder` is an **_error_**

---

### Readings

- **_Pattern_** and **_Matcher_** Classes
