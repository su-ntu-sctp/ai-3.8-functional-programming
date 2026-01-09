# Assignment (Optional)

## Brief

Create a program called FunctionalProgrammingAssignment.java and solve the following problems using lambda expressions, functional interfaces, and the Stream API.

1. **Student Grade Filter and Transform**
   - Create a `Student` class (or record) with attributes:
     - `String name`
     - `int age`
     - `double grade`
   - Create an `ArrayList` with at least 8 Student objects with varying grades (between 0-100)
   - Use Stream API to:
     - Filter students who have grades above 70
     - Transform the filtered students to only show their names in UPPERCASE
     - Sort the names alphabetically
     - Collect and print the final result
   - Use lambda expressions and method references where possible
   - Example output: `[ALICE, BOB, CHARLIE]`

2. **Functional Interface Operations**
   - Create a list of integers from 1 to 15
   - Use a `Predicate<Integer>` to check if a number is divisible by both 2 and 3
   - Use a `Function<Integer, String>` to convert numbers to strings with format: "Number: X"
   - Use a `Consumer<String>` to print each formatted string
   - Apply these functional interfaces in sequence:
     - Filter numbers using the Predicate (keep only those divisible by both 2 and 3)
     - Transform them using the Function
     - Print them using the Consumer
   - Expected output:
```
     Number: 6
     Number: 12
```

## Submission (Optional)

- Submit the URL of the GitHub Repository that contains your work to NTU black board.
- Should you reference the work of your classmate(s) or online resources, give them credit by adding either the name of your classmate or URL.

## References
- Java: https://docs.oracle.com/javase/
- Spring Boot: https://docs.spring.io/spring-boot/docs/current/reference/html/
- PostgreSQL: https://www.postgresql.org/docs/
- OWASP: https://cheatsheetseries.owasp.org/