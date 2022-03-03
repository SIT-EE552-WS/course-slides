---
title: Lecture 7 - Lambdas and the Streams API
---
# Lecture 7
## Lambdas and the Streams API
---
## Outline

* Common patterns in collections
* Anonymous classes
* Lambdas
* The Streams API
---
## Filtering Lists

```java
/**
* Find all multiples of three in a list
*/
static List<Integer> example1(List<Integer> source) {
    List<Integer> result = new ArrayList<>();
    for (Integer element : source) {
        if (element % 3 == 0) {
            result.add(element);
        }
    }
    return result;
}

/**
* Find all multiples of five in a list
*/
static List<Integer> example2(List<Integer> source) {
    List<Integer> result = new ArrayList<>();
    for (Integer element : source) {
        if (element % 5 == 0) {
            result.add(element);
        }
    }
    return result;
}

/**
* Find all numbers less than 50
*/
static List<Integer> example3(List<Integer> source) {
    List<Integer> result = new ArrayList<>();
    for (Integer element : source) {
        if (element < 50) {
            result.add(element);
        }
    }
    return result;
}

/**
* Find all words that start with the letter A
*/
static List<String> example4(List<String> source) {
    List<String> result = new ArrayList<>();
    for (String element : source) {
        if (element.startsWith("A")) {
            result.add(element);
        }
    }
    return result;
}

/**
* Find all non-empty strings
*/
static List<String> example5(List<String> source) {
    List<String> result = new ArrayList<>();
    for (String element : source) {
        if (!element.isEmpty()) {
            result.add(element);
        }
    }
    return result;
}

```

----
## Transforming Lists

```java
/**
* Double every element in a list
*/
static List<Integer> example1(List<Integer> source) {
    List<Integer> result = new ArrayList<>();
    for (Integer element : source) {
        int newElem = element * 2;
        result.add(newElem);
    }
    return result;
}

/**
* Multiply every element in a list by 10
*/
static List<Integer> example2(List<Integer> source) {
    List<Integer> result = new ArrayList<>();
    for (Integer element : source) {
        int newElem = element * 10;
        result.add(newElem);
    }
    return result;
}

/**
* Add 5 to every element in a list
*/
static List<Integer> example3(List<Integer> source) {
    List<Integer> result = new ArrayList<>();
    for (Integer element : source) {
        int newElem = element + 5;
        result.add(newElem);
    }
    return result;
}

/**
* Extract the first three characters of each string
*/
static List<String> example4(List<String> source) {
    List<String> result = new ArrayList<>();
    for (String element : source) {
        String newElem = element.substring(0, 3);
        result.add(newElem);
    }
    return result;
}

/**
* Convert a list of strings to uppercase
*/
static List<String> example5(List<String> source) {
    List<String> result = new ArrayList<>();
    for (String element : source) {
        String newElem = element.toUpperCase();
        result.add(newElem);
    }
    return result;
}
```
----
## Summarizing Lists

```java
    /**
     * Find the sum of a list
     */
    static int example1(List<Integer> source) {
        int result = 0;
        for (Integer element : source) {
            result += element;
        }
        return result;
    }

    /**
     * Count the total number of characters in a list of
     * Strings
     */
    static int example2(List<String> source) {
        int result = 0;
        for (String element : source) {
            result += element.length();
        }
        return result;
    }

    /**
     * Combine words into a longer string
     */
    static String example3(List<String> source) {
        StringBuilder result = new StringBuilder();
        for (String element : source) {
            result.append(element);
        }
        return result.toString();
    }

```
---
## Anonymous Classes

- Given an interface or class, it is possible to extend it
  inline

```java
interface Something {
    int aMethod();
}

Something something = new Something() {
    @Override
    public int aMethod(){
        return 5;
    }
}

```

----

## Filtering with Anonymous Classes

```java
    
interface Filter<T> {
    boolean keep(T element);
}

static <T> List<T> baseFilter(
    List<T> source, 
    Filter<T> filter
) {
    List<T> result = new ArrayList<>();
    for (T element : source) {
        if (filter.keep(element)) {
            result.add(element);
        }
    }
    return result;
}

/**
* Find all multiples of three in a list
*/
static List<Integer> example1(List<Integer> source) {
    Filter<Integer> filter = new Filter<>(){
        @Override
        public boolean keep(Integer element){
            return element % 3 == 0;
        }
    }

    return baseFilter(source, filter);
}

/**
* Find all multiples of five in a list
*/
static List<Integer> example2(List<Integer> source) {
    Filter<Integer> filter = new Filter<>(){
        @Override
        public boolean keep(Integer element){
            return element % 5 == 0;
        }
    }

    return baseFilter(source, filter);
}

/**
* Find all numbers less than 50
*/
static List<Integer> example3(List<Integer> source) {
    Filter<Integer> filter = new Filter<>(){
        @Override
        public boolean keep(Integer element){
            return element < 50;
        }
    }

    return baseFilter(source, filter);
}

/**
* Find all words that start with the letter A
*/
static List<String> example4(List<String> source) {
    Filter<String> filter = new Filter<>(){
        @Override
        public boolean keep(String element){
            return element.startsWith("A");
        }
    }

    return baseFilter(source, filter);
}

/**
* Find all non-empty strings
*/
static List<String> example5(List<String> source) {
    Filter<String> filter = new Filter<>(){
        @Override
        public boolean keep(String element){
            return !element.isEmpty();
        }
    }

    return baseFilter(source, filter);
}

```
----
## Transforming with Anonymous Classes

```java
interface Transformer<T, R> {
    R transform(T element);
}

static <T, R> List<R> baseTransform(
    List<T> source, 
    Transformer<T, R> transformer
) {
    List<R> result = new ArrayList<>();
    for (T element : source) {
        R newElem = transformer.transform(element);
        result.add(newElem);
    }
    return result;
}

/**
* Double every element in a list
*/
static List<Integer> example1(List<Integer> source) {
    Transformer<Integer, Integer> transformer = 
    new Transformer<>() {

        @Override
        public Integer transform(Integer element) {
            return element * 2;
        }

    };

    return baseTransform(source, transformer);
}

/**
* Multiply every element in a list by 10
*/
static List<Integer> example2(List<Integer> source) {
    Transformer<Integer, Integer> transformer = 
    new Transformer<>() {

        @Override
        public Integer transform(Integer element) {
            return element * 10;
        }

    };
    return baseTransform(source, transformer);
}

/**
* Add 5 to every element in a list
*/
static List<Integer> example3(List<Integer> source) {

    Transformer<Integer, Integer> transformer = 
    new Transformer<>() {

        @Override
        public Integer transform(Integer element) {
            return element + 5;
        }

    };
    return baseTransform(source, transformer);
}

/**
* Extract the first three characters of each string
*/
static List<String> example4(List<String> source) {
    Transformer<String, String> transformer = 
    new Transformer<>(){

        @Override
        public String transform(String element) {
            return element.substring(0, 3);
        }

    };

    return baseTransform(source, transformer);
}

/**
* Convert a list of strings to uppercase
*/
static List<String> example5(List<String> source) {
    Transformer<String, String> transformer = 
    new Transformer<>(){

        @Override
        public String transform(String element) {
            return element.toUpperCase();
        }

    };
    return baseTransform(source, transformer);
}

static List<Integer> example6(List<String> source){
    Transformer<String, Integer> transformer = 
    new Transformer<>(){

        @Override
        public Integer transform(String element) {
            return element.length();
        }

    };

    return baseTransform(source, transformer);
}

```

----
## Summarizing with Anonymous Classes

```java
interface Summarizer<T, R> {
    R summarize(R acc, T element);
}

static <T,R> R baseSummarize(
    List<T> source, 
    R initialValue, 
    Summarizer<T, R> summarizer
) {
    R result = initialValue;
    for (T element : source) {
        result = summarizer.summarize(result, element);
    }
    return result;
}

/**
* Find the sum of a list
*/
static int example1(List<Integer> source) {
    Summarizer<Integer, Integer> summarizer = 
        (acc, element) -> acc + element;
    int initialValue = 0;
    return baseSummarize(source, initialValue, summarizer);
}

/**
* Count the total number of characters in a list of
* Strings
*/
static int example2(List<String> source) {
    Summarizer<String, Integer> summarizer = 
        (acc, element) -> acc + element.length();

    int initialValue = 0;
    return baseSummarize(source, initialValue, summarizer);
}

/**
* Combine words into a longer string
*/
static String example3(List<String> source) {
    Summarizer<String, StringBuilder> summarizer = 
        (acc, element) -> acc.append(element);
    StringBuilder initialValue = new StringBuilder();
    return baseSummarize(
            source, 
            initialValue, 
            summarizer
        ).toString();
}

```
---
 ## Lambda Expressions

 - Given an **interface** with only **one** method, you can create
   an implementation using a lambda expression

```java

interface Filter<T> {
    boolean keep(T element);
}

Filter<Integer> filter1 = (i) -> i % 3 == 0

List<Integer> result1 = filter(
    intSource,
    filter1
);

List<Integer> result2 = filter(
    intSource,
    i -> i % 5 == 0
);


```
----
## Common Lambda Interfaces
- Examples include
  * `Predicate<T>` 
     * `boolean test(T t)`
  * `Function<T,R>`
     * `R apply(T t)`
  * `BiFunction<T,U,R>` 
     * `R apply(T t, U u)`
  * `UnaryOperator<T>`
     * `T apply(T t)`
- The rest can be found in [java.util.function](https://docs.oracle.com/en/java/javase/17/docs/api/java.base/java/util/function/package-summary.html)

----
## Method References

- If you have a method that corresponds to a functional interface, you 
  can create a method reference lambda

```java
// for non-static methods, the first input element will
// be the type of the class
Predicate<String> p = String::isBlank
p.test(""); // returns true

BiFunction<String, Integer, String> f = String::substring
f.apply("Hello World", 5); // returns Hello

// for static methods, the inputs and return type must 
// match the functional interface
Function<Double, Double> f2 = Math::ceil
f2.apply(5.6);  // returns 6.0
```

---
## The Streams API

- The built-in API to perform functional operations on [streams](https://docs.oracle.com/en/java/javase/17/docs/api/java.base/java/util/stream/package-summary.html) of 
  elements
-  Streams can be created from any Collection using the `stream()` method
- Most stream methods take lambdas as parameters
- They are typically written to create pipelines of operations

----
### Filtering with Streams

```java
List<Integer> result = intSource
    .stream()
    .filter(i -> i % 3 == 0)
    .toList();    
```

----
### Transforming with Streams

```java
List<Integer> result = intSource
    .stream()
    .map(i -> i * 5)
    .toList();    
```

----
### Summarizing with Streams

```java
int result = intSource
    .stream()
    .reduce(0, (a,b) -> a + b);    

String result2 = strSource
    .stream()
    .reduce(
        new StringBuilder(), 
        (acc, el)->acc.append(el), 
        (sb1, sb2)->sb1.append(sb2)
    ).toString();
```

----
### Combining Stream Operations

```java
int result = intSource
    .stream()
    .filter(i -> i % 2 == 0)
    .map(i -> i * 2)
    .reduce(0, (a,b) -> a + b);    
```

----
### Stream Operations Must be Composable

- Given a list what is four times the first 
  number in that list greater than 6?

```java
source
    .stream()
    .filter(x -> x > 6)
    .findFirst()
    .map(x -> x * 4)
```

----

### But what if every number in source is less than 6?

```java
source
    .stream()
    .filter(x -> x > 6)
    .findFirst() // can't return Null here or the next line
                 // would throw a NullPointerException
    .map(x -> x * 4)
```
----

### `Optional<T>`
- A container that may hold zero or one non-null value
- See the [Javadoc](https://docs.oracle.com/en/java/javase/17/docs/api/java.base/java/util/Optional.html)

----

## Special Streams

* Collections cannot hold primitive values and must use boxed types,
  but there are special primitive streams such as:
    * `IntStream` - a stream of primitive integers
    * `LongStream` - a stream of primitive long values
    * `DoubleStream` a stream of primitive double values

----
## Example

```java
IntStream
    .range(0, 10) // 0, 1, ... 9
    .map( x -> x * 2)
    .reduce(0, Integer::sum);
    
IntStream
    .rangeClosed(0, 10) // 0, 1, ... 10
    .map( x -> x * 2)
    .reduce(0, Integer::sum);

```
