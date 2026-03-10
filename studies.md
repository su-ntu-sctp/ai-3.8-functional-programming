# 3.8 Self Studies

**Estimated Preparation Time:** 70 minutes

---

## Task 1 — Lambda Expressions in Java (25 minutes)

Watch the following videos on Lambda Expressions in Java:

- 📹 [Lambda Expressions in Java — (https://www.youtube.com/watch?v=tj5sLSFjVj4)]
  


While watching, refer to **lesson.md Part 2** and pay attention to:
- What problem lambda expressions solve compared to the old verbose syntax
- The three parts of a lambda: parameters, arrow `->`, and body
- The different lambda formats (no params, one param, multiple params, multiple statements)

**Guiding Questions:**
1. What does the `->` arrow mean in a lambda expression?
2. When do you need curly braces `{}` in a lambda body?
3. How would you rewrite a `for` loop that prints each item in a list using a lambda?

---

## Task 2 — Functional Interfaces in Java (20 minutes)

Watch the following video on Functional Interfaces in Java:

- 📹 [Predicate, Function and Consumer in Java — 
 https://www.youtube.com/watch?v=mrTjoF3-j0s]

While watching, refer to **lesson.md Part 3** and pay attention to:
- What makes an interface a "functional interface" (exactly one abstract method)
- The difference between Predicate (test), Function (apply), and Consumer (accept)
- How to assign a lambda to a functional interface variable and call it

**Guiding Questions:**
1. What is the difference between `Predicate` and `Function`?
2. What does `Consumer` return, and when would you use it?
3. Why is the `@FunctionalInterface` annotation useful even though it is optional?

---

## Task 3 — Stream API in Java (25 minutes)

Watch the following videos on the Stream API in Java:

- 📹 [Java Streams in 5 Minutes — https://www.youtube.com/watch?v=2StXP1XaU04]
 



While watching, refer to **lesson.md Part 4** and pay attention to:
- The three steps of a stream: create → transform → collect
- The difference between intermediate operations (filter, map, sorted) and terminal operations (collect, forEach, count)
- How to chain multiple operations together into a pipeline

**Guiding Questions:**
1. What is the difference between an intermediate operation and a terminal operation?
2. What does `collect(Collectors.toList())` do at the end of a stream?
3. How would you use a stream to get only the even numbers from a list and square them?

---

## Active Engagement Strategies

- Pause and code along with each video in VS Code
- After each video, close it and try to recreate the example from memory
- Use the guiding questions to check your understanding before class

---

## Additional Reading Material

- [Java Lambda Expressions — W3Schools](https://www.w3schools.com/java/java_lambda.asp)
- [Java Functional Interfaces — Baeldung](https://www.baeldung.com/java-8-functional-interfaces)
- [Java Stream API — W3Schools](https://www.w3schools.com/java/java_stream.asp)
- [Java Stream API — Oracle Docs](https://docs.oracle.com/javase/8/docs/api/java/util/stream/Stream.html)