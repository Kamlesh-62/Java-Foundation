# Java Object-Oriented Programming (OOP) Guide

This guide provides an in-depth exploration of Object-Oriented Programming (OOP) concepts in Java, tailored for beginners and intermediate learners. Each section includes detailed explanations, key points for better understanding, and examples based on a consistent `Library` system to demonstrate how OOP principles are applied.

## 2.1 Basics of OOP
Object-Oriented Programming is a paradigm that organizes code into objects, which are instances of classes. Java is inherently object-oriented, and OOP is built on four core principles:

- **Encapsulation**: Bundling data (attributes) and methods that operate on the data within a class, controlling access to protect the data's integrity.
- **Inheritance**: Allowing a class to inherit attributes and methods from another class, promoting code reuse.
- **Polymorphism**: Enabling objects to be treated as instances of their parent class, with the ability to override or overload methods for flexibility.
- **Abstraction**: Hiding complex implementation details and exposing only necessary functionality through interfaces or abstract classes.

**Key Points**:
- OOP models real-world entities (e.g., a library with books and members) as objects.
- Classes define blueprints, while objects are instances of those blueprints.
- OOP improves modularity, maintainability, and scalability of code.

**Example Context**: Throughout this guide, we will use a `Library` system where a `Book` class represents books with attributes like title and author, and methods to manage them. This example will evolve to demonstrate each OOP concept.

## 2.1.1 Classes and Objects
A **class** is a blueprint for creating objects, defining their structure (attributes) and behavior (methods). An **object** is an instance of a class, representing a specific entity based on the class's blueprint.

- **Class Declaration**: Use the `class` keyword, followed by the class name and a body containing attributes and methods.
- **Object Creation**: Use the `new` keyword with the class constructor to instantiate an object.

**Key Points**:
- Classes are templates; objects are concrete instances with their own data.
- Constructors initialize objects, often setting initial values for attributes.
- Multiple objects can be created from the same class, each with independent data.

**Example**:
```java
public class Book {
    // Attributes
    String title;
    String author;

    // Constructor
    public Book(String title, String author) {
        this.title = title;
        this.author = author;
    }

    // Method
    public void displayInfo() {
        System.out.println("Title: " + title + ", Author: " + author);
    }
}

class Library {
    public static void main(String[] args) {
        // Creating objects
        Book book1 = new Book("1984", "George Orwell");
        Book book2 = new Book("Pride and Prejudice", "Jane Austen");

        // Calling methods
        book1.displayInfo(); // Outputs: Title: 1984, Author: George Orwell
        book2.displayInfo(); // Outputs: Title: Pride and Prejudice, Author: Jane Austen
    }
}
```
In this example, `Book` is a class with attributes (`title`, `author`) and a method (`displayInfo`). Two objects (`book1`, `book2`) are created, each with distinct data.

## 2.1.2 Attributes and Methods
**Attributes** (or fields) are variables defined within a class that store an object's state. **Methods** are functions defined within a class that define an object's behavior.

- **Attributes**: Typically declared at the class level, they hold data specific to each object (instance variables) or shared across all objects (static variables).
- **Methods**: Define actions an object can perform, often operating on the object's attributes. Methods can return values or modify the object's state.

**Key Points**:
- Attributes represent the "what" (data) of an object; methods represent the "how" (behavior).
- Use `this` to refer to the current object's attributes or methods, avoiding ambiguity.
- Methods can be overloaded (same name, different parameters) to provide flexibility.

**Example** (Extending the `Book` class):
```java
public class Book {
    String title;
    String author;
    int copiesAvailable;

    public Book(String title, String author, int copiesAvailable) {
        this.title = title;
        this.author = author;
        this.copiesAvailable = copiesAvailable;
    }

    public void displayInfo() {
        System.out.println("Title: " + title + ", Author: " + author + ", Copies: " + copiesAvailable);
    }

    public void borrowBook() {
        if (copiesAvailable > 0) {
            copiesAvailable--;
            System.out.println("Book borrowed successfully.");
        } else {
            System.out.println("No copies available.");
        }
    }
}

class Library {
    public static void main(String[] args) {
        Book book1 = new Book("1984", "George Orwell", 3);
        book1.displayInfo(); // Outputs: Title: 1984, Author: George Orwell, Copies: 3
        book1.borrowBook(); // Outputs: Book borrowed successfully.
        book1.displayInfo(); // Outputs: Title: 1984, Author: George Orwell, Copies: 2
    }
}
```
Here, `copiesAvailable` is a new attribute, and `borrowBook` is a method that modifies the object's state. The `this` keyword ensures the constructor assigns parameters to instance variables.

## 2.1.3 Access Specifiers
Access specifiers control the visibility and accessibility of class members (attributes, methods, constructors). Java provides four access levels:

- **public**: Accessible from everywhere.
- **protected**: Accessible within the same package and in subclasses (even in different packages).
- **default** (package-private): Accessible only within the same package if no specifier部分 of the same package, no specifier is provided.
- **private**: Accessible only within the same class.

**Key Points**:
- Use `private` for attributes to enforce encapsulation, providing controlled access via public methods (getters/setters).
- `public` methods expose functionality to other classes.
- Default access is useful for package-level utilities, while `protected` supports inheritance.

**Example** (Adding encapsulation to `Book`):
```java
public class Book {
    private String title;
    private String author;
    private int copiesAvailable;

    public Book(String title, String author, int copiesAvailable) {
        this.title = title;
        this.author = author;
        this.copiesAvailable = copiesAvailable;
    }

    // Getter
    public String getTitle() {
        return title;
    }

    // Setter
    public void setTitle(String title) {
        this.title = title;
    }

    public void displayInfo() {
        System.out.println("Title: " + title + ", Author: " + author + ", Copies: " + copiesAvailable);
    }

    public void borrowBook() {
        if (copiesAvailable > 0) {
            copiesAvailable--;
            System.out.println("Book borrowed successfully.");
        } else {
            System.out.println("No copies available.");
        }
    }
}

class Library {
    public static void main(String[] args) {
        Book book1 = new Book("1984", "George Orwell", 3);
        book1.displayInfo(); // Outputs: Title: 1984, Author: George Orwell, Copies: 3
        book1.setTitle("Animal Farm"); // Changing title
        book1.displayInfo(); // Outputs: Title: Animal Farm, Author: George Orwell, Copies: 3
        book1.borrowBook(); // Outputs: Book borrowed successfully.
    }
}
```
Here, attributes are `private`, and public getters (`getTitle`) and setters (`setTitle`) control access, ensuring encapsulation.

## 2.1.4 Static Keyword
The `static` keyword indicates that a member (attribute or method) belongs to the class rather than an instance. Static members are shared across all objects of the class.

- **Static Attributes**: Store data common to all instances (e.g., a counter for total books).
- **Static Methods**: Can be called without creating an object, often for utility functions.

**Key Points**:
- Static members are initialized when the class is loaded.
- Use `ClassName.staticMember` to access static members.
- Static methods cannot access non-static (instance) members directly.

**Example** (Adding a static counter):
```java
public class Book {
    private String title;
    private String author;
    private int copiesAvailable;
    private static int totalBooks = 0; // Static attribute

    public Book(String title, String author, int copiesAvailable) {
        this.title = title;
        this.author = author;
        this.copiesAvailable = copiesAvailable;
        totalBooks++; // Increment on object creation
    }

    public static int getTotalBooks() { // Static method
        return totalBooks;
    }

    public void displayInfo() {
        System.out.println("Title: " + title + ", Author: " + author + ", Copies: " + copiesAvailable);
    }
}

class Library {
    public static void main(String[] args) {
        System.out.println("Total Books: " + Book.getTotalBooks()); // Outputs: 0
        Book book1 = new Book("1984", "George Orwell", 3);
        Book book2 = new Book("Pride and Prejudice", "Jane Austen", 2);
        System.out.println("Total Books: " + Book.getTotalBooks()); // Outputs: 2
        book1.displayInfo();
    }
}
```
The `totalBooks` static attribute tracks the number of `Book` objects created, and the static `getTotalBooks` method provides access to it.

## 2.1.5 Final Keyword
The `final` keyword restricts modification, ensuring immutability or fixed behavior.

- **Final Variables**: Cannot be reassigned once initialized (like constants).
- **Final Methods**: Cannot be overridden by subclasses.
- **Final Classes**: Cannot be extended.

**Key Points**:
- Use `final` for constants or to prevent unintended changes.
- Final variables must be initialized at declaration or in the constructor.
- Improves code reliability and optimization.

**Example** (Adding a final attribute):
```java
public class Book {
    private final String ISBN; // Final attribute
    private String title;
    private String author;
    private static int totalBooks = 0;

    public Book(String isbn, String title, String author) {
        this.ISBN = isbn; // Set once, cannot change
        this.title = title;
        this.author = author;
        totalBooks++;
    }

    public String getISBN() {
        return ISBN;
    }

    public void displayInfo() {
        System.out.println("ISBN: " + ISBN + ", Title: " + title + ", Author: " + author);
    }

    public static int getTotalBooks() {
        return totalBooks;
    }
}

class Library {
    public static void main(String[] args) {
        Book book1 = new Book("978-0451524935", "1984", "George Orwell");
        book1.displayInfo(); // Outputs: ISBN: 978-0451524935, Title: 1984, Author: George Orwell
        // book1.ISBN = "123"; // Error: cannot assign a value to final variable
        System.out.println("Total Books: " + Book.getTotalBooks()); // Outputs: 1
    }
}
```
The `ISBN` is `final`, ensuring it cannot be changed after initialization, suitable for unique identifiers.

## 2.1.6 Nested Classes
A **nested class** is a class defined within another class. Java supports two types:

- **Static Nested Classes**: Declared with `static`, they belong to the outer class and don’t require an instance of the outer class.
- **Inner Classes**: Non-static, they require an instance of the outer class to be instantiated.

**Key Points**:
- Nested classes are useful for logically grouping related classes with limited scope.
- Inner classes can access the outer class’s instance members, while static nested classes cannot.
- Common in event handling or helper classes.

**Example** (Adding an inner class for borrowing records):
```java
public class Book {
    private final String ISBN;
    private String title;
    private String author;
    private static int totalBooks = 0;

    public Book(String isbn, String title, String author) {
        this.ISBN = isbn;
        this.title = title;
        this.author = author;
        totalBooks++;
    }

    // Inner class
    public class BorrowRecord {
        private String borrowerName;

        public BorrowRecord(String borrowerName) {
            this.borrowerName = borrowerName;
        }

        public void displayRecord() {
            System.out.println("Book: " + title + ", Borrowed by: " + borrowerName);
        }
    }

    public BorrowRecord borrowBook(String borrowerName) {
        return new BorrowRecord(borrowerName);
    }

    public void displayInfo() {
        System.out.println("ISBN: " + ISBN + ", Title: " + title + ", Author: " + author);
    }

    public static int getTotalBooks() {
        return totalBooks;
    }
}

class Library {
    public static void main(String[] args) {
        Book book1 = new Book("978-0451524935", "1984", "George Orwell");
        book1.displayInfo();
        Book.BorrowRecord record = book1.borrowBook("Alice");
        record.displayRecord(); // Outputs: Book: 1984, Borrowed by: Alice
        System.out.println("Total Books: " + Book.getTotalBooks()); // Outputs: 1
    }
}
```
The `BorrowRecord` inner class tracks borrowing details and can access the outer class’s `title` attribute. It requires a `Book` instance to be created.

## 2.1.7 Packages
A **package** is a namespace that organizes classes and interfaces, preventing naming conflicts and improving code maintainability.

- **Declaration**: Use the `package` keyword at the top of a file (e.g., `package com.library`).
- **Importing**: Use `import` to access classes from other packages (e.g., `import java.util.*`).
- **Directory Structure**: Packages correspond to a directory hierarchy (e.g., `com/library/Book.java`).

**Key Points**:
- Packages group related classes (e.g., `com.library.model` for model classes).
- Use fully qualified names (e.g., `com.library.Book`) or imports to resolve conflicts.
- Default package (no `package` declaration) is discouraged for large projects.

**Example** (Organizing `Book` in a package):
```java
package com.library.model;

public class Book {
    private final String ISBN;
    private String title;
    private String author;
    private static int totalBooks = 0;

    public Book(String isbn, String title, String author) {
        this.ISBN = isbn;
        this.title = title;
        this.author = author;
        totalBooks++;
    }

    public class BorrowRecord {
        private String borrowerName;

        public BorrowRecord(String borrowerName) {
            this.borrowerName = borrowerName;
        }

        public void displayRecord() {
            System.out.println("Book: " + title + ", Borrowed by: " + borrowerName);
        }
    }

    public BorrowRecord borrowBook(String borrowerName) {
        return new BorrowRecord(borrowerName);
    }

    public void displayInfo() {
        System.out.println("ISBN: " + ISBN + ", Title: " + title + ", Author: " + author);
    }

    public static int getTotalBooks() {
        return totalBooks;
    }
}
```
```java
package com.library.app;

import com.library.model.Book;

public class Library {
    public static void main(String[] args) {
        Book book1 = new Book("978-0451524935", "1984", "George Orwell");
        book1.displayInfo();
        Book.BorrowRecord record = book1.borrowBook("Alice");
        record.displayRecord();
        System.out.println("Total Books: " + Book.getTotalBooks());
    }
}
```
The `Book` class is in the `com.library.model` package, and the `Library` class in `com.library.app` imports it. This structure organizes the codebase, with `Book.java` stored in `com/library/model/` and `Library.java` in `com/library/app/`.

## Summary
This guide has explored OOP in Java through the lens of a `Library` system, demonstrating how to:
- Define classes and create objects (`Book`).
- Manage attributes and methods with encapsulation (`private` fields, getters/setters).
- Control access with specifiers (`public`, `private`).
- Use `static` for shared data (`totalBooks`).
- Ensure immutability with `final` (`ISBN`).
- Organize related classes with nested classes (`BorrowRecord`).
- Structure code with packages (`com.library.model`).

By consistently applying these concepts to the `Book` class, the guide illustrates how OOP principles work together to create modular, maintainable, and scalable code.
