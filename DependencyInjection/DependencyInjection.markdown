# Dependency Injection in Java

## Overview
Dependency Injection (DI) is a design pattern in Java that implements Inversion of Control (IoC) to manage dependencies. Instead of a class creating its dependencies, they are injected from an external source, promoting loose coupling, testability, and maintainability. DI is widely used in frameworks like Spring and Java EE.

## Purpose
DI decouples object creation from usage, enabling:
- **Flexibility**: Easily swap implementations.
- **Testability**: Mock dependencies for unit testing.
- **Maintainability**: Cleaner, modular code.
- **Scalability**: Simplifies managing complex dependencies.

## Core Concepts
- **Dependencies**: Objects a class needs to function (e.g., a service or repository).
- **Injector**: External entity (e.g., DI framework or manual code) that provides dependencies.
- **Inversion of Control (IoC)**: Control of dependency creation is inverted to an external entity.
- **Types of Injection**:
  - **Constructor Injection**: Dependencies passed via constructor.
  - **Setter Injection**: Dependencies set via setter methods.
  - **Field Injection**: Dependencies injected directly into fields (less recommended).

## How DI Works
1. Define interfaces for dependencies to ensure loose coupling.
2. Create concrete implementations.
3. Configure an injector (e.g., Spring’s IoC container) to provide dependencies.
4. Inject dependencies into the consuming class.

## Benefits
- Reduces tight coupling between classes.
- Simplifies unit testing with mock objects.
- Enhances code reuse and modularity.
- Centralizes dependency management.

## Drawbacks
- Adds complexity for small projects.
- Requires learning curve for DI frameworks.
- Overuse can obscure dependency flow.

## Implementation Approaches
1. **Manual DI**: Inject dependencies without a framework.
2. **Framework-based DI**: Use Spring, Guice, or CDI (Java EE).
3. **Annotation-based**: Common in frameworks like Spring (`@Autowired`).

## Example: Manual Dependency Injection
Consider a `LibraryService` that depends on a `BookRepository`.

### Without DI (Tight Coupling)
```java
public class LibraryService {
    private BookRepository bookRepository = new BookRepositoryImpl(); // Hard-coded dependency

    public Book findBook(int id) {
        return bookRepository.findById(id);
    }
}
```
Problem: Hard to test or swap `BookRepositoryImpl`.

### With DI (Constructor Injection)
```java
public interface BookRepository {
    Book findById(int id);
}

public class BookRepositoryImpl implements BookRepository {
    public Book findById(int id) {
        // Simulate database lookup
        return new Book(id, "Book Title");
    }
}

public class LibraryService {
    private final BookRepository bookRepository;

    // Constructor Injection
    public LibraryService(BookRepository bookRepository) {
        this.bookRepository = bookRepository;
    }

    public Book findBook(int id) {
        return bookRepository.findById(id);
    }
}

public class Book {
    private int id;
    private String title;

    public Book(int id, String title) {
        this.id = id;
        this.title = title;
    }

    public String getTitle() {
        return title;
    }
}

public class Main {
    public static void main(String[] args) {
        // Manual DI
        BookRepository bookRepository = new BookRepositoryImpl();
        LibraryService libraryService = new LibraryService(bookRepository);
        Book book = libraryService.findBook(1);
        System.out.println("Found: " + book.getTitle());
    }
}
```
**Explanation**:
- `LibraryService` depends on `BookRepository` (interface).
- Dependency is injected via constructor, allowing flexibility to swap implementations.
- Enables easy mocking for tests.

## Example: Spring Framework DI
Using Spring Boot with annotation-based DI.

### Setup
Add Spring Boot dependency (e.g., via Maven):
```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter</artifactId>
    <version>3.3.0</version>
</dependency>
```

### Code
```java
import org.springframework.stereotype.Component;
import org.springframework.stereotype.Repository;
import org.springframework.stereotype.Service;
import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;
import org.springframework.context.annotation.Bean;

interface BookRepository {
    Book findById(int id);
}

@Repository
class BookRepositoryImpl implements BookRepository {
    public Book findById(int id) {
        return new Book(id, "Book Title");
    }
}

@Service
class LibraryService {
    private final BookRepository bookRepository;

    // Constructor Injection with Spring
    public LibraryService(BookRepository bookRepository) {
        this.bookRepository = bookRepository;
    }

    public Book findBook(int id) {
        return bookRepository.findById(id);
    }
}

class Book {
    private int id;
    private String title;

    public Book(int id, String title) {
        this.id = id;
        this.title = title;
    }

    public String getTitle() {
        return title;
    }
}

@SpringBootApplication
public class Application {
    public static void main(String[] args) {
        var context = SpringApplication.run(Application.class, args);
        LibraryService libraryService = context.getBean(LibraryService.class);
        Book book = libraryService.findBook(1);
        System.out.println("Found: " + book.getTitle());
    }
}
```
**Explanation**:
- `@Repository` and `@Service` mark beans for Spring’s IoC container.
- Spring automatically injects `BookRepository` into `LibraryService` via constructor.
- `@SpringBootApplication` enables auto-configuration and component scanning.

## Best Practices
- Prefer **constructor injection** for mandatory dependencies.
- Use interfaces to define dependencies for flexibility.
- Avoid field injection (`@Autowired` on fields) due to testability issues.
- Keep dependency graphs simple to avoid circular dependencies.
- Document dependency requirements in APIs.
- Use DI frameworks for large projects, manual DI for small ones.

## Testing with DI
DI simplifies testing by allowing mock dependencies:
```java
import org.junit.jupiter.api.Test;
import static org.mockito.Mockito.*;

class LibraryServiceTest {
    @Test
    void testFindBook() {
        // Mock dependency
        BookRepository bookRepository = mock(BookRepository.class);
        when(bookRepository.findById(1)).thenReturn(new Book(1, "Mock Book"));

        // Inject mock
        LibraryService libraryService = new LibraryService(bookRepository);

        // Test
        Book book = libraryService.findBook(1);
        assert book.getTitle().equals("Mock Book");
    }
}
```
**Explanation**:
- Mockito creates a mock `BookRepository`.
- Injected into `LibraryService` for testing without real database access.

## Performance Considerations
- DI frameworks add startup overhead due to bean creation and wiring.
- Minimal runtime impact for most applications.
- Manual DI avoids framework overhead but requires more boilerplate.

## Historical Context
- DI gained prominence with frameworks like Spring (2004) and Guice (2008).
- Java EE’s CDI (Context and Dependency Injection) standardized DI in Java.
- As of May 20, 2025, Spring remains the dominant DI framework, with Spring Boot simplifying configuration.

## Common Pitfalls
- Overusing DI can lead to complex configurations.
- Circular dependencies cause runtime errors (e.g., Spring’s `BeanCurrentlyInCreationException`).
- Field injection hides dependencies, complicating testing.

## Conclusion
Dependency Injection in Java enhances modularity, testability, and maintainability by decoupling classes from their dependencies. Manual DI suits small projects, while frameworks like Spring excel in large applications. By following best practices and leveraging constructor injection, developers can create robust, testable systems.