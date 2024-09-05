# Files, Streams, and Object Serialization

- Java views each file as a sequential stream of bytes

- File streams can be used to input and output data as bytes or characters

  - Byte-based streams output and input data in its binary format—a char is two bytes, an int is four bytes, a double is eight bytes, etc

  - Character-based streams output and input data as a sequence of characters in which every character is two bytes—the number of bytes for a given value depends on the number of characters in that value

- Files created using byte-based streams are referred to as **_binary files_**, while files created using character-based streams are referred to as **text files**. Text files can be read by text editors, while binary files are read by programs that understand the file’s specific content and its ordering.

- `System.in`, `System.out`, `System.err` are a common stream objects that enable programs to take input, produce output or error messages

- each stream can be **_redirected_**. for example, the input stream can take input from console, or redirected to read input from files

  - same for output and error streams

- _Java SE 8_ introduces a new type of stream that’s used to process collections of elements (like arrays stream)

- Character-based input and output can be performed with classes **_Scanner_** and **_Formatter_**

---

### File & Directory Information

- Interfaces `Path` and `DirectoryStream` and classes `Paths` and `Files` are useful for retrieving information about files and directories on disk:

  - **_Path interface_**: Objects of classes that implement this interface represent the location of a file or directory. Path objects do not open files or provide any file-processing capabilities

  - **_Paths class_**: Provides static methods used to get a Path object representing a file or directory location

  - **_Files class_**: Provides static methods for common file and directory manipulations, such as copying files; creating and deleting files and directories; getting information about files and directories; reading the contents of files; getting objects that allow you to manipulate the contents of files and directories; and more

  - **_DirectoryStream interface_**: Objects of classes that implement this interface enable a program to iterate through the contents of a directory

> absolute path is the path from the root directory to the current file or directory, while a relative path is relative to another directory

- An overloaded version of Files static method get uses a URI object to locate the file or directory. A **_Uniform Resource Identifier_** (URI) is a more general form of the **_Uniform Resource Locators_** (URLs) that are used to locate websites.

```java
public static void main(String[] args) throws IOException {
        Scanner input = new Scanner(System.in);

        System.out.println("Enter file or directory name: ");
        // using the path interface to create a 'Path' class object
        // case doesn't matter (test.txt = TEST.txt)
        // extension matter (test != test.txt)
        Path path = Paths.get(input.next());
        if (Files.exists(path)) {
            System.out.println(path.getFileName());
            System.out.println(Files.isDirectory(path));
            // getLastTimeModified => return FileTime Object
            System.out.println(Files.getLastModifiedTime(path));
            System.out.println(path.isAbsolute());
            System.out.println(Files.size(path));
            System.out.println(path);   // file path
            System.out.println(path.toAbsolutePath());

            if (Files.isDirectory(path)) {
                // display directory contents
                DirectoryStream<Path> directoryStream = Files.newDirectoryStream(path);
                for (Path p: directoryStream) {
                    System.out.println(p);
                }
            }
        } else {
            System.out.printf("%s doesn't exist", path);
        }
    }
```

- if the `Files.exists()` method returned true, it is still possible that methods like `Files.size()` to throw IOException. because the file could be deleted after executing the method `exists()` for example.

  - **_IOException_**: unhandeled exception that needs to be added to the method signature

- separator chars: chars used to separate directories and files in a path

  - `\`: in windows
  - `/`: in linux, or macOS

- java handles both separators efficiently

  - `D:\java\projects/p1/src` works fine!

- the constant `File.separator` will tell you the suitable separator for the current OS, this could be useful in building strings that represent path info

- if the path is a hardcoded string, you may use double slashes `D:\\java` to avoid escaping characters => `D:\n` will not access file named `n` but escape the next character `n`

---

### Creating text files (sequential access files)

- write to file

```java
// if file exists, it will be truncated
private static Formatter output = new Formatter("clients.txt");
Scanner input = new Scanner(System.in);

while (input.hasNext()) {
    output.format(
        "%d %s %s %.2f%n",
        input.nextInt(),
        input.next(),
        input.next(),
        input.nextDouble()
    );
}

output.close();
```

> when 0 is passed to `exit()`, it means successful termination. any other value indicates error

- if the file is not closed explicitely, the OS will close it when the program terminates

- note that there are **_many ways_** to write & read files

- read from file

```java
private static Scanner input = new Scanner(Paths.get("clients.txt"));

// we can use try with resources
  // try (Scanner input = new Scanner(Paths.get("clients.txt"))) { ... }

try {
  while (input.hasNext()) {
    System.out.printf(
        "%-10d%-12s%-12s%10.2f%n",
        input.nextInt(),
        input.next(),
        input.next(),
        input.nextDouble()
    );
  }
} catch (NoSuchElementException elementException) {
  // should read int, instead a string exists
  System.err.println("File improperly formed. Terminating.");
} catch (IllegalStateException stateException) {
  // scanner is closed before data was input
  System.err.println("Error reading from file. Terminating.");
}


input.close();
```

- class Scanner doesn't allow re-positioning to the beginning of the file. if you want to go back, close the file and re-open it

---

### Updating Sequential Access Files

- when you update a seq-access file, you take the risk of destroying the whole file, because the new data may require a greater size.

- for ex., over-writing the recrod `300 Pam White 0.00` to change 'white' => 'washington' will destroy the balance information

- if the record is re-written at the same location, following records will be affected

- seq-access files are not usually updated in-place. instead, copy the prev records to a new file, write the modified record, copy the following records.

---

### Object Serialization

- with regular text files, data is written without any type information

  - whether '3' was an `int` value ? `double` ? `string` ?

- object serialization is used to read and/or write data to files, or over a network

- a serialized object is an object represented as a sequence of bytes that includes the object’s data as well as information about the object’s type and the types of data stored in the object

- after a serialized object has been written into a file, it can be read from the file and deserialized. that is, the type information and bytes that represent the object and its data can be used to recreate the object in memory

- classes `ObjectInputStream` and `ObjectOutputStream` which respectively implement the `ObjectInput` and `ObjectOutput` interfaces, enable entire objects to be read from or written to a stream (possibly a file).

- to use serialization with files, you pass another stream (initialize) to the constructor of those classes

- initializing stream objects with another stream objects is called **_wrapping_**

  - the new stream object being created wraps the stream passed as a constructor argument

- classes `ObjectInputStream` and `ObjectOutputStream` simply read & write byte-based representation of objects. they **_DO NOT_** know where to write bytes to, or read them from

  - this depends on the passed stream argument (file stream, ...)

- classes that implement `ObjectOutput` and `ObjectInput` interfaces define the methods `writeObject` which takes an object and writes its information to an output stream, and `readObject` which reads and returns a reference to object from an input stream

  - the object passed must implement the interface `Serializable`

- see the following example

```java
public class Account implements Serializable {
  private int account;
  private String firstName;
  private String lastName;
  private double balance;


}
```

- class `ObjectOutputStream` will not output an object unless it's serializable, thats why we tagged out `Account` class with `Serializable`

- interface `Serializable` is a **_tagging_** interafce, which means it has **NO** methods

- in a serializable class, every instance variable must be serializable, non-serializable instance variables must be declared `transient` to indicate that they should be ignored during the serialization process

- all primitive types are serializable
- for reference type variables, check the class documentation

  - Strings and arrays are serializable

- when using wrapped streams, closing the outermost stream also closes the wrapped stream as well

#### Writing serialized data to sequential access files

```java
public static void main(String[] args) {
    private static ObjectOutputStream output;
    output = new ObjectOutputStream(Files.newOutputStream(Paths.get("clients.ser")));

    Scanner input = new Scanner(System.in);

      System.out.printf("%s%n? ",
              "Enter account number, first name, last name and balance.",

      while (input.hasNext()) {
          try {
              Account record = new Account(input.nextInt(), input.next(), input.next(), input.nextDouble());
              // write whole object with 1 statement
              output.writeObject(record);
          } catch (NoSuchElementException noSuchElementException) {
              System.err.println("Invalid input. Please try again.");
              input.nextLine(); // discard input so user can try again
          } catch (IOException ioException) {
              System.err.println("Error writing to file. Terminating.");
              System.exit(1);
          }
          System.out.print("? ");
      }

      if (output != null)
          output.close();
}
```

#### Reading & de-serializing data from sequential access files

```java
public static void main(String[] args) {
    private static ObjectInputStream input;
    input = new ObjectInputStream(Files.newInputStream(Paths.get("clients.ser")));


    System.out.printf("%-10s%-12s%-12s%10s%n", "Account", "First Name", "Last Name", "Balance");

    try {
        while (true) {
            Account record = (Account) input.readObject();
            System.out.printf("%-10d%-12s%-12s%10.2f%n",
                    record.getAccount(), record.getFirstName(), record.getLastName(), record.getBalance());
        }
    } catch (EOFException endOfFileException) {
        // readObject() will throw this exception will it reaches EOF
        System.out.printf("%nNo More Records%n");
    } catch (IOException e) {
        System.err.println("Error reading from file. Terminating.");
        System.exit(1);
    } catch (ClassNotFoundException e) {
        // Method readObject throws a ClassNotFoundException if the class for the object being read cannot be located. This may occur if the file is accessed on a computer that does not have the class.
        System.err.println("Invalid object type. Terminating");
        System.exit(1);
    }

    if (input != null)
        input.close();
}
```

---

### Readings

- Streams

- Object Serialization

- `IllegalStateException` Exception

- Pipe Streams

- Filter Streams

- Data Streams

- Buffered Streams (enhanced IO operations => less communication with devices)

- BufferedReader & BufferedWriter

```

```
