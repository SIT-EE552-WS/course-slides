---
title: Lecture 2 - Functions, Methods, and Recursion
---

# Lecture 2
## Functions, Methods, and Recursion
---
## Outline
- Motivation
- What is a function?
- Inputs and outputs
- Functions in Java
- Methods vs Functions
- Composition
- Recursion
- Javadoc
---
## Motivation 
> A good programmer is a **lazy** programmer.

> Don't repeat yourself.
---
## What is a Function?
- A function takes some input and transforms it into some output
- Some familiar functions from math
`$$ f(x) = x^2 $$`
`$$ f(x) = cos(x) $$`
`$$ f(x) = log_{10}(x) $$`
---
## Inputs and Outputs
- Still looking at math
  - **Domain** the set of values for which a function is defined
  - **Range** the set of values which the function can produce
----
### For example: 
`$$ f(x) = x^2 $$`

<ul>
  <li class="fragment"><b>Domain</b> Takes as input any number $(-\infty$, $+\infty)$</li>
  <li class="fragment"><b>Range</b> Only produces positive numbers $[0, +\infty$)</li>
</ul>
----
### For example: 
`$$ f(x) = cos(x) $$`

<ul>
  <li class="fragment"><b>Domain</b> Takes as input any number $(-\infty$, $+\infty)$</li>
  <li class="fragment"><b>Range</b> Only produces values between -1 and +1 $[-1, 1]$</li>
</ul>
----
### For example: 
`$$ f(x) = log_{10}(x) $$`
_(Ignore imaginary numbers for now)_

<ul>
  <li class="fragment"><b>Domain</b> Takes as input any number greater than zero $(0,\infty)$</li>
  <li class="fragment"><b>Range</b> No minimum or maximum output value $(-\infty$, $+\infty)$</li>
</ul>
---
## Functions in Java
- Also have a valid set of inputs and outputs similar to domain and range
- These are expressed using _types_

----
#### Template for a Function

```
[type] functionName( [type] arg) { 
    // some code... 
    return [value];
}
```
----
### For example: 
```java
int square(int x){
    return x*x;
}
```
----
### For example: 
```java
boolean isEven(int x){
    return x % 2 == 0;
}
```
----
### For example:
```java
int ceiling(double x){
    return (int) x; // this notation is called a "type cast"

    // There's more accurate way to write this function,
    // but it's a little too complicated to get into for now.
    // This one produces the wrong result for negative numbers.
}
```
---
## Functions in Java
- Functions in math can take multiple inputs
`$$ f(x,y) = x + y $$`
- So can functions in Java

```java
int add(int x, int y){
    return x + y;
}
```
- Each input argument has a type and the overall function has **only one** output type
----
## Functions in Java
- The types don't have to be the same

```java
int myFunction(int x, char c){
    if( c >= 'A' && c <= 'Z'){
        // c is a capital Latin letter
        return x * 2;
    } else if (c >= 'a' && c <= 'z'){
        // c is a lowercase latin letter
        return x * 4;
    } else {
        // c is not a latin letter
        return x;
    }
}
```
----
## Functions in Java
- The Java Standard Library already defines many [standard mathematical functions](https://docs.oracle.com/en/java/javase/17/docs/api/java.base/java/lang/Math.html) 
----
They can be used if you import the `java.lang.Math` class

```java
import java.lang.Math;

double quartic(int a, int b, int c, int d, int e, int x){
    return a * Math.pow(x,4) + b * Math.pow(x,3) + 
           c * Math.pow(x,2) + d * x + e;
}
```
Which is equivalent to
`$$ a \cdot x^4 + b\cdot x^3 + c\cdot x^2 + d\cdot x + e $$`

---

## Methods
- All the code we write in Java has to belong to a _class_.
```java
public class HelloWorld {
    public static void main(String[] args){
        System.out.println("Hello World");
    }
}
```
- in this example we call `main` a _method_ of the _class_ `HelloWorld`.
----
- Functions like those on the previous slides also need to be put inside a class

```java
public class SomeClass {
    static int square(int x){
        return x*x;
    }

    public static void main(String[] args){
        System.out.println(square(2)); 
        // will output 4 to the console
    }
}
```
- Ignore `static` for now - just know that if you want to use your function inside `main` it has to have
  the `static` keyword.
----
- Every _function_ is a _method_.
- Not every _method_ is a _function_ 
- For example, `main` does not return anything. That's why we say it has return type `void`
```java
    public static void main(String[] args){
        System.out.println("Hello World");
    }
```
---
## Composition
- You can combine functions together as long as they are compatible
`\[\begin{aligned} 
f(x) &= x^2 \\
g(x) &= x + 5 \\
g(f(x)) &= x^2 + 5
\end{aligned}\]
`
----
## Composition
- The same thing in Java

```java
public class Composition {
    static int square(int x){
        return x*x;
    }

    static int plus5(int x){
        return x + 5;
    }

    public static void main(String[] args){
        System.out.println(plus5(square(2))); 
        // will output 9 to the console
    }
}
```
---
## Recursion
- You can compose a function with itself (as long as the program knows when to stop!)
- This next code will cause your program to crash

```java
int recurse(int x){
    return recurse(x-1);
}
```
- This one will run, but will always produce zero, but why?

```java
int recurse(int x){
    if(x == 0){
        return 0;
    }
    return recurse(x-1);
}
```
----
```java [2|5|2|5|2|5|2|5|2|5|2|3]
int recurse(int x){
    if(x == 0){
        return 0;
    }
    return recurse(x-1);
}
```

Let's step through the code...

```
recurse(5) = recurse(4)
           = recurse(3)
           = recurse(2)
           = recurse(1)
           = recurse(0)
           = 0
```
----
### A more practical example
- Factorial: `$ 5! = 5 \cdot 4 \cdot 3 \cdot 2 \cdot 1 $`  
- Or more generally, 
----
`\[\begin{aligned} 
0! &= 1 \\
x! &= x \cdot (x-1)!
\end{aligned}\]
`
----
And in Java 

```java
int factorial(int x){
    if(x == 0){
        return 1;
    }
    return x * factorial(x-1);
}
```
---

## Javadoc

- You should aim to write self-documenting code as much as possible
- When writing functions give thought to the function name and parameters
- Sometimes, though, even with careful names users of your function might
  need more information

----

### Javadoc

- The JDK includes `javadoc` - a tool for generating documentation from 
  comments in Java code
- `javadoc` is how they produce the API documentation we reference

----

 ### How to write Javadoc comments

 ```java [1|2,3|5-15|17|18-19]
/**
 * Returns the natural logarithm (base <i>e</i>) of a 
 * {@code double} value.  
 * 
 * Special cases:
 * <ul><li>If the argument is NaN or less than zero, 
 * then the result is NaN.
 * <li>If the argument is positive infinity, then the 
 * result is positive infinity.
 * <li>If the argument is positive zero or negative 
 *     zero, then the result is negative infinity.
 * </ul>
 *
 * <p>The computed result must be within 1 ulp of the exact 
 * result. Results must be semi-monotonic.
 *
 * @param   a   a value
 * @return  the value ln&nbsp;{@code a}, the natural 
 *          logarithm of {@code a}.
 */
public static double log(double a) { 
// ... 
}
 ``` 