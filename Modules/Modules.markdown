# Java Modules Guide

This guide provides a clear overview of **Modules** in Java, a feature introduced in Java 9 to improve code organization, encapsulation, and scalability. The explanation aligns with the `Library` system used in prior responses for continuity. Examples illustrate key concepts, and key points aid understanding.

## Modules

**Modules** in Java are a way to group related packages and resources into a single unit, enhancing modularity and encapsulation. Part of the Java Platform Module System (JPMS), modules define dependencies and control access to packages, reducing runtime errors and improving maintainability.

### Explanation
- **Definition**: A module is a named collection of packages, classes, and resources (e.g., configuration files) with a `module-info.java` file that specifies its dependencies, exported packages, and services.
- **Components**:
  - **`module-info.java`**: The module descriptor, declaring the module’s name, dependencies (`requires`), exported packages (`exports`), and services (`uses`, `provides`).
  - **Packages**: Grouped within the module, with controlled access.
- **Key Features**:
  - **Strong Encapsulation**: Only exported packages are accessible to other modules; non-exported packages are hidden.
  - **Explicit Dependencies**: Modules declare dependencies using `requires`, ensuring clarity and preventing runtime errors like `ClassNotFoundException`.
  - **Modular JDK**: The JDK itself is modularized (e.g., `java.base`, `java.sql`), allowing developers to include only needed modules.
- **Use Cases**: Large-scale applications, libraries, and frameworks benefit from modules for better structure, reduced classpath issues, and improved security.
- **Syntax**:
  - `module <module-name> { ... }`: Defines a module.
  - `requires <module>`: Declares a dependency.
  - `exports <package>`: Makes a package accessible to other modules.
  - `opens <package>`: Allows reflective access to a package.
  - `provides <service> with <implementation>`: Registers a service implementation.

### Key Points
- Modules replace the brittle classpath with a robust dependency system.
- Use `exports` to share packages; unexported packages are inaccessible, enhancing encapsulation.
- `requires transitive` implies dependencies for downstream modules, simplifying dependency management.
- Modules support backward compatibility via the unnamed module (for non-modular code) and automatic modules (for JARs without `module-info.java`).
- Tools like `jdeps`, `jlink`, and `jmod` help analyze, package, and create custom runtimes for modular applications.

### Example
The `Library` system is organized into two modules: `library.model` (for `Book` and related classes) and `library.app` (for the application logic). The example shows how modules encapsulate and depend on each other.

#### Directory Structure
```
library-system/
├── library.model/
│   ├── src/
│   │   ├── com/library/model/
│   │   │   └── Book.java
│   │   ├── module-info.java
├── library.app/
│   ├── src/
│   │   ├── com/library/app/
│   │   │   └── Library.java
│   │   ├── module-info.java
```

#### `library.model` Module
**`library.model/src/module-info.java`**:
```java
module library.model {
    exports com.library.model; // Export Book class
}
```

**`library.model/src/com/library/model/Book.java`**:
```java
package com.library.model;

public class Book {
    private String title;

    public Book(String title) {
        this.title = title;
    }

    public String getTitle() {
        return title;
    }

    public String toString() {
        return "Book: " + title;
    }
}
```

#### `library.app` Module
**`library.app/src/module-info.java`**:
```java
module library.app {
    requires library.model; // Depend on library.model
}
```

**`library.app/src/com/library/app/Library.java`**:
```java
package com.library.app;

import com.library.model.Book;

public class Library {
    public static void main(String[] args) {
        Book book = new Book("1984");
        System.out.println(book);
    }
}
```

#### Compilation and Execution
To compile and run (from `library-system` directory):
```bash
# Compile modules
javac -d mods/library.model library.model/src/module-info.java library.model/src/com/library/model/Book.java
javac --module-path mods -d mods/library.app library.app/src/module-info.java library.app/src/com/library/app/Library.java

# Run
java --module-path mods -m library.app/com.library.app.Library
```

**Output**:
```
Book: 1984
```

### Explanation
- **Modules**:
  - `library.model` contains the `Book` class and exports `com.library.model` for access.
  - `library.app` depends on `library.model` via `requires` and uses `Book`.
- **Encapsulation**: Only `com.library.model` is exported; other packages in `library.model` (if any) are inaccessible.
- **Dependency**: `library.app` explicitly declares its need for `library.model`, ensuring all required classes are available.
- **Modular Execution**: The `--module-path` specifies where compiled modules are located, and `-m` runs the main class.

### Additional Notes
- **Unnamed Module**: Non-modular code (e.g., JARs without `module-info.java`) runs in the unnamed module, accessing all modules but lacking encapsulation.
- **Automatic Modules**: A JAR without `module-info.java` on the module path becomes an automatic module, deriving its name from the JAR filename.
- **JLink**: Create a custom runtime with only needed modules using `jlink`, reducing application size.
- **Challenges**: Migrating legacy code to modules requires careful planning due to encapsulation and dependency changes.
- **Real-World Use**: Frameworks like Spring and libraries like JavaFX leverage modules for better structure and lightweight deployments.

## Summary
Java Modules, introduced in Java 9, provide a robust system for organizing code into encapsulated, reusable units. Key features include:
- **Strong Encapsulation**: Hiding internal packages with `exports`.
- **Explicit Dependencies**: Declaring dependencies with `requires`.
- **Modular JDK**: Enabling lightweight applications with only necessary modules.

The `Library` example demonstrates modules with `library.model` and `library.app`, showing how to structure, compile, and run a modular application. Modules enhance scalability, maintainability, and reliability in large Java projects.