# Chapter 2 : Data Types & operators

### Data Types

Java doesn't have the concept of _type less_ variables

Java compiler does **_type checking_** to check for type compatibility

The type of variable determine what operations are allowed on it, and how much memory it should acquire

Java has 8 primitive types

- boolean: true\false
- byte: 8 bit integer
- char: character
- double: double precision floating point
- float: single precision floating point
- int: integer
- long: long integer
- short: short integer

Primitive means that those types are not object, they are normal _binary values_, and all other types are constructed from those types

> Those types are not an objects because of efficiency concerns

You may notice that there several numerical types in java, each has different memory size

- byte: 8 bits
- shor: 16
- int: 32
- long: 64
- float: 32
- double: 64

> Java doesn't support **_unsigned integers_**

Characters in java are not 8bit quantities like other languages, java uses **_Unicode_**

So, a char is represented as _unsigned 16-bit_ type

> Note that **_ASCII_** chars are a subset of **_Unicode_**, which means all ASCII chars are still valid in java

> Starting from Java7, you can use \_ in numeric values to increase it's readability `int x = 1_000_000`

> Java supports also assigning octal (0 + val) & hex (0x + val) & binary (0b + val) values to numerical variables `oct = 011` and `hex = 0xFF` and `0B1001`

> You're **not allowed** to create 2 variables with the same name in the same scope (or if one enclose the other)

---

### Operators

> `%` operator can be used with integer & floating point types

> note that `y = ++x` and `y = x++` are not the same

> `^` is the logical xor operator

> note that short-circuit operators `&&` and `||` will evaluate the second argument only if necessary, while `&` and `|` will always evaluate all operands

See the next example for illustration

```java
// expression (n % d) will cause division by zero exception if d = 0

int d = 0;
if(d != 0 && (n % d) == 0)   // prevents division by 0
  n /= d;

if(d != 0 & (n % d) == 0)   // cause division by 0
  n /= d;
```

> You can assign several variables in 1 statement `x = y = z = 100`, because `=` operator in java returns the value of the RHS of the expression, thus `z = 100` will return `100`

> Java will automatically cast the type of variable in assignment statement only if both types are compatible & destination are larger type, `intX = byteY`

> You can cast manually => `(int) (1.5 * x / y)`

> Java performs type promotion in expressions, it promot `char`, `byte`, `short` to `int`
> then to `long` ,`float`, `double` if one of the variables is one of those (cast all to the widest variable type & atleast set all to `int`)


