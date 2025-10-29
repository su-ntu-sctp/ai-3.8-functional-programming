# Java Functional Programming - Basic Level

## Lesson Overview

This lesson introduces functional programming in Java, focusing on lambda expressions, functional interfaces, and the Stream API. You'll learn to write cleaner, more expressive code by applying functional programming concepts to common programming tasks like filtering, transforming, and processing collections.

**Prerequisites:** Basic Java knowledge (classes, interfaces, collections like ArrayList)

## Lesson Objectives

By the end of this lesson, students will be able to:

1. **Write** lambda expressions to create concise anonymous functions
2. **Use** built-in functional interfaces (Predicate, Function, Consumer, Supplier)
3. **Apply** Stream API operations to process collections declaratively
4. **Chain** multiple stream operations to create data processing pipelines
5. **Transform** imperative code into functional style for better readability

---

## Part 1 - Introduction to Functional Programming

### What is Functional Programming?

Functional Programming (FP) is a style of programming where you focus on **what to do** rather than **how to do it**. Instead of writing step-by-step instructions, you describe the operations you want to perform.

Think of it like giving directions:
- **Traditional approach**: "Go forward 100m, turn left, go 50m, turn right..."
- **Functional approach**: "Go to the nearest coffee shop"

Key ideas in functional programming:
- **Functions are values**: You can pass functions around just like numbers or strings
- **Avoid changing data**: Instead of modifying existing data, create new data
- **Chain operations**: Connect multiple operations together like building blocks

Java introduced functional programming features in **Java 8** with Lambda expressions and the Stream API.

### Why Learn Functional Programming?

Functional programming helps you write:
- **Cleaner code**: Less boilerplate, more readable
- **Safer code**: Fewer bugs from accidental data changes
- **More maintainable code**: Easier to understand and modify

Create `LearnFunctionalProgramming.java` and code along.

---

## Part 2 - Lambda Expressions

### What Problem Do Lambdas Solve?

Before Java 8, if you wanted to pass behavior (not just data) to a method, you had to create entire classes or use verbose anonymous classes.

**Example problem**: You want to print each item in a list.

**Old way (verbose):**

```java
ArrayList<String> names = new ArrayList<>(Arrays.asList("Alice", "Bob", "Charlie"));

for (int i = 0; i < names.size(); i++) {
    System.out.println(names.get(i));
}
```

This works, but you're focused on **how** to loop (the index, the condition, incrementing) rather than **what** you want to do (print each name).

**With Lambda (concise):**

```java
names.forEach(name -> System.out.println(name));
```

Now you're just saying: "For each name, print it". Much clearer!

### What Exactly is a Lambda Expression?

A **lambda expression** is a short way to write a function without giving it a name. It's also called an **anonymous function**.

Think of it as a recipe you use once and don't need to save:
- **Named function**: Like a recipe card you keep in your recipe box
- **Lambda**: Like quickly telling someone "just mix flour and water"

### Lambda Syntax Explained

The basic structure is:

```
(parameters) -> { body }
```

Let's break this down:

```java
(name) -> System.out.println(name)
```

- `(name)` - **Parameter**: The input to your function (like ingredients)
- `->` - **Arrow**: Means "goes to" or "becomes"
- `System.out.println(name)` - **Body**: What to do with the input (like cooking instructions)

**Read it as**: "name goes to print name"

### Different Lambda Formats

#### 1. No parameters

When you don't need any input:

```java
() -> System.out.println("Hello!")
```

**Read as**: "Nothing goes to print Hello"

**Real example:**

```java
// Create a simple task
Runnable task = () -> System.out.println("Task is running");
task.run(); // Output: Task is running
```

#### 2. One parameter (parentheses optional)

```java
// With parentheses
(x) -> x * 2

// Without parentheses (cleaner)
x -> x * 2
```

**Real example:**

```java
ArrayList<Integer> numbers = new ArrayList<>(Arrays.asList(1, 2, 3, 4, 5));

// Double each number
numbers.forEach(num -> System.out.println(num * 2));
// Output: 2, 4, 6, 8, 10
```

#### 3. Multiple parameters

When you need more than one input:

```java
(a, b) -> a + b
```

**Read as**: "a and b go to a plus b"

**Real example:**

```java
// Sort by length
Collections.sort(names, (a, b) -> a.length() - b.length());
```

#### 4. Multiple statements (need curly braces)

When you need to do more than one thing:

```java
(x, y) -> {
    int sum = x + y;
    System.out.println("Sum is: " + sum);
    return sum;
}
```

### Understanding the Arrow ->

The arrow `->` is the key symbol in lambda expressions. Think of it as:
- "becomes"
- "goes to"
- "transforms into"
- "results in"

```java
x -> x * 2      // x becomes x times 2
name -> name.toUpperCase()  // name becomes uppercase name
(a, b) -> a + b // a and b become their sum
```

### Lambda vs Traditional Comparison

Let's see how lambdas simplify code with a practical example.

**Task**: Sort a list of names by their length.

**Old way with Anonymous Class:**

```java
import java.util.ArrayList;
import java.util.Collections;
import java.util.Comparator;

ArrayList<String> names = new ArrayList<>(Arrays.asList("Alice", "Bob", "Charlie", "David"));

// Using anonymous class - VERY verbose!
Collections.sort(names, new Comparator<String>() {
    @Override
    public int compare(String a, String b) {
        return a.length() - b.length();
    }
});

System.out.println(names); // [Bob, Alice, David, Charlie]
```

**New way with Lambda:**

```java
Collections.sort(names, (a, b) -> a.length() - b.length());

System.out.println(names); // [Bob, Alice, David, Charlie]
```

**What happened?**
- We removed 5 lines of boilerplate code!
- The logic is the same: compare lengths of two strings
- Much easier to read and understand

### When to Use Lambdas

✅ **Use lambdas when:**
- You need to pass simple behavior to a method
- The function is used only once
- It makes your code clearer and shorter

### 🧑‍💻 Quick Activity: Lambda Practice

Try these exercises:

1. Create a lambda that takes two integers and returns their sum
   ```java
   // Your code here
   Calculator add = (a, b) -> a + b;
   System.out.println(add.calculate(10, 5)); // 15
   ```

2. Use lambda to sort a list of strings by length (shortest first)
   ```java
   List<String> words = Arrays.asList("apple", "pie", "banana", "kiwi");
   // Your code here
   ```

3. Use lambda with forEach to print each number multiplied by 3
   ```java
   List<Integer> numbers = Arrays.asList(1, 2, 3, 4, 5);
   // Your code here
   ```

---

## Part 3 - Functional Interfaces

### What is a Functional Interface?

Before we can understand functional interfaces, let's understand why they exist.

**Remember**: Lambda expressions are just short ways to write functions. But in Java, **everything must be an object**. So how does Java know what type a lambda is?

**Answer**: Functional Interfaces!

A **functional interface** is an interface with **exactly one abstract method**. This one method is what your lambda expression implements.

Think of a functional interface as a **contract**:
- The contract says: "You must provide one specific behavior"
- Your lambda expression fulfills that contract

### A Simple Example

Let's create our own functional interface:

```java
@FunctionalInterface
interface Greeting {
    void sayHello(String name);
}
```

This interface says: "You must provide a method that takes a String and returns nothing."

**Now we can use it with a lambda:**

```java
// The lambda implements the sayHello method
Greeting friendlyGreeting = name -> System.out.println("Hello, " + name + "!");
Greeting formalGreeting = name -> System.out.println("Good day, " + name + ".");

// Use them
friendlyGreeting.sayHello("Alice"); // Output: Hello, Alice!
formalGreeting.sayHello("Bob");     // Output: Good day, Bob.
```

**What happened?**
- `name -> System.out.println("Hello, " + name + "!")` is a lambda
- It implements the `sayHello` method from the `Greeting` interface
- We can store it in a variable and call it later

### Built-in Functional Interfaces

Java provides common functional interfaces in the `java.util.function` package so you don't have to create your own for common patterns.

#### 1. Predicate<T> - Testing a Condition

**What it does**: Takes one input, returns true or false

**The method**: `boolean test(T t)`

```java
import java.util.function.Predicate;

// Create a predicate that tests if a number is even
Predicate<Integer> isEven = number -> number % 2 == 0;

// Test it
System.out.println(isEven.test(4));  // true
System.out.println(isEven.test(7));  // false
```

**More examples:**

```java
// Check if a string is long
Predicate<String> isLongWord = word -> word.length() > 5;
System.out.println(isLongWord.test("Hello"));     // false
System.out.println(isLongWord.test("Beautiful")); // true

// Check if a number is positive
Predicate<Integer> isPositive = num -> num > 0;
System.out.println(isPositive.test(10));  // true
System.out.println(isPositive.test(-5));  // false
```

**Practical use with collections:**

```java
ArrayList<Integer> numbers = new ArrayList<>(Arrays.asList(1, 2, 3, 4, 5, 6, 7, 8, 9, 10));

// Remove all odd numbers using a predicate
numbers.removeIf(n -> n % 2 != 0);
System.out.println(numbers); // [2, 4, 6, 8, 10]
```

#### 2. Function<T, R> - Transforming Data

**What it does**: Takes one input of type T, returns output of type R

**The method**: `R apply(T t)`

```java
import java.util.function.Function;

// Takes a String, returns its length (Integer)
Function<String, Integer> getLength = str -> str.length();

System.out.println(getLength.apply("Hello")); // 5
System.out.println(getLength.apply("Java"));  // 4
```

**More examples:**

```java
// Convert string to uppercase
Function<String, String> toUpper = str -> str.toUpperCase();
System.out.println(toUpper.apply("hello")); // HELLO

// Double a number
Function<Integer, Integer> doubleIt = n -> n * 2;
System.out.println(doubleIt.apply(5)); // 10
```

#### 3. Consumer<T> - Performing Actions

**What it does**: Takes one input, returns nothing (just does something with the input)

**The method**: `void accept(T t)`

```java
import java.util.function.Consumer;

// Takes a string and prints it
Consumer<String> printer = message -> System.out.println(message);

printer.accept("Hello, World!"); // Hello, World!
printer.accept("Java is fun!");  // Java is fun!
```

**Use with collections:**

```java
ArrayList<String> names = new ArrayList<>(Arrays.asList("Alice", "Bob", "Charlie"));

// Print each name with a greeting
Consumer<String> greetPerson = name -> System.out.println("Hello, " + name + "!");
names.forEach(greetPerson);
// Output:
// Hello, Alice!
// Hello, Bob!
// Hello, Charlie!
```

#### 4. Supplier<T> - Providing Values

**What it does**: Takes no input, returns a value

**The method**: `T get()`

```java
import java.util.function.Supplier;

// Supplier that always returns "Hello"
Supplier<String> greetingSupplier = () -> "Hello";

System.out.println(greetingSupplier.get()); // Hello
System.out.println(greetingSupplier.get()); // Hello
```

**More examples:**

```java
// Generate random number
Supplier<Double> randomSupplier = () -> Math.random();
System.out.println(randomSupplier.get()); // Random number

// Get current time
Supplier<Long> timeSupplier = () -> System.currentTimeMillis();
System.out.println(timeSupplier.get()); // Current timestamp
```

### Summary of Common Functional Interfaces

| Interface | Input | Output | Method | Use Case | Example |
|-----------|-------|--------|--------|----------|---------|
| `Predicate<T>` | T | boolean | `test(T t)` | Testing conditions | Is this number even? |
| `Function<T,R>` | T | R | `apply(T t)` | Transforming data | Convert string to uppercase |
| `Consumer<T>` | T | void | `accept(T t)` | Performing actions | Print each item |
| `Supplier<T>` | none | T | `get()` | Providing values | Generate random number |

### Creating Your Own Functional Interface

You can create custom functional interfaces for specific needs:

```java
@FunctionalInterface
interface Calculator {
    int calculate(int a, int b);
}

public class Main {
    public static void main(String[] args) {
        // Different calculations using the same interface
        Calculator add = (a, b) -> a + b;
        Calculator subtract = (a, b) -> a - b;
        Calculator multiply = (a, b) -> a * b;
        
        System.out.println("5 + 3 = " + add.calculate(5, 3));        // 8
        System.out.println("5 - 3 = " + subtract.calculate(5, 3));   // 2
        System.out.println("5 * 3 = " + multiply.calculate(5, 3));   // 15
    }
}
```

**The `@FunctionalInterface` annotation** is optional but recommended because:
- It documents that this interface is meant for lambda expressions
- The compiler will give an error if you accidentally add a second abstract method

### 🧑‍💻 Quick Activity: Functional Interfaces Practice

Try these exercises:

1. Create a Predicate to check if a string contains the letter 'a'
   ```java
   Predicate<String> containsA = // Your code here
   System.out.println(containsA.test("banana")); // true
   System.out.println(containsA.test("kiwi"));   // false
   ```

2. Create a Function that takes an integer and returns "Even" or "Odd"
   ```java
   Function<Integer, String> evenOrOdd = // Your code here
   System.out.println(evenOrOdd.apply(4));  // "Even"
   System.out.println(evenOrOdd.apply(7));  // "Odd"
   ```

3. Use Consumer to print each word with "Word: " prefix
   ```java
   List<String> words = Arrays.asList("Java", "Python", "C++");
   Consumer<String> printWithPrefix = // Your code here
   words.forEach(printWithPrefix);
   // Output: Word: Java, Word: Python, Word: C++
   ```

---

## Part 4 - Stream API

### What is a Stream?

A **Stream** is like a pipeline for processing data. Imagine a conveyor belt in a factory:
- Items come in at one end
- They go through various stations (cleaning, painting, packaging)
- Final products come out at the other end

With Streams, you can:
- Filter out items you don't want
- Transform items into something else
- Collect the results at the end

A Stream is **not** a data structure. It doesn't store data. It's a **pipeline** of operations on data.

### Why Use Streams?

**Old way** (imperative - tell computer HOW to do it):

```java
ArrayList<Integer> numbers = new ArrayList<>(Arrays.asList(1, 2, 3, 4, 5, 6, 7, 8, 9, 10));
ArrayList<Integer> evenNumbers = new ArrayList<>();

// Step by step instructions
for (int num : numbers) {
    if (num % 2 == 0) {        // Check if even
        evenNumbers.add(num);   // Add to new list
    }
}

System.out.println(evenNumbers); // [2, 4, 6, 8, 10]
```

**New way** (declarative - tell computer WHAT you want):

```java
List<Integer> evenNumbers = numbers.stream()
    .filter(num -> num % 2 == 0)  // Keep only even numbers
    .collect(Collectors.toList()); // Collect into a list

System.out.println(evenNumbers); // [2, 4, 6, 8, 10]
```

**Benefits:**
- More readable - reads like English
- Less code - no manual loops
- Fewer bugs - can't mess up loop conditions
- Easy to modify - just add more operations

### How Streams Work

Streams work in three steps:

1. **Create** a stream from a data source
2. **Transform** the data with intermediate operations (filter, map, sort, etc.)
3. **Collect** or use the results with a terminal operation

```
Data Source → Intermediate Operations → Terminal Operation → Result
```

### Creating Streams

```java
import java.util.stream.Stream;
import java.util.Arrays;
import java.util.List;

// Method 1: From a collection
List<String> names = Arrays.asList("Alice", "Bob", "Charlie");
Stream<String> nameStream = names.stream();

// Method 2: From an array
String[] nameArray = {"Alice", "Bob", "Charlie"};
Stream<String> arrayStream = Arrays.stream(nameArray);

// Method 3: Using Stream.of()
Stream<Integer> numberStream = Stream.of(1, 2, 3, 4, 5);
```

### Stream Operations: Two Types

#### 1. Intermediate Operations
- **Return** a new stream
- Are **lazy** (don't execute until terminal operation is called)
- Can be **chained** together
- Examples: `filter()`, `map()`, `sorted()`

#### 2. Terminal Operations
- **Trigger** the stream pipeline execution
- Return a **result** or cause a side effect
- Can only be used **once** on a stream
- Examples: `collect()`, `forEach()`, `count()`

### Important: Streams are Lazy

```java
List<String> names = Arrays.asList("Alice", "Bob", "Charlie");

// This doesn't execute yet!
Stream<String> stream = names.stream()
    .filter(name -> {
        System.out.println("Filtering: " + name);
        return name.startsWith("A");
    });

System.out.println("Stream created, but nothing printed yet!");

// Now it executes!
stream.forEach(name -> System.out.println("Result: " + name));

// Output:
// Stream created, but nothing printed yet!
// Filtering: Alice
// Result: Alice
// Filtering: Bob
// Filtering: Charlie
```

### Intermediate Operations

#### filter() - Keep Only What You Want

**What it does**: Keeps elements that match a condition (Predicate)

```java
List<Integer> numbers = Arrays.asList(1, 2, 3, 4, 5, 6, 7, 8, 9, 10);

// Get only even numbers
List<Integer> evenNumbers = numbers.stream()
    .filter(n -> n % 2 == 0)
    .collect(Collectors.toList());

System.out.println(evenNumbers); // [2, 4, 6, 8, 10]
```

**More examples:**

```java
List<String> names = Arrays.asList("Alice", "Bob", "Charlie", "Alex");

// Get names starting with 'A'
List<String> aNames = names.stream()
    .filter(name -> name.startsWith("A"))
    .collect(Collectors.toList());

System.out.println(aNames); // [Alice, Alex]
```

#### map() - Transform Each Element

**What it does**: Transforms each element using a Function

```java
List<String> names = Arrays.asList("alice", "bob", "charlie");

// Convert all names to uppercase
List<String> upperNames = names.stream()
    .map(name -> name.toUpperCase())
    .collect(Collectors.toList());

System.out.println(upperNames); // [ALICE, BOB, CHARLIE]
```

**More examples:**

```java
List<Integer> numbers = Arrays.asList(1, 2, 3, 4, 5);

// Square each number
List<Integer> squared = numbers.stream()
    .map(n -> n * n)
    .collect(Collectors.toList());

System.out.println(squared); // [1, 4, 9, 16, 25]
```

#### sorted() - Arrange in Order

**What it does**: Sorts elements in natural order or using a Comparator

```java
List<Integer> numbers = Arrays.asList(5, 2, 8, 1, 9, 3);

// Sort in ascending order
List<Integer> sorted = numbers.stream()
    .sorted()
    .collect(Collectors.toList());

System.out.println(sorted); // [1, 2, 3, 5, 8, 9]

// Sort in descending order
List<Integer> descending = numbers.stream()
    .sorted((a, b) -> b - a)
    .collect(Collectors.toList());

System.out.println(descending); // [9, 8, 5, 3, 2, 1]
```

### Terminal Operations

#### collect() - Gather Results

**What it does**: Converts stream back into a collection

```java
import java.util.stream.Collectors;

List<String> names = Arrays.asList("Alice", "Bob", "Charlie");

// Collect to List
List<String> nameList = names.stream()
    .collect(Collectors.toList());

// Collect to Set (removes duplicates)
Set<String> nameSet = names.stream()
    .collect(Collectors.toSet());
```

#### forEach() - Do Something With Each Element

**What it does**: Performs an action on each element

```java
List<String> names = Arrays.asList("Alice", "Bob", "Charlie");

// Print each name
names.stream()
    .forEach(name -> System.out.println(name));

// Print with formatting
names.stream()
    .forEach(name -> System.out.println("Hello, " + name + "!"));
```

#### count() - Count Elements

**What it does**: Counts how many elements are in the stream

```java
List<Integer> numbers = Arrays.asList(1, 2, 3, 4, 5, 6, 7, 8, 9, 10);

// Count even numbers
long count = numbers.stream()
    .filter(n -> n % 2 == 0)
    .count();

System.out.println("Even numbers: " + count); // 5
```

### Chaining Operations - The Real Power

The real power of streams comes from chaining operations together:

```java
List<String> names = Arrays.asList("alice", "BOB", "Charlie", "DAVID", "eve");

// Complex pipeline
List<String> result = names.stream()
    .filter(name -> name.length() > 3)     // Only names with 4+ letters
    .map(String::toLowerCase)              // Convert to lowercase
    .sorted()                              // Sort alphabetically
    .collect(Collectors.toList());         // Collect to list

System.out.println(result); // [alice, bob, charlie, david]
```

### Common Stream Patterns

#### Pattern 1: Filter → Collect

Get items matching a condition:

```java
List<String> names = Arrays.asList("Alice", "Bob", "Charlie", "Alex", "Diana");

List<String> aNames = names.stream()
    .filter(name -> name.startsWith("A"))
    .collect(Collectors.toList());
// [Alice, Alex]
```

#### Pattern 2: Map → Collect

Transform all items:

```java
List<String> words = Arrays.asList("cat", "dog", "bird");

List<Integer> lengths = words.stream()
    .map(word -> word.length())
    .collect(Collectors.toList());
// [3, 3, 4]
```

#### Pattern 3: Filter → Map → Collect

Filter then transform:

```java
List<Integer> numbers = Arrays.asList(1, 2, 3, 4, 5, 6, 7, 8, 9, 10);

List<Integer> result = numbers.stream()
    .filter(n -> n % 2 == 0)    // Get even numbers
    .map(n -> n * n)             // Square them
    .collect(Collectors.toList());
// [4, 16, 36, 64, 100]
```

### 🧑‍💻 Quick Activity: Stream Practice

Try these exercises:

1. From a list of numbers 1-20, get all numbers divisible by 3
   ```java
   List<Integer> numbers = Arrays.asList(1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 12, 13, 14, 15, 16, 17, 18, 19, 20);
   // Your code here to get [3, 6, 9, 12, 15, 18]
   ```

2. From a list of names, get all names in uppercase that are longer than 4 characters
   ```java
   List<String> names = Arrays.asList("John", "Alice", "Bob", "Charlie", "Eve");
   // Your code here to get [ALICE, CHARLIE]
   ```

3. From a list of prices, calculate how many items cost more than 50
   ```java
   List<Double> prices = Arrays.asList(25.99, 65.00, 45.50, 80.00, 30.00, 55.75);
   // Your code here to get count: 3
   ```

4. Sort a list of words alphabetically and get the first 3
   ```java
   List<String> words = Arrays.asList("zebra", "apple", "mango", "banana", "kiwi");
   // Your code here to get [apple, banana, kiwi]
   ```

---

## Part 5 - Method References

### What are Method References?

Method references are an even shorter way to write lambda expressions **when the lambda only calls a single existing method**.

Think of it as a shortcut:
- **Lambda**: `x -> System.out.println(x)`
- **Method Reference**: `System.out::println`

Both do the same thing, but the method reference is shorter!

### The :: Operator

The double colon `::` operator is used for method references. Read it as "reference to method".

### When to Use Method References

✅ Use method reference when your lambda **only calls one existing method**:

```java
// Lambda that only calls a method
names.forEach(name -> System.out.println(name));

// Method reference - cleaner!
names.forEach(System.out::println);
```

❌ Don't use method reference when lambda does more than call one method:

```java
// Can't use method reference here
names.forEach(name -> System.out.println("Name: " + name));
```

### Types of Method References

#### 1. Static Method Reference

**Format**: `ClassName::staticMethodName`

```java
// Lambda version
Function<String, Integer> parser1 = s -> Integer.parseInt(s);

// Method reference version
Function<String, Integer> parser2 = Integer::parseInt;

// Using it
System.out.println(parser2.apply("123")); // 123
```

#### 2. Instance Method Reference

**Format**: `ClassName::instanceMethodName`

The most common type!

```java
List<String> names = Arrays.asList("alice", "bob", "charlie");

// Lambda - calling method ON the parameter
names.stream()
    .map(name -> name.toUpperCase())
    .forEach(name -> System.out.println(name));

// Method reference - much cleaner!
names.stream()
    .map(String::toUpperCase)
    .forEach(System.out::println);
// Output: ALICE, BOB, CHARLIE
```

**How to read it**: `String::toUpperCase` means "call toUpperCase() on each String"

#### 3. Constructor Reference

**Format**: `ClassName::new`

```java
// Lambda version
Supplier<ArrayList<String>> supplier1 = () -> new ArrayList<>();

// Constructor reference version
Supplier<ArrayList<String>> supplier2 = ArrayList::new;

// Using it
ArrayList<String> list = supplier2.get();
```

### Quick Conversion Examples

| Lambda Expression | Method Reference |
|-------------------|------------------|
| `s -> Integer.parseInt(s)` | `Integer::parseInt` |
| `s -> System.out.println(s)` | `System.out::println` |
| `s -> s.toUpperCase()` | `String::toUpperCase` |
| `s -> s.length()` | `String::length` |
| `() -> new ArrayList<>()` | `ArrayList::new` |

### Practical Example

```java
import java.util.*;
import java.util.stream.Collectors;

public class MethodRefExample {
    public static void main(String[] args) {
        List<String> names = Arrays.asList("alice", "bob", "charlie", "david");
        
        // Without method references
        List<String> result1 = names.stream()
            .map(name -> name.toUpperCase())
            .collect(Collectors.toList());
        
        // With method references - cleaner!
        List<String> result2 = names.stream()
            .map(String::toUpperCase)
            .collect(Collectors.toList());
        
        // Print using method reference
        result2.forEach(System.out::println);
        // Output: ALICE, BOB, CHARLIE, DAVID
    }
}
```

### 🧑‍💻 Quick Activity: Method References Practice

Convert these lambdas to method references:

1. ```java
   List<String> words = Arrays.asList("apple", "banana", "cherry");
   words.forEach(word -> System.out.println(word));
   // Convert to method reference
   ```

2. ```java
   List<String> names = Arrays.asList("john", "alice", "bob");
   List<String> upper = names.stream()
       .map(name -> name.toUpperCase())
       .collect(Collectors.toList());
   // Convert to method reference
   ```

3. ```java
   List<String> numbers = Arrays.asList("1", "2", "3", "4", "5");
   List<Integer> ints = numbers.stream()
       .map(s -> Integer.parseInt(s))
       .collect(Collectors.toList());
   // Convert to method reference
   ```

---

END
