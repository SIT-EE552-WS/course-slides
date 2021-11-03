# Lecture 10
## Serialization, Deserialization and Reflection
---
## Outline
- Enums
- Serialization and Deserialization - what, why and how?
- Annotations
- Reflection API

---
## Enums

- An `enum` type is a special data type that enables for a variable to be a set of
  predefined constants
- `enums` are fully-fledged classes, not just primitives

----
### Simple Example

```java
public enum Month {
  JANUARY,
  FEBRUARY,
  MARCH,
  APRIL,
  MAY,
  JUNE,
  JULY,
  AUGUST,
  SEPTEMBER,
  OCTOBER,
  NOVEMBER,
  DECEMBER;
}
```

----
### Simple Example

- You can use `enum`s the same way you use static final fields

```java
Month m1 = Month.NOVEMBER;

Month m2 = Month.NOVEMBER

System.out.println(m1 == m2); // prints true

```

- All `enum`s also give you a few methods for free

```java
System.out.println(
   Month.valueOf("OCTOBER") == Month.OCTOBER
); // also prints true

System.out.println
   Month.DECEMBER.ordinal()
); // prints 11

```
----

- Since `enum`s are classes, they can also have fields and constructors:

```java
enum Month {
  JANUARY("January","Jan."),
  FEBRUARY("February","Feb."),
  MARCH("March", "Mar."),
  APRIL("April", "Apr."),
  MAY("May", "May"),
  JUNE("June", "Jun."),
  JULY("July","Jul."),
  AUGUST("August","Aug."),
  SEPTEMBER("September","Sep."),
  OCTOBER("October", "Oct."),
  NOVEMBER("November","Nov."),
  DECEMBER("December", "Dec.");

  private final String name;
  private final String abbreviation;

  Month(String name, String abbreviation){
    this.name = name;
    this.abbreviation = abbreviation;
  }

  @Override
  public String toString(){
    return this.name + " (" + this.abbreviation + ")";
  }
}
```
----
- Since simple equality works, we can also use them in a `switch` statement

```java
  static String getSeason(Month month){
    String season;
    switch (month){
      case MARCH:
      case MAY:
      case APRIL:
        season = "Spring";
        break;
      case FEBRUARY:
      case JANUARY:
      case DECEMBER:
        season= "Winter";
        break;
      case OCTOBER:
      case SEPTEMBER:
      case NOVEMBER:
        season= "Fall";
        break;
      case JULY:
      case JUNE:
      case AUGUST:
        season = "Summer";
        break;
      default:
        season= "";
    }
    return season;
  }
```

---
## Serialization and Deserialization

----
### What...?

> In computing, serialization is the process of translating a data structure or
> object state into a format that can be stored or transmitted and reconstructed
> later.

Source: [Wikipedia](https://en.wikipedia.org/wiki/Serialization)

----

### Why...?

- Store the state of your program across restarts
- Send the state of your program to another developer
- Interact with distributed systems

----

### Distributed Systems?

![Distributed System](https://upload.wikimedia.org/wikipedia/commons/c/c6/Distributed-parallel.svg)

----

- Most Java computing runs on distributed systems nowadays
- Need some way to send the results of a computation / task performed on 
  one node to the next part of the system

----

### How...?

- Multiple possibilities
    - Native Serialization
    - XML
    - JSON
    - Protocol Buffers
    - ...and many more

---
## Annotations
- Annotations let you add metadata about your program to your code
- Many libraries you use will rely on annotations for various purposes

----
### Creating your own is also simple:

```java
@interface ClassPreamble {
   String author();
   String date();
   int currentRevision() default 1;
   String lastModified() default "N/A";
   String lastModifiedBy() default "N/A";
   // Note use of array
   String[] reviewers();
}
```

----
### Then to use the annotation:

```java
@ClassPreamble (
   author = "John Doe",
   date = "3/17/2002",
   currentRevision = 6,
   lastModified = "4/12/2004",
   lastModifiedBy = "Jane Doe",
   // Note array notation
   reviewers = {"Alice", "Bob", "Cindy"}
)
public class SomeClass {

// class code goes here

}
```
----

For more detail see this
  [tutorial](https://docs.oracle.com/javase/tutorial/java/annotations/index.html) from Oracle

---
## Reflection

> Inspect and/or modify your program at runtime
