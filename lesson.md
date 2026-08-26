# Java Functional Programming

## Lesson Overview

This lesson introduces functional programming in Java, focusing on functional interfaces, lambda expressions, and the Stream API. You'll learn to write cleaner, more expressive code by applying functional programming concepts to common programming tasks like filtering, transforming, and processing collections.

**Prerequisites:** Basic Java knowledge (classes, interfaces, collections like ArrayList)

## Lesson Objectives

By the end of this lesson, students will be able to:

1. **Write** lambda expressions to create concise anonymous functions
2. **Use** built-in functional interfaces (Predicate, Function, Consumer, Supplier)
3. **Apply** Stream API operations to process collections declaratively
4. **Chain** multiple stream operations to create data processing pipelines
5. **Transform** imperative code into functional style for better readability

---

> 📌 **Note for students**: All code examples in this lesson go inside `public static void main(String[] args)`. The first example of each Part shows the full class and main wrapper. After that, snippets show only the relevant code — but everything runs inside `main`.

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

### Traditional vs Functional — Side by Side

**Task**: Get all even numbers from a list.

**Traditional way (How to do it):**

```java
ArrayList<Integer> numbers = new ArrayList<>(Arrays.asList(1, 2, 3, 4, 5));
ArrayList<Integer> evenNumbers = new ArrayList<>();

for (int i = 0; i < numbers.size(); i++) {
    if (numbers.get(i) % 2 == 0) {
        evenNumbers.add(numbers.get(i));
    }
}

System.out.println(evenNumbers); // [2, 4]
```

You are telling Java — loop through, get each item, check if even, add to list. You control every step.

**Functional way (What you want):**

```java
List<Integer> evenNumbers = numbers.stream()
    .filter(n -> n % 2 == 0)
    .collect(Collectors.toList());

System.out.println(evenNumbers); // [2, 4]
```

You are just saying — stream the numbers, keep only the even ones, collect them. Java handles the loop.

> **Key point**: The original `numbers` list is **untouched**. Functional programming creates a **new list** rather than modifying the original. This makes your code safer — other parts of your program that use `numbers` are not affected.

Create `LearnFunctionalProgramming.java` and code along.

---

## Part 2 - Functional Interfaces

### What is a Functional Interface?

A **functional interface** is an interface with **exactly one abstract method**. This is the foundation of functional programming in Java — before you can use a lambda, you need a functional interface to hold it.

Think of a functional interface as a **contract**:
- The contract says: "You must provide one specific behavior"
- Your lambda expression fulfills that contract

> **Quick preview: what is a lambda?** You'll see lambdas everywhere in the examples below, so here's just enough to follow along. A **lambda expression** is a short, unnamed function that fills in the one method a functional interface requires. It looks like this: `(parameters) -> body` — for example, `name -> System.out.println(name)` means "take `name`, and print it." That's all you need for now — we'll do a full deep dive into lambda syntax and all its formats in **Part 3**.

### A Simple Example

Here is the full working code — notice the class and main wrapper:

```java
import java.util.Arrays;
import java.util.List;

@FunctionalInterface
interface Greeting {
    void sayHello(String name);
}

public class LearnFunctionalProgramming {
    public static void main(String[] args) {

        // The lambda implements the sayHello method
        Greeting friendlyGreeting = name -> System.out.println("Hello, " + name + "!");
        Greeting formalGreeting = name -> System.out.println("Good day, " + name + ".");

        // Use them
        friendlyGreeting.sayHello("Alice"); // Output: Hello, Alice!
        formalGreeting.sayHello("Bob");     // Output: Good day, Bob.

    }
}
```

**What happened?**
- `Greeting` interface has exactly one method — `sayHello`
- `name -> System.out.println("Hello, " + name + "!")` is a lambda that fulfills that contract
- We can store different behaviors in the same interface type and call them later

### Built-in Functional Interfaces

Java provides common functional interfaces in the `java.util.function` package so you don't have to create your own for common patterns.

#### 1. Predicate\<T\> - Testing a Condition

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

**Passing a Predicate as a method argument:**

A Predicate isn't just for testing values directly — you can also pass it into a method as an argument, the same way you'd pass an `int` or a `String`. The method just needs a parameter of type `Predicate<T>`:

```java
public static void checkNumber(int number, Predicate<Integer> condition) {
    System.out.println(condition.test(number));
}

// Passing isEven as the argument
checkNumber(4, isEven); // true
checkNumber(7, isEven); // false
```

**More examples:**

```java

// Check if a number is positive
Predicate<Integer> isPositive = num -> num > 0;
System.out.println(isPositive.test(10));  // true
System.out.println(isPositive.test(-5));  // false
```

**Passing different behaviors to the same method:**

The real power of passing a Predicate as an argument is that the *same method* can behave differently depending on which Predicate you hand it. The method stays generic — the rule comes from outside.

```java
public static void checkString(String str, Predicate<String> isLengthy) {
    System.out.println(isLengthy.test(str));
}
```

```java
// Two different definitions of "lengthy"
Predicate<String> longerThan5  = s -> s.length() > 5;
Predicate<String> longerThan10 = s -> s.length() > 10;

// Same method, same word — but different rule passed in → different result
checkString("Beautiful", longerThan5);  // true  (9 > 5)
checkString("Beautiful", longerThan10); // false (9 > 10)
```

Notice: `checkString` never changed. We only changed *which behavior we passed in*. This is what "functions are values" really means — you hand the method a rule, and it runs whatever rule it was given.

**Practical use with collections:**

```java
ArrayList<Integer> numbers = new ArrayList<>(Arrays.asList(1, 2, 3, 4, 5, 6, 7, 8, 9, 10));

// Remove all odd numbers using a predicate
numbers.removeIf(n -> n % 2 != 0);
System.out.println(numbers); // [2, 4, 6, 8, 10]
```

`removeIf()` takes a `Predicate` — here, `n -> n % 2 != 0` ("is this number odd?"). It goes through the list and removes every element where the predicate returns `true`. So it strips out all the odd numbers, leaving `[2, 4, 6, 8, 10]` — this **mutates the original list in place**, using the predicate to decide what gets removed.

#### 2. Function\<T, R\> - Transforming Data

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

#### 3. Consumer\<T\> - Performing Actions

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

#### 4. Supplier\<T\> - Providing Values

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

public class LearnFunctionalProgramming {
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

## Part 3 - Lambda Expressions

### What Problem Do Lambdas Solve?

Now that you understand functional interfaces, you know that Java needs a way to pass behavior as a value. Before Java 8, you had to create entire classes or use verbose anonymous classes to do this.

**Example problem**: You want to print each item in a list.

**Old way (verbose):**

```java
ArrayList<String> names = new ArrayList<>(Arrays.asList("Alice", "Bob", "Charlie"));

for (int i = 0; i < names.size(); i++) {
    System.out.println(names.get(i));
}
```

This works, but you're focused on **how** to loop rather than **what** you want to do.

**With Lambda (concise):**

```java
names.forEach(name -> System.out.println(name));
```

Now you're just saying: "For each name, print it". Much clearer!

### What Exactly is a Lambda Expression?

A **lambda expression** is a short way to write a function without giving it a name. It's also called an **anonymous function**. It is how you fulfill the contract of a functional interface in the shortest possible way.

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

- `(name)` - **Parameter**: The input to your function
- `->` - **Arrow**: Means "goes to" or "becomes"
- `System.out.println(name)` - **Body**: What to do with the input

**Read it as**: "name goes to print name"

### Different Lambda Formats

Here is the full working code showing all lambda formats:

```java
import java.util.ArrayList;
import java.util.Arrays;
import java.util.Collections;

@FunctionalInterface
interface Calculator {
    int calculate(int a, int b);
}

public class LearnFunctionalProgramming {
    public static void main(String[] args) {

        // ── Format 1: No parameters ──
        Runnable task = () -> System.out.println("Task is running");
        task.run(); // Output: Task is running

        // ── Format 2: One parameter ──
        ArrayList<Integer> numbers = new ArrayList<>(Arrays.asList(1, 2, 3, 4, 5));
        numbers.forEach(num -> System.out.println(num * 2));
        // Output: 2, 4, 6, 8, 10

        // ── Format 3: Multiple parameters ──
        ArrayList<String> names = new ArrayList<>(Arrays.asList("Alice", "Bob", "Charlie"));
        Collections.sort(names, (a, b) -> a.length() - b.length());
        System.out.println(names); // [Bob, Alice, Charlie]

        // ── Format 4: Multiple statements ──
        Calculator sumWithPrint = (x, y) -> {
            int sum = x + y;
            System.out.println("Sum is: " + sum);
            return sum;
        };
        sumWithPrint.calculate(3, 4); // Sum is: 7

    }
}
```

#### Format 1: No parameters

When you don't need any input:

```java
() -> System.out.println("Hello!")
```

**Read as**: "Nothing goes to print Hello"

#### Format 2: One parameter (parentheses optional)

```java
// With parentheses
(x) -> x * 2

// Without parentheses (cleaner)
x -> x * 2
```

#### Format 3: Multiple parameters

```java
(a, b) -> a + b
```

**Read as**: "a and b go to a plus b"

#### Format 4: Multiple statements (need curly braces)

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

```java
x -> x * 2                  // x becomes x times 2
name -> name.toUpperCase()  // name becomes uppercase name
(a, b) -> a + b             // a and b become their sum
```

### When to Use Lambdas

✅ **Use lambdas when:**
- You need to pass simple behavior to a method
- The function is used only once
- It makes your code clearer and shorter

> **Note**: You've already been writing lambdas throughout Part 2 to fill in your functional interfaces, and you'll get more practice with them in the Stream activity coming up — so there's no separate lambda exercise here.

---

## Part 4 - Stream API

### What is a Stream?

A **Stream** is like a pipeline for processing data. Imagine a conveyor belt in a factory:
- Items come in at one end
- They go through various stations (filtering, transforming, sorting)
- Final products come out at the other end

A Stream is **not** a data structure. It doesn't store data. It's a **pipeline** of operations on data.

### Why Use Streams?

**Old way** (imperative - tell computer HOW to do it):

```java
ArrayList<Integer> numbers = new ArrayList<>(Arrays.asList(1, 2, 3, 4, 5, 6, 7, 8, 9, 10));
ArrayList<Integer> evenNumbers = new ArrayList<>();

for (int num : numbers) {
    if (num % 2 == 0) {
        evenNumbers.add(num);
    }
}

System.out.println(evenNumbers); // [2, 4, 6, 8, 10]
```

**New way** (declarative - tell computer WHAT you want):

```java
import java.util.stream.Collectors;

List<Integer> evenNumbers = numbers.stream()
    .filter(num -> num % 2 == 0)
    .collect(Collectors.toList());

System.out.println(evenNumbers); // [2, 4, 6, 8, 10]
```

**Benefits:**
- More readable — reads like English
- Less code — no manual loops
- Fewer bugs — can't mess up loop conditions
- Easy to modify — just add more operations

### How Streams Work

Streams work in three steps:

1. **Create** a stream from a data source
2. **Transform** the data with intermediate operations (filter, map, sort, etc.)
3. **Collect** or use the results with a terminal operation

```
Data Source → Intermediate Operations → Terminal Operation → Result
```

### Creating Streams

Here is the full working code for creating streams:

```java
import java.util.Arrays;
import java.util.List;
import java.util.stream.Stream;

public class LearnFunctionalProgramming {
    public static void main(String[] args) {

        // Method 1: From a collection
        List<String> names = Arrays.asList("Alice", "Bob", "Charlie");
        Stream<String> nameStream = names.stream();

        // Method 2: From an array
        String[] nameArray = {"Alice", "Bob", "Charlie"};
        Stream<String> arrayStream = Arrays.stream(nameArray);

        // Method 3: Using Stream.of()
        Stream<Integer> numberStream = Stream.of(1, 2, 3, 4, 5);

    }
}
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

#### Collecting into a Map

You can also collect a stream into a `Map` — you just tell Java what the **key** should be and what the **value** should be.

```java
import java.util.Map;
import java.util.stream.Collectors;

List<String> names = Arrays.asList("Alice", "Bob", "Charlie");

// Key = the name, Value = the length of the name
Map<String, Integer> nameLengths = names.stream()
    .collect(Collectors.toMap(
        name -> name,           // how to build the key
        name -> name.length()   // how to build the value
    ));

System.out.println(nameLengths); // {Bob=3, Alice=5, Charlie=7}
```

`Collectors.toMap()` takes two functions — the first decides the **key**, the second decides the **value**. Java runs both on every element and builds the map for you.

> **Note**: Keys must be unique. If two elements produce the same key, Java throws an error — so pick something unique as your key (like an ID or a name).

---

### forEach() - Do Something With Each Element

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
    .filter(name -> name.length() > 3)     // Only names with 4+ letters (removes "BOB" and "eve")
    .map(n -> n.toLowerCase())             // Convert to lowercase
    .sorted()                              // Sort alphabetically
    .collect(Collectors.toList());         // Collect to list

System.out.println(result); // [alice, charlie, david]
```

### Common Stream Patterns

#### Pattern 1: Filter → Collect

```java
List<String> names = Arrays.asList("Alice", "Bob", "Charlie", "Alex", "Diana");

List<String> aNames = names.stream()
    .filter(name -> name.startsWith("A"))
    .collect(Collectors.toList());
// [Alice, Alex]
```

#### Pattern 2: Map → Collect

```java
List<String> words = Arrays.asList("cat", "dog", "bird");

List<Integer> lengths = words.stream()
    .map(word -> word.length())
    .collect(Collectors.toList());
// [3, 3, 4]
```

#### Pattern 3: Filter → Map → Collect

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

---

## Part 5 - Method References (Optional / Exposure Only)

> This section is optional. It's here purely to give you exposure to a syntax shortcut you may encounter in other people's code — there is no activity for this part.

Method references are an even shorter way to write a lambda **when the lambda only calls a single existing method**, using the `::` operator.

```java
// Lambda
names.forEach(name -> System.out.println(name));

// Method reference — same thing, shorter
names.forEach(System.out::println);
```

**Simple rule**: whenever your lambda looks like `x -> someMethod(x)`, you can usually replace it with `someClass::someMethod`. Java infers that `x` is coming from the stream/list and passes it automatically — you don't need to write it yourself.

That's all you need to know for now — if you see `::` in other people's code, you'll know what it's doing.

---

END