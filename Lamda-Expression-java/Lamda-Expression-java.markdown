# Lambda Expressions in Java

## Introduction
Lambda expressions, introduced in Java 8, let you write concise, functional-style code by representing functions as expressions. They’re ideal for tasks like processing collections, handling events, or passing behavior to methods.

**Analogy**: Imagine you’re hiring a worker for a one-time task. Instead of writing a detailed job contract (a full method or class), you give a quick instruction like “sort these boxes by size” (a lambda). It’s fast and gets the job done.

## Core Principles
- **Lambda Expression**: A shorthand for an anonymous function, defined as `(parameters) -> body`.
- **Functional Interface**: An interface with one abstract method (e.g., `Runnable`, `Comparator`). Lambdas implement these interfaces.
- **Use Cases**: Simplify code for sorting, filtering, event handling, or parallel processing.
- **Benefits**: Less boilerplate, supports functional programming, improves readability.

## Syntax
```java
// Single parameter, single expression
x -> x * 2

// Multiple parameters, block of code
(x, y) -> {
    int sum = x + y;
    return sum;
}

// No parameters
() -> System.out.println("Hello!")
```

## Practical Example: Sorting a List
Let’s sort a list of names alphabetically using a lambda expression.

### Without Lambda (Anonymous Class)
```java
import java.util.Arrays;
import java.util.Comparator;
import java.util.List;

public class SortWithoutLambda {
    public static void main(String[] args) {
        List<String> names = Arrays.asList("Bob", "Alice", "Charlie");
        names.sort(new Comparator<String>() {
            @Override
            public int compare(String a, String b) {
                return a.compareTo(b); // Sort alphabetically
            }
        });
        System.out.println(names);
    }
}
```

### With Lambda
```java
import java.util.Arrays;
import java.util.List;

public class SortWithLambda {
    public static void main(String[] args) {
        List<String> names = Arrays.asList("Bob", "Alice", "Charlie");
        names.sort((a, b) -> a.compareTo(b)); // Lambda for sorting
        System.out.println(names);
    }
}
```

**Output** (both cases):
```
[Alice, Bob, Charlie]
```

**What’s Happening?**
- The lambda `(a, b) -> a.compareTo(b)` replaces the verbose `Comparator` implementation.
- It defines how to compare two strings (`a` and `b`) in one line.

## Comparison: Anonymous Class vs. Lambda
| **Anonymous Class** | **Lambda Expression** |
|---------------------|-----------------------|
| Verbose, lots of boilerplate. | Concise, minimal code. |
| Explicit method declaration. | Inline behavior. |
| Harder to read for simple tasks. | Clear for functional tasks. |

**Best Practice**: Use lambdas for functional interfaces to reduce code and improve clarity.

## Using Lambdas with Streams
Lambdas shine with Java’s Stream API for processing collections. Here’s an example filtering names starting with “A”:

```java
import java.util.Arrays;
import java.util.List;
import java.util.stream.Collectors;

public class StreamWithLambda {
    public static void main(String[] args) {
        List<String> names = Arrays.asList("Alice", "Bob", "Andrew");
        List<String> filtered = names.stream()
                                    .filter(name -> name.startsWith("A")) // Lambda for filtering
                                    .collect(Collectors.toList());
        System.out.println(filtered);
    }
}
```

**Output**:
```
[Alice, Andrew]
```

**Key Points**:
- The lambda `name -> name.startsWith("A")` acts as a `Predicate` to filter names.
- Streams + lambdas make data processing concise and readable.

## Creating a Custom Functional Interface
You can define your own functional interface and use lambdas with it:

```java
@FunctionalInterface
interface MathOperation {
    int operate(int a, int b);
}

public class CustomLambda {
    public static void main(String[] args) {
        // Lambda for addition
        MathOperation add = (a, b) -> a + b;
        // Lambda for multiplication
        MathOperation multiply = (a, b) -> a * b;

        System.out.println("Add: " + add.operate(5, 3)); // 8
        System.out.println("Multiply: " + multiply.operate(5, 3)); // 15
    }
}
```

**Output**:
```
Add: 8
Multiply: 15
```

**Why?** Lambdas let you swap behaviors (add vs. multiply) without writing separate classes.

## Gotchas and Best Practices
1. **Don’t Overuse Lambdas**:
   - **Bad**: Complex logic in a lambda:
     ```java
     (x, y) -> {
         // 20 lines of complex code
         return result;
     }
     ```
     - **Why?** Hard to read and debug. Move complex logic to a method.

2. **Keep Lambdas Focused**:
   - A lambda should do one thing (e.g., compare, filter, compute). Split multiple tasks into separate lambdas or methods.

3. **Avoid Side Effects**:
   - **Bad**: Modifying external state:
     ```java
     List<String> results = new ArrayList<>();
     names.forEach(name -> results.add(name.toUpperCase()));
     ```
     - **Why?** Side effects make code unpredictable. Use Stream’s `map` and `collect` instead.

4. **Use Method References for Clarity**:
   - Instead of:
     ```java
     names.forEach(name -> System.out.println(name));
     ```
   - Use:
     ```java
     names.forEach(System.out::println); // Method reference
     ```

5. **Understand Variable Capture**:
   - Lambdas can use variables from their enclosing scope, but they must be **effectively final** (not reassigned).
     ```java
     int count = 0;
     // names.forEach(name -> count++); // Error: count must be final
     ```

**Best Practice**: Use lambdas for short, clear tasks. For complex logic, use traditional methods or classes.

## Visual Explanation
To understand lambdas, sketch this mental model:
- **Traditional Method**: A named box with fixed instructions (e.g., a `compare` method).
- **Lambda**: A sticky note with quick instructions (e.g., `(a, b) -> a - b`) you pass to a method like `sort`.

**Diagram Suggestion**:
```
[Method Call: sort]
       |
[Lambda: (a, b) -> a.compareTo(b)]
       |
[Functional Interface: Comparator]
       |
[Execution: Compare strings]
```

## Learning Path
To master lambda expressions:
1. **Learn the Basics**:
   - Read [Oracle’s Lambda Tutorial](https://docs.oracle.com/javase/tutorial/java/javaOO/lambdaexpressions.html).
   - Practice writing lambdas for `Runnable` and `Comparator`.

2. **Explore Built-in Functional Interfaces**:
   - Study `Predicate`, `Function`, `Consumer`, and `Supplier` in `java.util.function`.
   - Example: Use `Predicate` to filter a list.

3. **Master Streams with Lambdas**:
   - Write a program that filters, maps, and reduces a collection (e.g., sum numbers > 10).
   - Try parallel streams for performance.

4. **Create Custom Functional Interfaces**:
   - Define an interface like `MathOperation` and use lambdas to implement different behaviors.

5. **Study Real-World Code**:
   - Explore [Spring Boot](https://github.com/spring-projects/spring-boot) or [Java Stream Examples](https://github.com/openjdk/jdk) on GitHub.
   - Search for `->` to find lambda usage.

6. **Tools**:
   - Use IntelliJ IDEA to auto-suggest lambda conversions (e.g., from anonymous classes).
   - Use JShell to experiment with lambdas interactively.

7. **Practice Problems**:
   - Solve Stream-based problems on LeetCode (e.g., filter or group data).
   - Build a program that processes a CSV file using lambdas and Streams.

## Suggested Project
Build a **command-line task manager**:
- Store tasks in a list (e.g., `{id, description, priority}`).
- Use lambdas to:
  - Sort tasks by priority.
  - Filter tasks by keyword.
  - Map tasks to a summary format.
- Use Streams for processing and a custom functional interface for task operations.
- Example:
  ```java
  tasks.stream()
       .filter(task -> task.getPriority() > 5)
       .sorted((t1, t2) -> t2.getPriority() - t1.getPriority())
       .forEach(System.out::println);
  ```

## Common Functional Interfaces
| Interface | Method | Lambda Example | Use Case |
|-----------|--------|----------------|----------|
| `Predicate<T>` | `boolean test(T t)` | `x -> x > 0` | Filtering |
| `Function<T, R>` | `R apply(T t)` | `x -> x * 2` | Transforming |
| `Consumer<T>` | `void accept(T t)` | `x -> System.out.println(x)` | Side effects |
| `Supplier<T>` | `T get()` | `() -> new Random().nextInt()` | Providing values |

## Next Steps
Try writing a program that uses lambdas with Streams to process data or create a custom functional interface for a specific use case. Let me know if you want to dive deeper into Streams, method references, or a specific lambda use case!