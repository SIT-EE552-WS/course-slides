---
title: Lecture 3 - Strings and User Input/Output
---
# Lecture 3
## Strings and User Input/Output
---
## Outline
- Motivation
- What is a `String` in Java?
- How to Receive String Input Three Ways
- File Input/Output
- Processing Mouse Input/Output
---
## Motivation
- So far, we have written methods to handle
  various inputs, but then, we hard-code
  all the inputs in our `main` method
- If we want to change those inputs, we
  have to recompile the program each time
- That's very inconvenient
- I/O allows us to make decisions at **runtime**
  instead of **compile time**

---
## What is a String in Java?
- A `String` is an object
- A `String` is an **immutable** array of `char`

```java
String str = "abc";
```

is equivalent to:

```java
char data[] = {'a', 'b', 'c'};
String str = new String(data);
```
----
### Aside - Arrays

- An array is fixed-size container of values of a single type. 
- The size of the array is set when the array is created

```java
int[] someArray = new int[10]; // array of 10 integers

someArray[5] = 42; // setting the value of some element

System.out.println(someArray[5] + 1); // prints 43

System.out.prinln(someArray.length); // prints 10
```
----
### Aside - Arrays

- If you run out of room in an array, the only way to get 
  more room is to create a new array and copy the old values
  into it.

```java
// alternate way to create an array
int[] arr1 = {1, 2, 3, 4, 5}; 

arr1[6] = 6;
// Will throw this error: 
// java.lang.ArrayIndexOutOfBoundsException: 
//    Index 6 out of bounds for length 5

int[] arr2 = int[10];
System.arraycopy(
    someArr,       // array to copy from
    0,             // position to start copying
    arr2,          // array to copy to
    0,             // position to start pasting
    someArr.length // number of elements to copy
)

arr2[6] = 6; // works now



```
----
### Back to Strings...
- Remember, internally `char` is a numeric type
- The mapping of numbers to written characters is
  call the **character encoding**
- It's a good idea to _always_ consider character
  encoding when working with strings.
- [Example](https://www.rapidtables.com/code/text/ascii-table.html)
----
### Back to Strings...
- The `String` class defines many useful methods

```java [1|1,3|1,4|1,5|1,6|1,7|1,8]
String str = "Hello World";

str.length();         // 11
str.startsWith("H");  // true
str.endsWith("z");    // false
str.replace("e","a"); // "Hallo World"
str.toUpperCase();    // "HELLO WORLD"
str.split(" ");       // ["Hello", "World"]

```

- Remember that the string is immutable so `toUpperCase` and `replace` return
  new strings
- See the [Java API documentation](https://docs.oracle.com/en/java/javase/17/docs/api/java.base/java/lang/String.html) for a full list

----
### Multi-line Strings
- **Text Blocks** were introduced to Java in version 14.

```java

String longText = """
                  This is a string
                  that takes up
                  multiple lines.
                  """;
```
- Text blocks measure indentation based on the 
  left of the first triple quote
----
### Strings and Other Data Types
- You can convert other data types to `String` in 
  multiple ways.

```
String s1 = "" + 1;            // s1 = "1"
String s2 = String.valueOf(1); //s2 = "1"
```

- You can also convert from `String` to other 
  data types (as long as the content matches).

```java
int i = Integer.parseInt("1"); // i = 1
int j = Integer.parseInt("a"); // NumberFormatException 
```

----
### Testing for Equality
- Since strings are objects, you cannot use `==` to compare them

```java
String str1 = "Hello";
String str2 = "Hello";

str1 == str2 // may be false!
```

- Instead...use the `equals` method or even `equalsIgnoreCase`.  (We'll discuss that more in detail in a future lecture.)

```java 
String str1 = "Hello";
String str2 = "Hello";
String str3 = "Goodbye";

str1.equals(str2); // true
str1.equals(str3); // false
str1.equalsIgnoreCase("HELLO"); // true

```
---
## How to Receive String Input Three Ways

1. Command line arguments
2. `java.util.Scanner`
3. `java.io.InputStream`

----

### Command Line Arguments

- When a program has a main method, 
  and it is executed with the `java` command
  anything after the class name is separated 
  by spaces and passed in as an array.

```java
public class Hello {
  public static void main(String[] args){
    System.out.println("Hello, " + args[0]);
  }
}
```

```bash
$> java Hello Frank
Hello, Frank
```

----
### Scanners

- A [Scanner](https://docs.oracle.com/en/java/javase/16/docs/api/java.base/java/util/Scanner.html) is a simple text scanner which can parse primitive types and strings using regular expressions.

```java
Scanner sc = new Scanner(System.in);
String s = sc.next();  
boolean b = sc.nextBoolean();
byte bt = sc.nextByte();
int i = sc.nextInt();
// etc.
```

- By default, a scanner assumes tokens are separated by 
  new lines

----
### Input Streams
- Just as individual strings are ultimately represented
  as an array of bytes, inputs (and outputs) are 
  streams of bytes.
- A stream is an ordered, unbounded sequence
- [InputStream](https://docs.oracle.com/en/java/javase/17/docs/api/java.base/java/io/InputStream.html) is the base class

---
## File Input/Output
- Reading is getting data from a file
- Writing is putting data into a file
- We consume from `InputStream`s using `Reader`s
- We publish from `OutputStream`s using `Writer`s
----
### Reading Example
- The most basic file read looks like this especially
  in early versions of Java.

```java
FileReader src = new FileReader("/path/to/file.txt");
char[] buffer = new char[128];
int numChars = src.read(buffer);
```
- Managing buffers by hand is tedious, though so Java provides a
  [`BufferedReader`](https://docs.oracle.com/en/java/javase/17/docs/api/java.base/java/io/BufferedReader.html).

```java
BufferedReader src = new BufferedReader(
   new FileReader("/path/to/file.txt");
);

src.readLine(); //reads a single line from the file
```
----
### Reading Example
- Remember, though, that converting between bytes
  and strings without specifying encoding is dangerous
- Both of the previous examples would have worked
  in Java 1.1.  As of Java 7 and later, we can also do

```java
import java.nio.charset.StandardCharsets;

BufferedReader src = Files.newBufferedReader(
  Paths.get("/path/to/file.txt"),
  StandardCharsets.UTF_8
)
```
----
#### All of the previous examples will fail...
----
### Checked Exceptions
- Many things can go wrong with reading from files:
  - File doesn't exist
  - You don't have permission to read it
  - The disk had some error
  - etc.
- The designers of Java wanted to force programmers
  to treat these scenarios defensively
- Enter...the **checked exception**

----
### Checked Exceptions
- A `Throwable` is the superclass of all possible errors and exceptions
  in the language.
- We generally don't worry about `Error` in Java as application 
  developers
- We handle `Exception`s using the keywords `try` and `catch`
- Exceptions can be a `RuntimeException` or a checked exception
- Using `try...catch` for a `RuntimeException` is optional but 
  using it for checked exceptions is **required**
----
### Read a File without Compiler Errors

```java
try {
  BufferedReader src = Files.newBufferedReader(
    Paths.get("/path/to/file.txt"),
    StandardCharsets.UTF_8
  );
  String line = src.readLine();
} catch (IOException ioe){
  // do something to recover from the error
  ioe.printStackTrace();
}
```
----
### One more thing...

- The JVM and the Operating system keep track
  of which files are open
- On Windows, two programs usually can't open
  a file at the same time
- Can also also tie up system resources and 
  impact performance.

- **Always close your files when you're done with them**
----
### Closing files manually
```java
try {
  BufferedReader src = Files.newBufferedReader(
    Paths.get("/path/to/file.txt"),
    StandardCharsets.UTF_8
  );
  String line = src.readLine();
  src.close();
} catch (IOException ioe){
  // do something to recover from the error
  ioe.printStackTrace();
}
```
### But what if there's an exception?
----
- `try...catch` will short circuit right after
   the line where the error occurred

```java
try {
  BufferedReader src = Files.newBufferedReader(
    Paths.get("/path/to/file.txt"),
    StandardCharsets.UTF_8
  );
  String line = src.readLine();
} catch (IOException ioe){
  // do something to recover from the error
  ioe.printStackTrace();
} finally {
  src.close(); 
}
```
- Code in the `finally` block will always execute
  regardless of error or success
- ...but `src` is not available here

----
### Same example with corrected scope

```java
BufferedReader src;
try {
  src = Files.newBufferedReader(
    Paths.get("/path/to/file.txt"),
    StandardCharsets.UTF_8
  );
  String line = src.readLine();
} catch (IOException ioe){
  // do something to recover from the error
  ioe.printStackTrace();
} finally {
  if(src != null) {
    src.close(); 
  } 
}
```
----
#### This is why some folks hate Java...
----
#### But there's a better way
----
### Try-with-resources
- Any object that implements `java.lang.AutoCloseable` can be used as a resource.

```java 
try(BufferedReader src = 
  Files.newBufferedReader(
    Paths.get("/path/to/file.txt"),
    StandardCharsets.UTF_8
  );) {
  String line = src.readLine();
} catch (IOException ioe){
  // do something to recover from the error
  ioe.printStackTrace();
}
```
- AutoClosable means the resource is automatically
  closed for you
----
### Using Scanner with Files

```java 
try(BufferedReader src = 
  Files.newBufferedReader(
    Paths.get("/path/to/file.txt"),
    StandardCharsets.UTF_8
  );
  Scanner scanner = new Scanner(src);
  ) {
  int num = scanner.nextInt();
} catch (IOException ioe){
  // do something to recover from the error
  ioe.printStackTrace();
}
```
----
### Writing files is similar

```java
try(
 PrintWriter out = new PrintWriter(
   new BufferedWriter(
      new FileWriter("foo.out")
   )
 );
){
   out.println("Here is some text");
} catch (IOException ioe){
   ioe.printStackTrace();
}
```
---
## Processing Mouse Input/Output
---
