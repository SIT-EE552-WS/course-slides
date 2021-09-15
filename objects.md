# Lecture 3
## Objects, Classes and Inheritance
---
## Outline
- Motivation
- What is an Object?
- Data and Behaviors
- Principles
- Classes vs Objects
- Inheritance
- Access Modifiers: Scope and Encapsulation
- Abstract Methods and Interfaces (if we have time)
---
## Motivation 
> We write code for humans, not computers

> Humans' have a limited working memory (7 ± 2)

Let's start with an example...
---
## What is an Object?
- An object is a unit of code that wraps some **data** and/or **behaviors**
- It may or may not represent a physical "real world" object
- It should be namable using a **noun**
---
## Data and Behaviors
- Objects have **fields** (declared as variables within the object)
and **methods** (which we saw last week).

- Fields may describe characteristics that change over time (e.g. the position of
a car) typically called **state** or may be less likely to change (e.g. the
car's color).
----
### Field Examples

```java 
public class Rectangle {
  int height = 50;
  int width = 100;
  int top = 10;
  int left = 10;
}  
```
----
### Field Examples

```java 
public class Person {
  String name = "John Smith";
  String email = "jsmith@example.com";
  int age = 35;
}  
```
----
### Field Examples

```java 
public class Coffee {
  int espressoMl = 60;
  int chocolateMl = 60;
  int steamedMilkMl = 30;
}  
```
----
### Method Examples

```java 
public class Rectangle {
  int height = 50;
  int width = 100;
  int top = 10;
  int left = 10;

  int getArea(){
    return height * width;
  }
}  
```
----
### Method Examples

```java 
public class Person {
  String name = "John Smith";
  String email = "jsmith@example.com";
  int age = 35;

  void growUp(){
    age = age + 1;
  }
}  
```
---
## Principles
Java's idea of Object-Oriented Programming is inspired by the Smalltalk 
language (c. 1970s).

1. Everything is an object.
2. Every _object_ is an _instance_ of a _class_.
3. Every class has a _superclass_.
4. Everything happens by message sends.
5. Method lookup follows the _inheritance_ chain.
---
## Classes vs. Objects

- A **class** describes the structure and behavior of an object.  Think
of it as a template for making objects.

![Cookie Cutter](https://c.tenor.com/_1hHmOcQyOIAAAAC/kin-community-baking.gif)
----
## Classes vs. Objects
- An instance of a class is created by using the `new` keyword.

```java
Rectangle r = new Rectangle();
Person john = new Person();
Coffee c = new Coffee();
```
----
## Classes vs. Objects
- We can create multiple instances of the same class.

```java
Rectangle r1 = new Rectangle();
Rectangle r2 = new Rectangle();
Rectangle r3 = new Rectangle();
// etc...
```
----
## Classes vs. Objects
- Each instance has is own copy of each of the class's fields

```java
Person p1 = new Person();
Person p2 = new Person(); 

p1.growUp();

System.out.println(p1.age); // prints 36
System.out.println(p2.age); // prints 35
```
----
## Classes vs. Objects
- The `new` keyword calls a special method called a **constructor**.
- A constructor is written like this and can take arguments

```java
public class Rectangle {
  int height;
  int width;

  public Rectangle(int height, int width){
   this.height = height;
   this.width = width;
  }
}
```
----
## Classes vs. Objects

```java 
Rectangle r1 = new Rectangle(10, 5);
Rectangle r2 = new Rectagle(3, 10);

System.out.println(r1.getArea()); // prints 50
System.out.println(r2.getArea()); // prints 30
```
---
## Inheritance

- Inheritance in Java is the mechanism of basing one class upon another class.
- When thinking of inheritance, it is helpful to consider the **is-a** 
  relationship

----
### Example from Biology

- A poodle **is a** dog.
- A pug **is a** dog.
- A dog **is a** canid.
- A wolf **is a** canid.  (_But_... a wolf is not a dog)
- A canid **is a(n)** animal.
  - A dog **is a(n)** animal.
  - A poodle **is a(n)** animal.

----
### Example from Biology
- In Java, we declare class inheritance using the `extends` keyword

```java
public class Animal {
  // ...
}

public class Canid extends Animal {
  // ...
}

public class Dog extends Canid {
  // ...
}
```
----
### Example from Biology
- If we have 
```java
public class Dog extends Canid {
  // ...
}
```

- then we say that `Dog` is a **subclass** of `Canid`, and `Canid` is 
  a **superclass** of `Dog`.

----
### Example from Biology
- You can always replace a subclass with its superclass on the left side
  of a variable assignment

```java
Animal a = new Dog(); // this is fine
Canid c = new Dog();  // so is this
```

- You can also cast a superclass to its subclass if you are sure they match

```java 
Animal dog = new Dog();
Animal wolf = new Wolf();

Dog d = (Dog) dog; // this is fine
Dog d2 = (Dog) wolf; //this will throw a ClassCastException
```
----

### Example from Biology
- You can tell whether it is safe to do a cast with the `instanceof` keyword

```java 
Animal dog = new Dog();
Animal wolf = new Wolf();

System.out.println(dog instanceof Dog);  // prints true
System.out.println(wolf instanceof Dog); // prints false
System.out.println(dog instanceof Canid);  // prints true
System.out.println(wolf instanceof Canid); // prints true
```
---
## Inheritance and Overriding

- If a superclass and a subclass both include the same method declaration 
  with different code inside, the subclass's implementation will win.

```java
public class Animal {
  public void makeSound(){
    System.out.println("???");
  }
}

public class Cat extends Animal {
  public void makeSound(){
    System.out.println("Meow");
  }
}

public class Dog extends Animal {
  public void makeSound(){
    System.out.println("Woof");
  }
}

Animal dog = new Dog();
dog.makeSound(); // prints Woof

Animal cat = new Cat();
cat.makeSound(); // prints Meow
```
---
## Access Modifiers
- Fields and methods in a class can have one or more access modifiers
  - `static` means that the method or field belongs to the class rather
     than the object.
  - `public` means that the method or field can be used from any other
     class in your project.
  - `protected` means the method or field can only be used by the class
     and any of its subclasses
  - `private` means the method or field can only be used within the class
----
## Access Modifiers
|Modifier | Class| Package |Subclass | World|
|---------|------|---------|---------|------|
|public  |Y |Y |Y| Y|
|protected |Y |Y |Y |N|
|no modifier |Y |Y |N| N|
|private |Y |N| N |N|
----
## Why Make Things Private?

- **Encapsulation** - data and methods that operate on that data should be 
  bundled together.  Prevent direct access to data that should not be 
  changed outside the class.

- For example, a person may change their name for any number of reasons, 
  but _I_ cannot change __your__ name.
