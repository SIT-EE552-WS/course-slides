# Lecture 7
## Objects Continued
---
## Outline
- Equals and Hash Code
- Comparable
- The Builder Pattern
- Object Factories
---
## Equals and Hash Code

- **Recall:** a `Set` is a collection that cannot contain duplicate items.
- So what would you expect the following code to produce?

```java
Set<Fraction> fractions = new HashSet<>();
fractions.add(new Fraction(1,2));
fractions.add(new Fraction(1,2));
fractions.add(new Fraction(3,4));

System.out.println(fractions.size());

```
----
### The answer should be 2...
----
### But the answer is 3.

Why?
----

- Here is a simplified version of the `Fraction` class

```java 
public class Fraction {
  public int numerator;
  public int denominator;

  public Fraction(int num, int den){
     this.numerator = num;
     this.denominator = den;
  }
}

```

- Based on that, consider the following - will it print anything?

```java
Fraction f1 = new Fraction(1,2);
Fraction f2 = new Fraction(1,2);

if(f1 == f2){
  System.out.println("They're equal");
}

```
----
### Nothing

- When `==` is applied to Objects it is actually comparing their address in memory.

----

- Instead, the `Object` class defines a method called `equals` that should be
  used

```java

Fraction f1 = new Fraction(1,2);
Fraction f2 = new Fraction(1,2);

if(f1.equals(f2)){
  System.out.println("They're equal");
}

```
----

### This still won't print anything

But we're getting closer

----

The default implementation of `equals` also compares addresses in memory, 

but...  

----

`Fraction` is a **subclass** of `Object`

----

Subclasses can **override** methods from their superclass

----


```java [10-19]
public class Fraction {
  public int numerator;
  public int denominator;

  public Fraction(int num, int den){
     this.numerator = num;
     this.denominator = den;
  }

  @Override
  public boolean equals(Object o){
    if(!o instanceOf Fraction) {
      return false;
    } else {
      Fraction that = (Fraction) o;
      return this.numerator == that.numerator &&
         this.denominator == that.denominator;
    }
  }

}

```

----
### Finally


```java

Fraction f1 = new Fraction(1,2);
Fraction f2 = new Fraction(1,2);

if(f1.equals(f2)){
  System.out.println("They're equal");
}

```

This will print "They're equal"

----

- But this example still will not work the way we expect it to...

```java
Set<Fraction> fractions = new HashSet<>();
fractions.add(new Fraction(1,2));
fractions.add(new Fraction(1,2));
fractions.add(new Fraction(3,4));

System.out.println(fractions.size());

```

- That's because of how `HashSet` works internally

----

### A HashSet is backed by a hash table

- A **hash table** uses a hash function to compute an index, also called a hash
 code, into an array of buckets or slots, from which the desired value can be
 found.
- In Java, that function is the `hashCode()` method
- Again, this is a method on the `Object` class, which our objects can override

----
### The Rules of Hash Codes

1. If `hashCode()` is called twice on the same object, it **must** return the same
   value
2. If two objects are equal according to `equals`, then both **must** return the
   same hash code
3. If two objects are not equal according to `equals` they **may** return
   different hash codes

[See
also.](https://docs.oracle.com/en/java/javase/17/docs/api/java.base/java/lang/Object.html#hashCode())

----

### Some options

----
### Always return the same number

```java [10-13]
public class Fraction {
  public int numerator;
  public int denominator;

  public Fraction(int num, int den){
     this.numerator = num;
     this.denominator = den;
  }

  @Override
  public int hashCode(){
    return 1;
  }

  @Override
  pulblic boolean equals(Object o){ // ... }

}

```

----

### Return based on integer fields

```java [10-13]
public class Fraction {
  public int numerator;
  public int denominator;

  public Fraction(int num, int den){
     this.numerator = num;
     this.denominator = den;
  }

  @Override
  public int hashCode(){
    return numerator + denominator;
  }

  @Override
  pulblic boolean equals(Object o){ // ... }
}

```


### More efficient using prime numbers

```java [10-13]
public class Fraction {
  public int numerator;
  public int denominator;

  public Fraction(int num, int den){
     this.numerator = num;
     this.denominator = den;
  }

  @Override
  public int hashCode(){
    int result = (int) (id ^ (id >>> 32));
    result = 31 * result + numerator;
    result = 31 * result + denominator;
    return result;
  }

  @Override
  pulblic boolean equals(Object o){ // ... }
}
```
----
### That last one can be automatically generated even for non-integer fields

---
## Sorting and Comparing Objects

```java
List<Fraction> list = new ArrayList<>();
list.add(new Fraction(1,5));
list.add(new Fraction(1,2));
list.add(new Fraction(1,10))

Collections.sort(l);
```

----

### Error: 
No suitable method found for `sort(java.util.ArrayList<Fraction>)`

----

- The signature for `Collections.sort()` looks a little strange

```java
public static <T extends Comparable<? super T>> void sort(
   List<T> list
)
```
- The important part is that the generic type `T` must extend something called
`Comparable`


----

`Comparable` is an interface with one method.

```java
interface Comparable<T> {

  /**
   * Compares this object with the specified object for order. 
   * Returns a negative integer, zero, or a positive integer 
   * as this object is less than, equal to, or greater 
   * than the specified object.
   */
  int compareTo(T o);

}
```
