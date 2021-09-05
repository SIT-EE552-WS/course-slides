## Step 1
- How would you do this by hand without any loops?
```java
System.out.println(1);    // 2^0
System.out.println(2);    // 2^1
System.out.println(4);    // 2^2
System.out.println(8);    // 2^3
// ...
System.out.println(4096); // 2^12
```
---
## Step 2
- Since Java doesn't have an exponent operator, what's another way to express this?
```java
System.out.println(1);    // 2^0
System.out.println(1*2);    // 2^1
System.out.println(1*2*2);    // 2^2
System.out.println(1*2*2*2);    // 2^3
// ...
System.out.println(1*2*2*2*2*2*2*2*2*2*2*2*2); // 2^12
```
---
## Step 3
- Now, extract out a variable.  Nothing has changed at this point, just another way to express the same thing. `pow` is just a stand-in for the value 1.
```java
int pow = 1;
System.out.println(pow);    // 2^0
System.out.println(pow*2);    // 2^1
System.out.println(pow*2*2);    // 2^2
System.out.println(pow*2*2*2);    // 2^3
// ...
System.out.println(pow*2*2*2*2*2*2*2*2*2*2*2*2); // 2^12
```
---
## Step 4
- But...notice that each expression contains the previous expression.  
- If we allow `pow` to change, we can make it so that each call to print looks the same.
```java
int pow = 1;                // 2^0
System.out.println(pow);    
pow = pow * 2;              // 2^0 * 2 = 2^1
System.out.println(pow);    
pow = pow * 2;              // 2^1 * 2 = 2^2
System.out.println(pow);    
pow = pow * 2;              // 2^2 * 2 = 2^3
System.out.println(pow);    
// ...
pow = pow * 2;              // 2^11 * 2 = 2^12
System.out.println(pow); 
```
---
## Step 5
- Now we have a set of two lines that repeat, 
```java
    System.out.println(pow); 
    pow = pow * 2;   
```
we can rewrite this with a loop. 

- Notice that we increase the value of `pow` _after_ printing it.

```java
int pow = 1;
for(int i = 0; pow < 4097; i++>){
    System.out.println(pow); 
    pow = pow * 2;   
}
```
---
## Step 6
- That last bit of code will work, but it can be simplified. 
- The for loop declaration has three parts:
```java
for(
    int i = 0;  
    // This statement only runs once the first time
    pow < 4097; 
    // This condition tells how long to run the loop
    i++         
    // This statement is executed every time the loop runs
){ }
```
---
## Step 7
- Incidentally, notice that we never use the variable `i` directly.  Although `i` is traditionally used in a for loop, there's nothing special about it.  We can just use `pow` directly
to simplify the loop.

```java
for(
    int pow = 1;   
    // This statement only runs once the first time
    pow < 4097;    
    // This condition tells how long to run the loop
    pow = pow * 2  
    // This statement is executed every time the loop runs
){ 
    System.out.println(pow);
}
```
---
## Step 8
- Or written in the usual style

```java
for(int pow = 1; pow < 4097; pow = pow * 2){ 
    System.out.println(pow);
}
```