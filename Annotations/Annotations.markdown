# Java Annotations Guide

This guide provides an in-depth exploration of **Annotations** in Java, a powerful feature for adding metadata to code, enabling enhanced functionality, and supporting tools and frameworks. The explanation is tailored to align with the `Library` system used in prior responses, ensuring continuity. Examples are provided to illustrate key concepts, and key points are highlighted to facilitate understanding.

## Annotations

**Annotations** in Java are a form of metadata that provide additional information about code elements (e.g., classes, methods, fields) without directly affecting their runtime behavior. Introduced in Java 5, annotations are used by compilers, tools, and frameworks to enforce rules, generate code, or configure behavior.

### Explanation
- **Definition**: Annotations are special tags (starting with `@`) applied to code elements to convey metadata, such as instructions for the compiler, runtime processing, or documentation.
- **Syntax**: Annotations are declared using the `@interface` keyword and applied using `@AnnotationName`. They can include elements (key-value pairs) to pass configuration data.
- **Types**:
  - **Built-in Annotations**: Provided by Java (e.g., `@Override`, `@Deprecated`, `@SuppressWarnings`).
  - **Meta-Annotations**: Annotations that describe other annotations (e.g., `@Retention`, `@Target`).
  - **Custom Annotations**: User-defined annotations for specific purposes.
- **Retention Policies** (defined by `@Retention`):
  - `SOURCE`: Retained only in source code, discarded during compilation (e.g., `@Override`).
  - `CLASS`: Retained in compiled `.class` files but not available at runtime (rarely used).
  - `RUNTIME`: Retained at runtime, accessible via reflection (e.g., for frameworks like Spring).
- **Target Elements** (defined by `@Target`): Specifies where the annotation can be applied (e.g., `TYPE`, `METHOD`, `FIELD`).
- **Use Cases**:
  - Compiler directives (e.g., `@Override` ensures method overriding).
  - Code generation (e.g., Lombok’s `@Data` generates boilerplate code).
  - Runtime processing (e.g., JUnit’s `@Test` marks test methods).
  - Documentation and validation (e.g., `@Deprecated` marks obsolete code).

### Key Points
- Annotations do not directly affect program logic but influence how code is processed by tools, compilers, or runtime environments.
- Use `@Retention(RetentionPolicy.RUNTIME)` for annotations processed via reflection (e.g., in frameworks).
- Annotations with no elements can be applied without parentheses (e.g., `@Override`); those with elements use key-value pairs (e.g., `@Test(timeout = 100)`).
- Custom annotations require a processor (e.g., reflection or annotation processor) to interpret and act on the metadata.
- Overusing annotations can make code harder to read; use them judiciously for clear, specific purposes.

### Example
The following example, based on the `Library` system, demonstrates built-in and custom annotations. A custom `LibraryInfo` annotation adds metadata to `Book` and `TextBook` classes, and built-in annotations like `@Override` and `@Deprecated` are used.

```java
import java.lang.annotation.ElementType;
import java.lang.annotation.Retention;
import java.lang.annotation.RetentionPolicy;
import java.lang.annotation.Target;
import java.lang.reflect.Method;

// Custom annotation
@Retention(RetentionPolicy.RUNTIME)
@Target({ElementType.TYPE, ElementType.METHOD})
@interface LibraryInfo {
    String category() default "General";
    int version() default 1;
}

// Abstract class for library items
public abstract class LibraryItem {
    protected String title;
    protected int copiesAvailable;

    public LibraryItem(String title, int copiesAvailable) {
        this.title = title;
        this.copiesAvailable = copiesAvailable;
    }

    public abstract void displayInfo();
}

// Book class with annotations
@LibraryInfo(category = "Literature", version = 2)
public class Book extends LibraryItem {
    private String author;

    public Book(String title, String author, int copiesAvailable) {
        super(title, copiesAvailable);
        this.author = author;
    }

    @Override
    public void displayInfo() {
        System.out.println("Book - Title: " + title + ", Author: " + author + ", Copies: " + copiesAvailable);
    }

    @Deprecated
    public void oldBorrowMethod() {
        System.out.println("Deprecated borrowing method.");
    }
}

// TextBook subclass with annotations
@LibraryInfo(category = "Academic", version = 1)
public class TextBook extends Book {
    private String subject;

    public TextBook(String title, String author, int copiesAvailable, String subject) {
        super(title, author, copiesAvailable);
        this.subject = subject;
    }

    @Override
    @LibraryInfo(category = "Method", version = 3)
    public void displayInfo() {
        System.out.println("TextBook - Title: " + title + ", Author: " + author + ", Subject: " + subject + ", copies: " + copiesAvailable);
    }
}

class Library {
    public static void main(String[] args) {
        // Creating objects
        Book book = new Book("1984", "George Orwell", 3);
        TextBook textBook = new TextBook("Calculus", "James Stewart", 2, "Mathematics");

        // Runtime polymorphism with annotations
        book.displayInfo();
        // Outputs: Book - Title: 1984, Author: George Orwell, Copies: 3
        textBook.displayInfo();
        // Outputs: TextBook - Title: Calculus, Author: James Stewart, Subject: Mathematics, Copies: 2

        // Using deprecated method (compiler warning)
        book.oldBorrowMethod();
        // Outputs: Deprecated borrowing method.

        // Processing custom annotations via reflection
        System.out.println("Annotation Info:");
        processAnnotations(book.getClass());
        processAnnotations(textBook.getClass());
    }

    // Method to process annotations using reflection
    private static void processAnnotations(Class<?> clazz) {
        // Class-level annotation
        if (clazz.isAnnotationPresent(LibraryInfo.class)) {
            LibraryInfo info = clazz.getAnnotation(LibraryInfo.class);
            System.out.println("Class: " + clazz.getSimpleName() + ", Category: " + info.category() + ", Version: " + info.version());
        }

        // Method-level annotations
        for (Method method : clazz.getDeclaredMethods()) {
            if (method.isAnnotationPresent(LibraryInfo.class)) {
                LibraryInfo info = method.getAnnotation(LibraryInfo.class);
                System.out.println("Method: " + method.getName() + ", Category: " + info.category() + ", Version: " + info.version());
            }
        }
    }
}
```

### Explanation
- **Built-in Annotations**:
  - `@Override`: Ensures `displayInfo` in `Book` and `TextBook` correctly overrides the superclass method, preventing errors (e.g., mismatched signatures).
  - `@Deprecated`: Marks `oldBorrowMethod` as obsolete, triggering compiler warnings when used, encouraging developers to adopt newer methods.
- **Custom Annotation**:
  - `LibraryInfo` is defined with `@Retention(RetentionPolicy.RUNTIME)` to be accessible at runtime and `@Target` to restrict usage to classes and methods.
  - It includes `category` and `version` elements with default values.
  - Applied to `Book`, `TextBook`, and `TextBook`’s `displayInfo` method to provide metadata.
- **Reflection**:
  - The `processAnnotations` method uses Java’s reflection API to inspect `LibraryInfo` annotations on classes and methods, printing their `category` and `version`.
  - Outputs for the example:
    ```
    Class: Book, Category: Literature, Version: 2
    Class: TextBook, Category: Academic, Version: 1
    Method: displayInfo, Category: Method, Version: 3
    ```
- **Library System**:
  - The `Book` and `TextBook` classes demonstrate polymorphism, with annotations adding metadata about their category and version.
  - The `Library` class processes annotations to simulate how frameworks might use metadata (e.g., for configuration or validation).

### Additional Notes
- **Common Built-in Annotations**:
  - `@SuppressWarnings`: Suppresses compiler warnings (e.g., `@SuppressWarnings("unchecked")` for unchecked type casts).
  - `@FunctionalInterface`: Ensures an interface has exactly one abstract method, suitable for lambda expressions.
- **Annotation Processing**:
  - Custom annotations often require an annotation processor (e.g., for compile-time code generation) or reflection (for runtime processing).
  - Frameworks like Spring and Hibernate heavily rely on annotations (e.g., `@Autowired`, `@Entity`) for configuration.
- **Best Practices**:
  - Use clear, descriptive names for custom annotations.
  - Specify appropriate retention and target policies to match the use case.
  - Avoid overusing annotations, as they can obscure code intent if misapplied.
- **Limitations**:
  - Annotations cannot inherit from other annotations (though meta-annotations can be applied).
  - Runtime reflection can impact performance, so use sparingly in performance-critical code.
- **Real-World Use**:
  - Annotations are integral to frameworks (e.g., JUnit’s `@Test`, Spring’s `@Controller`) and tools like Lombok, which use them to reduce boilerplate code.

## Summary
Annotations in Java provide metadata to enhance code functionality, supporting compiler checks, code generation, and runtime processing. They include:
- **Built-in Annotations**: Like `@Override` and `@Deprecated`, for compiler directives and documentation.
- **Custom Annotations**: User-defined metadata, processed via reflection or annotation processors.
- **Meta-Annotations**: Like `@Retention` and `@Target`, to configure annotation behavior.

The `Library` example demonstrates annotations in the context of `Book` and `TextBook`, using `LibraryInfo` to add metadata and reflection to process it, alongside built-in annotations for method overriding and deprecation. Annotations are a versatile tool for building robust, maintainable, and framework-friendly Java applications.