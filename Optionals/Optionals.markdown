# Optionals in Java

## Overview
Optionals, introduced in Java 8 under `java.util`, handle potentially absent values, reducing `NullPointerException` risks. They wrap values that might be `null`, enhancing code safety.

## Purpose
Optionals explicitly represent the possibility of absence, aligning with the Null Object pattern. They reduce null checks and improve readability, especially in functional programming.

## Key Methods
- `isPresent()`: Checks if a value exists.
- `get()`: Retrieves value, throws `NoSuchElementException` if absent.
- `orElse(T other)`: Returns value or default if absent.
- `orElseGet(Supplier)`: Lazily computes default if absent.
- `ifPresent(Consumer)`: Executes action if value present.
- `map(Function)`: Transforms value, returns new Optional.
- `filter(Predicate)`: Returns Optional if value matches predicate.

## Creation
- `Optional.of(T value)`: Non-null value, throws `NullPointerException` if `null`.
- `Optional.ofNullable(T value)`: Handles `null`, returns empty Optional if `null`.
- `Optional.empty()`: Represents absence.

## Usage
Best for method returns where `null` is possible, e.g., retrieving a book:
```java
public Optional<Book> getBookById(int id) {
    return Optional.ofNullable(bookMap.get(id));
}
```
Handle results with:
```java
Optional<Book> book = library.getBookById(123);
book.ifPresent(b -> System.out.println("Found: " + b.getTitle()));
Book result = book.orElse(new Book("Default"));
```

## Example
```java
import java.util.HashMap;
import java.util.Optional;

public class Library {
    private HashMap<Integer, Book> bookMap = new HashMap<>();

    public Optional<Book> getBookById(int id) {
        return Optional.ofNullable(bookMap.get(id));
    }

    public static void main(String[] args) {
        Library library = new Library();
        library.bookMap.put(1, new Book("1984"));

        Optional<Book> book = library.getBookById(1);
        book.ifPresent(b -> System.out.println("Book found: " + b.getTitle()));
        // Outputs: Book found: 1984

        Optional<Book> missing = library.getBookById(999);
        Book defaultBook = missing.orElse(new Book("Default Book"));
        System.out.println("Default book: " + defaultBook.getTitle());
        // Outputs: Default book: Default Book
    }
}

class Book {
    private String title;

    public Book(String title) {
        this.title = title;
    }

    public String getTitle() {
        return title;
    }
}
```

## Best Practices
- Use `Optional.ofNullable()` for nullable values, not `Optional.of()`.
- Avoid wrapping every variable in Optional.
- Document expected Optional behavior in APIs.
- Use `map` and `flatMap` for transformations.
- Limit use in performance-critical code due to object creation overhead.

## Benefits
- Clarifies code intent.
- Forces handling of absent values.
- Reduces null checks.

## Limitations
- Overuse can clutter code.
- Minor performance overhead from object creation.

## Conclusion
Optionals enhance code safety and readability for handling absent values, ideal for method returns and APIs. Use judiciously to balance clarity and performance.