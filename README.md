# Chapter 3 : Input & Control Statements

### Taking Input

To read input from the console, you need to define a scanner object

```java
Scanner input = new Scanner(System.in);

// we can read all data types except char

// Methods of scanner object
input.next();  // reads one word (no spaces)
input.nextLine();  // reads whole line (with spaces)
input.nextInt();
input.nextDouble();
input.nextFloat();
input.nextLong();
input.nextBoolean();
input.nextByte();
```

```java
// To read char
char c = (char) System.in.read();
c = scanner.next().charAt(0);
```

> We pass `System.in` to tell java that we will read from console, not (file, ...)

---

### Control Statements

Explanation will be through examples

```java
int x = 1;

if(x == 1)  // with or without { }
  System.out.println(1);

if(x > 1) { // nested if's
  if(x == 1)
    System.out.println(1);
  else if(x == 2)
    System.out.println(2);
  else {
    // ...
  }
}

// another way
switch (x) {
  case 1:
    // ..
    break;

  case 2:
    // ...
    break;

  default:
    // ...
}
```

> Before JDK7, expression in the switch statement must be (byte, short, int, char), after it => could also be a string

Java allows nested switch statements

---

### Looping

```java
for(init; condition; counter) {
  // ...
}

int sum = 0;
for(int i=1; i<=5; sum += i++)  // valid loop

// WHILE loop
while(condition) {
  // ...
}


// DO WHILE loop
do {
  // ...
} while(condition);
```

Break, and continue

```java
while(cond) {
  if(c1) // break from loop
    break;

  if(c2)
    continue; // skip iteration
}

// break as goto
for(int i=0; i<5; ++i)
one : {
    for(int j=0; j<5; ++j) {
      if(j > 2)
          break one; // go to 'one'
          System.out.println(j);  // repeat 0, 1, 2 'i' times
    }
}


// The following code won't compile because the one clause
// doesn't enclose the other loop
one: for(int i=0; i<3; i++) {
    System.out.print("Pass " + i + ": ");
}

for(int j=0; j<100; j++) {
    if(j == 10) break one; // WRONG
}
```

> When using break with a label, the label must be on a block that contains the break

> The else statement is associated with the last if

Look at the following

```java
// The else belongs to which if ? inner if

if (x == 1)
    if (y == 1) {
        System.out.println("inside 2 if's");
    }
else
    System.out.println("No");
```
