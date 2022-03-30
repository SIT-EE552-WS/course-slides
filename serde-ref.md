---
title: Lecture 10 - Serialization, Deserialization and Reflection
---
# Lecture 10
## Serialization, Deserialization and Reflection
---
## Outline
- Serialization and Deserialization - what, why and how?
- Annotations
- Reflection API
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

----
### Native Serialization

- Native serialization is simple, but somewhat _dangerous_

```java
// From an Object to a File
try(
  FileOutputStream fos = new FileOutputStream("some-hero.ser");
  ObjectOutputStream oos = new ObjectOutputStream(fos);
){
  oos.writeObject(hero);
} catch (IOException e){
  e.printStackTrace();
}

// From a File to an Object
try (
  FileInputStream fis = new FileInputStream("some-hero.ser");
  ObjectInputStream ois = new ObjectInputStream(fis);
){
  Object o = ois.readObject();
  if( o  instanceof SuperHero ) {
      SuperHero hero = (SuperHero) o;
      System.out.println(hero);
      System.out.println(hero.name);
      System.out.println(hero.toV2());
  }
} catch (IOException | ClassNotFoundException e){
  e.printStackTrace();
}
```
----
### XML Serialization

- XML serialization is powerful but "old-fashioned"
- In latest versions of Java, depends on external libraries

```java

// From Object to File
XmlMapper mapper = new XmlMapper();
String xml = "";
try {
    xml = mapper.writeValueAsString(hero);
} catch (JsonProcessingException e) {
    e.printStackTrace();
}

// From File back to Object
try {
    SuperHero hero2 = mapper.readValue(xml, SuperHero.class);
    System.out.println(hero2);
} catch (JsonProcessingException e){
    e.printStackTrace();
}
}
```

----
### JSON Serialization
- Originally from the JavaScript world, JavaScript Object Notation, has
  become a popular data interchange format
- Also requires external libraries to work with and there are _many_ of them

```java
// Example using the Jackson library
ObjectMapper mapper = new ObjectMapper();

try {
    // From object to JSON
    String json = mapper.writeValueAsString(heroV2);
    System.out.println(json);

    // From JSON back to Object
    SuperHero heroDeserialized = mapper.readValue(
      json, 
      SuperHero.class
    );
    System.out.println(heroDeserialized);
} catch (JsonProcessingException e) {
    e.printStackTrace();
}

```

---
## Annotations
- Annotations let you add metadata about your program to your code
- Many libraries you use will rely on annotations for various purposes (e.g. `@Test`)

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
