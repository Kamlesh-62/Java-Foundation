# 2. More about OOP
## 2.1 Java Object Lifecycle Guide

This guide provides a comprehensive overview of the object lifecycle in Java, detailing the stages an object undergoes from creation to destruction. The explanation is tailored to align with the `Library` system used in prior responses, ensuring continuity, and includes examples to illustrate key concepts.

## Object Lifecycle in Java

The **object lifecycle** refers to the sequence of stages an object goes through from its creation to its eventual removal from memory. In Java, objects are instances of classes, and their lifecycle is managed by the Java Virtual Machine (JVM). Understanding this lifecycle is crucial for effective memory management, resource allocation, and writing robust programs. The lifecycle consists of three primary stages: **Creation**, **Usage**, and **Destruction**.

### 1. Creation
The creation stage involves allocating memory for the object and initializing its state. This stage occurs when an object is instantiated using the `new` keyword.

#### Steps in Creation
- **Memory Allocation**: The JVM allocates memory on the heap for the object, including space for its instance variables (attributes) and metadata (e.g., references to the class).
- **Default Initialization**: Instance variables are initialized to default values (e.g., `null` for reference types, `0` for `int`, `false` for `boolean`) before the constructor runs.
- **Constructor Execution**: The constructor is called to initialize the object’s attributes to specific values, setting up its initial state.
- **Object Reference**: A(reference) is returned to the program, allowing access to the object.

#### Key Points
- The `new` keyword triggers memory allocation and constructor invocation.
- Constructors can be overloaded to provide different ways to initialize the object.
- If a constructor calls another constructor (using `this()`), it happens before other initialization code.
- Exceptions during creation (e.g., `OutOfMemoryError`) may prevent the object from being fully instantiated.

### 2. Usage
The usage stage encompasses the period when the object is active and accessible in the program. During this stage, the object’s methods are called, and its attributes are read or modified.

#### Activities in Usage
- **Method Invocation**: The program calls the object’s methods to perform actions or retrieve data (e.g., displaying book details or borrowing a book).
- **Attribute Access**: Attributes are accessed or updated, often through getters and setters, to reflect changes in the object’s state.
- **Reference Management**: The object is referenced by variables, passed to methods, or stored in data structures (e.g., arrays or collections).

#### Key Points
- The object remains in memory as long as it is **reachable**, meaning there is at least one reference to it (e.g., a variable or collection holding its reference).
- Multiple references to the same object can exist, but they all point to the same memory location.
- The object’s state can change during usage (e.g., updating the number of available copies of a book).
- Poor reference management (e.g., keeping unnecessary references) can lead to memory leaks.

### 3. Destruction
The destruction stage occurs when the object is no longer needed and is removed from memory. In Java, this process is handled automatically by the **garbage collector**, a component of the JVM.

#### Steps in Destruction
- **Unreachable State**: The object becomes **unreachable** when there are no references to it (e.g., all variables pointing to it are set to `null` or go out of scope).
- **Garbage Collection Eligibility**: The garbage collector identifies unreachable objects during its sweep of the heap.
- **Finalization (Optional)**: If the object overrides the `finalize()` method (deprecated since Java 9), the JVM may call it before reclamation. However, this is rarely used due to unpredictable timing and performance issues.
- **Memory Reclamation**: The garbage collector deallocates the object’s memory, making it available for future allocations.

#### Key Points
- Java’s garbage collection is **non-deterministic**; the exact time of destruction is not guaranteed and depends on the JVM’s garbage collection algorithm (e.g., mark-and-sweep).
- An object can become eligible for garbage collection when:
  - Its reference variable is set to `null` (e.g., `book = null;`).
  - The reference variable goes out of scope (e.g., a local variable in a method).
  - The object is removed from a collection (e.g., `ArrayList.remove(book)`).
- Developers cannot explicitly destroy objects in Java (unlike languages like C++). Instead, they manage references to influence garbage collection.
- The `System.gc()` method can suggest garbage collection, but it is not guaranteed to run immediately.

### Example Using the `Library` System
The following example, based on the `Book` class, demonstrates the object lifecycle. It illustrates creation, usage, and destruction, with comments highlighting each stage.

```java
public class Book {
    private String title;
    private String author;
    private int copiesAvailable;

    // Constructor
    public Book(String title, String author, int copiesAvailable) {
        this.title = title;
        this.author = author;
        this.copiesAvailable = copiesAvailable;
        System.out.println("Book created: " + title);
    }

    // Method to display book information
    public void displayInfo() {
        System.out.println("Title: " + title + ", Author: " + author + ", Copies: " + copiesAvailable);
    }

    // Method to borrow a book
    public void borrowBook() {
        if (copiesAvailable > 0) {
            copiesAvailable--;
            System.out.println("Book borrowed successfully.");
        } else {
            System.out.println("No copies available.");
        }
    }

    // Deprecated finalize method (for illustration only, not recommended)
    @Override
    protected void finalize() throws Throwable {
        System.out.println("Book destroyed: " + title);
        super.finalize();
    }
}

class Library {
    public static void main(String[] args) {
        // Creation Stage
        Book book1 = new Book("1984", "George Orwell", 3); // Memory allocated, constructor called
        // Outputs: Book created: 1984

        // Usage Stage
        book1.displayInfo(); // Outputs: Title: 1984, Author: George Orwell, Copies: 3
        book1.borrowBook(); // Outputs: Book borrowed successfully.
        book1.displayInfo(); // Outputs: Title: 1984, Author: George Orwell, Copies: 2

        // Destruction Stage (making object eligible for garbage collection)
        book1 = null; // Remove reference, making book1 eligible for garbage collection
        System.gc(); // Suggest garbage collection (may not run immediately)
        // If finalize() is called, outputs: Book destroyed: 1984 (not guaranteed)

        // Creating another book to show lifecycle continuation
        Book book2 = new Book("Pride and Prejudice", "Jane Austen", 2);
        book2.displayInfo(); // Outputs: Title: Pride and Prejudice, Author: Jane Austen, Copies: 2
    }
}
```

### Explanation of the Example
- **Creation**:
  - The `new Book("1984", "George Orwell", 3)` statement allocates memory for `book1`, initializes its attributes to default values (`null` for `title` and `author`, `0` for `copiesAvailable`), and then calls the constructor to set specific values.
  - The constructor prints a message to confirm creation.
- **Usage**:
  - The `displayInfo` and `borrowBook` methods are called to interact with `book1`, demonstrating how the object’s state is accessed and modified.
  - The object is fully functional and reachable via the `book1` reference.
- **Destruction**:
  - Setting `book1 = null` removes the reference, making the object unreachable and eligible for garbage collection.
  - The `System.gc()` call suggests garbage collection, but the JVM may not run it immediately.
  - The `finalize` method (included for illustration) may print a destruction message if the garbage collector runs, but its use is discouraged in modern Java due to unpredictability and performance concerns.

### Additional Notes
- **Garbage Collection Optimization**: The JVM uses sophisticated algorithms (e.g., generational garbage collection) to optimize memory management. Young objects (in the Eden space) are collected more frequently than older ones (in the Tenured space).
- **Memory Leaks**: Holding unnecessary references (e.g., keeping objects in a static collection) can prevent garbage collection, leading to memory leaks. Developers should nullify references or remove objects from collections when no longer needed.
- **Finalization Alternatives**: Instead of `finalize()`, modern Java recommends using `try-with-resources` or `Cleaner` (introduced in Java 9) for resource cleanup, as they are more predictable.
- **Static vs. Instance Variables**: The lifecycle described applies to instance objects. Static variables, which belong to the class, are created when the class is loaded and destroyed when the class is unloaded (rarely, as classes typically remain loaded during the program’s execution).

### Summary
The object lifecycle in Java consists of:
1. **Creation**: Memory allocation, default initialization, and constructor execution to set up the object.
2. **Usage**: Active use through method calls and attribute access while the object is reachable.
3. **Destruction**: Becoming unreachable, eligibility for garbage collection, and memory reclamation by the JVM.

The `Book` example illustrates these stages, showing how a `Book` object is created, used, and made eligible for garbage collection. Understanding the object lifecycle helps developers manage resources effectively, avoid memory leaks, and write efficient Java programs.

If you have further questions or need additional examples, please let me know.

## 2.2. Inheritance
Inheritance is an OOP principle that allows a class (subclass or derived class) to inherit attributes and methods from another class (superclass or base class), promoting code reuse and establishing a hierarchical relationship.

### Explanation
- **Mechanism**: A subclass is defined using the `extends` keyword to inherit from a superclass.
- **Access**: The subclass inherits all non-private members (attributes, methods) of the superclass, unless overridden.
- **Types**:
  - **Single Inheritance**: A class inherits from one superclass (Java’s default).
  - **Multilevel Inheritance**: A class inherits from a subclass, forming a chain (e.g., A → B → C).
  - Java does **not** support multiple inheritance (inheriting from multiple superclasses) for classes to avoid complexity, but interfaces (discussed later) address this.
- **Super Keyword**: Used to access superclass constructors (`super()`) or methods (`super.methodName()`).
- **Overriding**: A subclass can provide a specific implementation of a superclass method using the same [ signature ](../Know-Java-more/Java-core.markdown#signature).

### Key Points
- Inheritance models “is-a” relationships (e.g., a `TextBook` is a `Book`).
- Use `@Override` annotation to ensure correct method overriding.
- A subclass can add new attributes/methods or modify inherited behavior.
- The `final` keyword on a class prevents inheritance; on a method, it prevents overriding.
- Constructors are not inherited but can be called using `super()`.

### Example
In the `Library` system, a `TextBook` class inherits from a `Book` class to represent specialized books with additional attributes like subject.

```java
public class Book {
    protected String title;
    protected String author;
    protected int copiesAvailable;

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

public class TextBook extends Book {
    private String subject;

    public TextBook(String title, String author, int copiesAvailable, String subject) {
        super(title, author, copiesAvailable); // Call superclass constructor
        this.subject = subject;
    }

    @Override
    public void displayInfo() {
        // Extend superclass method
        super.displayInfo();
        System.out.println("Subject: " + subject);
    }
}

class Library {
    public static void main(String[] args) {
        Book book = new Book("1984", "George Orwell", 3);
        TextBook textBook = new TextBook("Calculus", "James Stewart", 2, "Mathematics");

        book.displayInfo();
        // Outputs: Title: 1984, Author: George Orwell, Copies: 3

        textBook.displayInfo();
        // Outputs: Title: Calculus, Author: James Stewart, Copies: 2
        //         Subject: Mathematics

        textBook.borrowBook(); // Inherited method
        // Outputs: Book borrowed successfully.
    }
}
```

### Explanation
- `TextBook` inherits from `Book` using `extends`, gaining `title`, `author`, `copiesAvailable`, and methods like `borrowBook`.
- The `TextBook` constructor calls the superclass constructor with `super()`.
- `displayInfo` is overridden to include the `subject`, demonstrating polymorphism.
- The `protected` access specifier allows the subclass to access superclass attributes.

## 2.3. Abstraction
Abstraction is an OOP principle that hides complex implementation details and exposes only the essential features of an object, reducing complexity and enhancing usability.

### Explanation
- **Mechanism**: Achieved using **abstract classes** or **interfaces**.
  - **Abstract Class**: Defined with the `abstract` keyword, it cannot be instantiated and may contain abstract methods (without implementation) and concrete methods (with implementation).
  - **Interface**: A fully abstract contract (discussed later) with method signatures that implementing classes must define.
- **Abstract Methods**: Declared without a body, forcing subclasses to provide implementations.
- **Purpose**: Abstraction defines “what” an object does, not “how” it does it, allowing flexibility in implementation.

### Key Points
- Abstract classes are useful when some functionality is shared across subclasses.
- Use abstraction to enforce a common interface or behavior across related classes.
- Subclasses of an abstract class must implement all abstract methods unless they are also abstract.
- Abstraction supports polymorphism, as objects can be treated as their abstract type.

### Example
An abstract `LibraryItem` class defines common behavior for items like books and magazines, with `Book` as a concrete subclass.

```java
public abstract class LibraryItem {
    protected String title;
    protected int copiesAvailable;

    public LibraryItem(String title, int copiesAvailable) {
        this.title = title;
        this.copiesAvailable = copiesAvailable;
    }

    // Abstract method
    public abstract void displayInfo();

    // Concrete method
    public void borrowItem() {
        if (copiesAvailable > 0) {
            copiesAvailable--;
            System.out.println("Item borrowed successfully.");
        } else {
            System.out.println("No copies available.");
        }
    }
}

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
}

class Library {
    public static void main(String[] args) {
        // LibraryItem item = new LibraryItem("Test", 1); // Error: cannot instantiate abstract class
        Book book = new Book("1984", "George Orwell", 3);
        book.displayInfo();
        // Outputs: Book - Title: 1984, Author: George Orwell, Copies: 3
        book.borrowItem();
        // Outputs: Item borrowed successfully.
    }
}
```

### Explanation
- `LibraryItem` is abstract, defining a template for library items with a concrete `borrowItem` method and an abstract `displayInfo` method.
- `Book` extends `LibraryItem`, implementing `displayInfo` to provide specific behavior.
- The abstract class cannot be instantiated directly, ensuring only concrete subclasses like `Book` are used.

## 2.4. Method Chaining
Method chaining is a design pattern where methods return the object itself (`this`), allowing multiple method calls to be chained in a single statement for concise and readable code.

### Explanation
- **Mechanism**: A method returns the object’s reference (typically `this`) so that another method can be called immediately on the returned object.
- **Use Cases**: Common in builder patterns, fluent interfaces, or classes that perform multiple operations (e.g., setting properties or performing actions).
- **Implementation**: Methods must be designed to return the class type (e.g., `Book` for a `Book` class).

### Key Points
- Enhances code readability by reducing repetitive object references.
- Often used in setters or configuration methods.
- Requires careful design to ensure methods return the correct type and maintain object state.
- Avoid overuse, as long chains can reduce debugging clarity.

### Example
The `Book` class is modified to support method chaining for setting attributes and borrowing.

```java
public class Book {
    private String title;
    private String author;
    private int copiesAvailable;

    public Book() {
        this.copiesAvailable = 1; // Default value
    }

    public Book setTitle(String title) {
        this.title = title;
        return this; // Return this for chaining
    }

    public Book setAuthor(String author) {
        this.author = author;
        return this;
    }

    public Book setCopiesAvailable(int copiesAvailable) {
        this.copiesAvailable = copiesAvailable;
        return this;
    }

    public Book borrowBook() {
        if (copiesAvailable > 0) {
            copiesAvailable--;
            System.out.println("Book borrowed successfully.");
        } else {
            System.out.println("No copies available.");
        }
        return this;
    }

    public void displayInfo() {
        System.out.println("Title: " + title + ", Author: " + author + ", Copies: " + copiesAvailable);
    }
}

class Library {
    public static void main(String[] args) {
        Book book = new Book()
            .setTitle("1984")
            .setAuthor("George Orwell")
            .setCopiesAvailable(3)
            .borrowBook() // Outputs: Book borrowed successfully.
            .borrowBook(); // Outputs: Book borrowed successfully.
        book.displayInfo();
        // Outputs: Title: 1984, Author: George Orwell, Copies: 1
    }
}
```

### Explanation
- Each setter (`setTitle`, `setAuthor`, `setCopiesAvailable`) and `borrowBook` returns `this`, enabling method chaining.
- The chained call creates and configures a `Book` object in one statement, improving readability.
- A no-arg constructor initializes default values, supporting the builder-like pattern.

## 2.5. Encapsulation
Encapsulation is an OOP principle that bundles data (attributes) and methods within a class, restricting direct access to the data and exposing it through controlled interfaces (e.g., getters and setters).

### Explanation
- **Mechanism**: Attributes are declared `private`, and public methods (getters/setters) provide access or modification.
- **Purpose**: Protects the object’s state, ensures data integrity, and hides implementation details.
- **Benefits**:
  - Prevents invalid data (e.g., negative copies) through validation in setters.
  - Allows internal changes without affecting external code (e.g., changing attribute storage).

### Key Points
- Use `private` for attributes and `public` or `protected` for methods as needed.
- Getters retrieve values; setters validate and update values.
- Encapsulation supports maintenance and scalability by isolating changes.
- Immutable objects can omit setters, using only constructors for initialization.

### Example
The `Book` class uses encapsulation to protect its attributes and validate data.

```java
public class Book {
    private String title;
    private String author;
    private int copiesAvailable;

    public Book(String title, String author, int copiesAvailable) {
        this.title = title;
        this.author = author;
        setCopiesAvailable(copiesAvailable); // Validate in constructor
    }

    // Getter
    public String getTitle() {
        return title;
    }

    // Setter with validation
    public void setTitle(String title) {
        if (title != null && !title.trim().isEmpty()) {
            this.title = title;
        } else {
            throw new IllegalArgumentException("Title cannot be null or empty.");
        }
    }

    public int getCopiesAvailable() {
        return copiesAvailable;
    }

    public void setCopiesAvailable(int copiesAvailable) {
        if (copiesAvailable >= 0) {
            this.copiesAvailable = copiesAvailable;
        } else {
            throw new IllegalArgumentException("Copies cannot be negative.");
        }
    }

    public void displayInfo() {
        System.out.println("Title: " + title + ", Author: " + author + ", Copies: " + copiesAvailable);
    }
}

class Library {
    public static void main(String[] args) {
        Book book = new Book("1984", "George Orwell", 3);
        book.displayInfo();
        // Outputs: Title: 1984, Author: George Orwell, Copies: 3

        book.setCopiesAvailable(2);
        book.displayInfo();
        // Outputs: Title: 1984, Author: George Orwell, Copies: 2

        // book.setCopiesAvailable(-1); // Throws IllegalArgumentException
    }
}
```

### Explanation
- Attributes are `private`, preventing direct access (e.g., `book.title` is not allowed).
- The `setCopiesAvailable` method validates input, ensuring `copiesAvailable` is non-negative.
- Getters provide read-only access where setters are not needed (e.g., `getTitle`).
- Encapsulation ensures the object’s state remains valid and consistent.

## 2.6. Interfaces
An interface is a fully abstract type that defines a contract of methods that implementing classes must provide. It supports abstraction and multiple inheritance-like behavior.

### Explanation
- **Declaration**: Defined with the `interface` keyword; methods are implicitly `public` and `abstract` (unless `default` or `static`).
- **Implementation**: Classes implement interfaces using the `implements` keyword and must provide bodies for all abstract methods.
- **Features** (since Java 8):
  - **Default Methods**: Provide default implementations, allowing interface evolution without breaking existing code.
  - **Static Methods**: Utility methods belonging to the interface.
- **Multiple Interfaces**: A class can implement multiple interfaces, unlike single-class inheritance.

### Key Points
- Interfaces define “what” must be done, not “how” (pure abstraction).
- Useful for defining common behavior across unrelated classes (e.g., borrowable items).
- Default methods reduce code duplication in implementing classes.
- Interfaces cannot have instance variables but can have `public static final` constants.

### Example
A `Borrowable` interface defines borrowing behavior, implemented by `Book`.

```java
public interface Borrowable {
    void borrowItem(); // Abstract method

    default void returnItem() { // Default method
        System.out.println("Item returned successfully.");
    }

    static boolean isBorrowable(int copiesAvailable) { // Static method
        return copiesAvailable > 0;
    }
}

public class Book implements Borrowable {
    private String title;
    private String author;
    private int copiesAvailable;

    public Book(String title, String author, int copiesAvailable) {
        this.title = title;
        this.author = author;
        this.copiesAvailable = copiesAvailable;
    }

    @Override
    public void borrowItem() {
        if (Borrowable.isBorrowable(copiesAvailable)) {
            copiesAvailable--;
            System.out.println("Book borrowed successfully.");
        } else {
            System.out.println("No copies available.");
        }
    }

    public void displayInfo() {
        System.out.println("Title: " + title + ", Author: " + author + ", Copies: " + copiesAvailable);
    }
}

class Library {
    public static void main(String[] args) {
        Book book = new Book("1984", "George Orwell", 3);
        book.displayInfo();
        // Outputs: Title: 1984, Author: George Orwell, Copies: 3

        book.borrowItem();
        // Outputs: Book borrowed successfully.

        book.returnItem();
        // Outputs: Item returned successfully.

        System.out.println("Is borrowable? " + Borrowable.isBorrowable(book.getCopiesAvailable()));
        // Outputs: Is borrowable? true
    }
}
```

### Explanation
- The `Borrowable` interface defines `borrowItem` and provides a `default` `returnItem` method and a `static` `isBorrowable` method.
- `Book` implements `Borrowable`, providing an implementation for `borrowItem`.
- The default method `returnItem` is inherited without modification.
- The static method is called using the interface name (`Borrowable.isBorrowable`).

  ## 2.7. Enums
An **enum** (enumeration) is a special Java type used to define a fixed set of constants, representing a collection of related values.

### Explanation
- **Declaration**: Defined using the `enum` keyword, typically containing a list of constants.
- **Features**:
  - Enums are implicitly `final` and extend `java.lang.Enum`.
  - Each constant is an instance of the enum type.
  - Can include fields, constructors, and methods for additional functionality.
- **Use Cases**: Representing categories, statuses, or fixed options (e.g., book genres or loan statuses).

### Key Points
- Enums provide type safety, preventing invalid values.
- Constants are uppercase by convention (e.g., `FICTION`, `NON_FICTION`).
- Enums can be used in `switch` statements and comparisons.
- The `values()` method returns all constants, and `valueOf(String)` retrieves a constant by name.

### Example
An enum `Genre` categorizes books in the `Library` system.

```java
public enum Genre {
    FICTION("Fiction"), NON_FICTION("Non-Fiction"), SCIENCE("Science"), HISTORY("History");

    private final String displayName;

    Genre(String displayName) {
        this.displayName = displayName;
    }

    public String getDisplayName() {
        return displayName;
    }
}

public class Book {
    private String title;
    private Genre genre;

    public Book(String title, Genre genre) {
        this.title = title;
        this.genre = genre;
    }

    // Getter for the genre field
    public Genre getGenre() {
        return this.genre;
    }

    public void displayInfo() {
        System.out.println("Title: " + title + ", Genre: " + genre.getDisplayName());
    }
}

class Library {
    public static void main(String[] args) {
        Book book = new Book("1984", Genre.FICTION);
        book.displayInfo();
        // Outputs: Title: 1984, Genre: Fiction

        // Using enum in switch
        switch (book.getGenre()) {
            case FICTION:
                System.out.println("This is a fictional work.");
                break;
            default:
                System.out.println("Other genre.");
        }
        // Outputs: This is a fictional work.
    }
}
```

### Explanation
- `Genre` enum defines constants with associated `displayName` values.
- The constructor and `getDisplayName` method add functionality.
- `Book` uses `Genre` to categorize books, ensuring only valid genres are assigned.

## 2.8. Records
A **record** is a concise, immutable data class introduced in Java 14 (finalized in Java 16) to simplify the creation of classes that primarily hold data.

### Explanation
- **Declaration**: Defined with the `record` keyword, specifying components (fields) in the header.
- **Features**:
  - Automatically provides a constructor, getters, `equals()`, `hashCode()`, and `toString()`.
  - Components are implicitly `final`, ensuring immutability.
  - Cannot extend other classes but can implement interfaces.
- **Use Cases**: Representing data transfer objects, simple entities, or immutable records (e.g., book metadata).

### Key Points
- Reduces boilerplate code compared to traditional classes.
- Best for immutable data; setters are not provided.
- Custom constructors or methods can be added for additional logic.
- Components are accessed via auto-generated getters (e.g., `title()` instead of `getTitle()`).

### Example
A `BookRecord` record stores immutable book data.

```java
public record BookRecord(String title, String author, int copiesAvailable) {
    // Custom validation in constructor
    public BookRecord {
        if (title == null || title.trim().isEmpty()) {
            throw new IllegalArgumentException("Title cannot be null or empty.");
        }
        if (copiesAvailable < 0) {
            throw new IllegalArgumentException("Copies cannot be negative.");
        }
    }

    // Custom method
    public void displayInfo() {
        System.out.println("Title: " + title + ", Author: " + author + ", Copies: " + copiesAvailable);
    }
}

class Library {
    public static void main(String[] args) {
        BookRecord book = new BookRecord("1984", "George Orwell", 3);
        book.displayInfo();
        // Outputs: Title: 1984, Author: George Orwell, Copies: 3

        System.out.println(book.toString());
        // Outputs: BookRecord[title=1984, author=George Orwell, copiesAvailable=3]

        // BookRecord invalid = new BookRecord("", "Author", -1); // Throws IllegalArgumentException
    }
}
```

### Explanation
- `BookRecord` defines `title`, `author`, and `copiesAvailable` as components.
- The compact constructor validates inputs.
- Auto-generated methods (`toString`, getters) simplify usage, and a custom `displayInfo` method is added.

### 🔍 Why Do We Need Records? (Simplified)
In Java, when you just need a class to hold data—like a container—you often end up writing a lot of repetitive code:
- `Constructor`
- `Getters`
- `equals()` and `hashCode()` for comparison
- `toString()` for logging/debugging

Records eliminate this boilerplate automatically.

📦 Think of a record as:
- “A simple class that just holds data, is immutable, and writes all the boring code for you.”

✅ You should use records when:
- You don't need to modify the data after creation.
- You just need to store and pass data around (e.g. from a database, API, or form).
- You want your code to be cleaner, shorter, and less error-prone.

🧠 Example in real life:
Instead of writing a 40-line class just to store a book’s title, author, and number of copies, a record lets you do it in 1 line—with all necessary methods already built in.

🎯 Bottom Line:
- Use records when you're building simple, immutable data holders and want to write less code and make fewer mistakes.

## 2.9. Method Overloading / Overriding
**Method Overloading** and **Method Overriding** are mechanisms to enhance flexibility and [Polymorphism](#213-polymorphism) in Java.

### Explanation
- **Method Overloading**:
  - Multiple methods in the same class with the same name but different parameter lists (number, type, or order).
  - Resolved at **compile time** based on the method [ signature ](../Know-Java-more/Java-core.markdown#signature).
  - Return type alone cannot differentiate overloaded methods.
- **Method Overriding**:
  - A subclass provides a specific implementation of a method defined in its superclass, using the same signature (name, parameters, return type).
  - Resolved at **runtime** based on the object’s actual type (dynamic binding).
  - Requires the `@Override` annotation for clarity and error checking.

### Key Points
- Overloading supports flexibility by allowing different ways to call a method.
- Overriding enables polymorphic behavior, customizing superclass methods.
- Overridden methods must have the same or covariant return type and compatible exceptions.
- `final` methods cannot be overridden; `private` methods are not inherited, so they cannot be overridden.

### Example
The `Book` class demonstrates overloading, and a `TextBook` subclass demonstrates overriding.

```java
public class Book {
    private String title;
    private int copiesAvailable;

    public Book(String title, int copiesAvailable) {
        this.title = title;
        this.copiesAvailable = copiesAvailable;
    }

    // Overloaded methods
    public void displayInfo() {
        System.out.println("Title: " + title + ", Copies: " + copiesAvailable);
    }

    public void displayInfo(boolean showDetails) {
        System.out.println("Title: " + title + (showDetails ? ", Copies: " + copiesAvailable : ""));
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

public class TextBook extends Book {
    private String subject;

    public TextBook(String title, int copiesAvailable, String subject) {
        super(title, copiesAvailable);
        this.subject = subject;
    }

    // Overriding
    @Override
    public void displayInfo() {
        super.displayInfo();
        System.out.println("Subject: " + subject);
    }
}

class Library {
    public static void main(String[] args) {
        Book book = new Book("1984", 3);
        book.displayInfo(); // Calls no-arg method
        // Outputs: Title: 1984, Copies: 3
        book.displayInfo(true); // Calls overloaded method
        // Outputs: Title: 1984, Copies: 3

        TextBook textBook = new TextBook("Calculus", 2, "Mathematics");
        textBook.displayInfo(); // Calls overridden method
        // Outputs: Title: Calculus, Copies: 2
        //         Subject: Mathematics
    }
}
```

### Explanation
- `Book` has overloaded `displayInfo` methods (no-arg and with a `boolean` parameter).
- `TextBook` overrides `displayInfo` to include `subject`, calling the superclass method with `super`.
- Overloading is resolved at compile time; overriding is resolved at runtime based on the object’s type.

## 2.10. Initializer Block
An **initializer block** is a block of code in a class that runs during object creation, used to initialize instance variables or perform setup tasks.

### Explanation
- **Types**:
  - **Instance Initializer Block**: Runs for each object creation, after default initialization but before the constructor.
  - **Static Initializer Block**: Runs once when the class is loaded, used for static variable initialization.
- **Syntax**: Enclosed in `{}` for instance blocks or `static {}` for static blocks.
- **Use Cases**: Complex initialization logic that cannot be handled in variable declarations or constructors.

### Key Points
- Instance initializer blocks are useful when initialization logic is shared across multiple constructors.
- Static initializer blocks initialize static fields or perform one-time setup.
- Multiple initializer blocks are executed in the order they appear in the code.
- Constructors execute after instance initializer blocks.

### Example
A `Book` class uses both instance and static initializer blocks.

```java
public class Book {
    private String title;
    private int copiesAvailable;
    private static int totalBooks;

    // Static initializer block
    static {
        totalBooks = 0;
        System.out.println("Static initializer: Setting totalBooks to 0");
    }

    // Instance initializer block
    {
        copiesAvailable = 1; // Default value
        System.out.println("Instance initializer: Setting default copies");
    }

    public Book(String title) {
        this.title = title;
        totalBooks++;
    }

    public void displayInfo() {
        System.out.println("Title: " + title + ", Copies: " + copiesAvailable + ", Total Books: " + totalBooks);
    }
}

class Library {
    public static void main(String[] args) {
        // Static initializer runs once when class is loaded
        // Outputs: Static initializer: Setting totalBooks to 0

        Book book1 = new Book("1984");
        // Outputs: Instance initializer: Setting default copies
        book1.displayInfo();
        // Outputs: Title: 1984, Copies: 1, Total Books: 1

        Book book2 = new Book("Pride and Prejudice");
        // Outputs: Instance initializer: Setting default copies
        book2.displayInfo();
        // Outputs: Title: Pride and Prejudice, Copies: 1, Total Books: 2
    }
}
```

### Explanation
- The static initializer sets `totalBooks` when the class is loaded.
- The instance initializer sets a default `copiesAvailable` for each `Book` object.
- The constructor increments `totalBooks` and sets `title`, running after the instance initializer.

## 2.11. Static vs. Dynamic Binding
**Binding** refers to the process of associating a method call with the method’s implementation.

### Explanation
- **Static Binding** (Early Binding):
  - Resolved at **compile time**.
  - Applies to `static`, `final`, `private`, or overloaded methods.
  - The compiler determines the method based on the reference type.
- **Dynamic Binding** (Late Binding):
  - Resolved at **runtime**.
  - Applies to overridden instance methods.
  - The JVM determines the method based on the object’s actual type.

### Key Points
- Static binding is faster but less flexible; dynamic binding supports polymorphism.
- Use `final` or `private` to enforce static binding when polymorphism is not needed.
- The `@Override` annotation ensures dynamic binding for overridden methods.
- Variables and static methods are always statically bound; instance methods are dynamically bound unless `final` or `private`.

### Example
A `Book` and `TextBook` demonstrate binding types.

```java
public class Book {
    private String title;

    public Book(String title) {
        this.title = title;
    }

    public void displayInfo() {
        System.out.println("Book: " + title);
    }

    public static void getType() {
        System.out.println("Generic Book");
    }
}

public class TextBook extends Book {
    private String subject;

    public TextBook(String title, String subject) {
        super(title);
        this.subject = subject;
    }

    @Override
    public void displayInfo() {
        System.out.println("TextBook: " + super.getTitle() + ", Subject: " + subject);
    }

    public static void getType() {
        System.out.println("TextBook");
    }
}

class Library {
    public static void main(String[] args) {
        Book book = new Book("1984");
        Book textBook = new TextBook("Calculus", "Mathematics");

        book.displayInfo(); // Static binding (Book type)
        // Outputs: Book: 1984

        textBook.displayInfo(); // Dynamic binding (TextBook object)
        // Outputs: TextBook: Calculus, Subject: Mathematics

        Book.getType(); // Static binding (static method)
        // Outputs: Generic Book

        TextBook.getType(); // Static binding (static method)
        // Outputs: TextBook
    }
}
```

### Explanation
- `displayInfo` is dynamically bound; the actual object type (`TextBook`) determines the method called.
- `getType` is statically bound; the reference type (`Book` or `TextBook`) determines the method, as it is `static`.
- The `Book` reference to a `TextBook` object shows polymorphism via dynamic binding.

## 2.12. Pass by Value / Pass by Reference
Java uses **pass by value** for all method arguments, but the behavior differs for primitive and reference types, leading to confusion with pass by reference.

### Explanation
- **Pass by Value**:
  - Java copies the value of the argument (either a primitive or a reference) and passes it to the method.
  - For **primitives**, the value is the actual data (e.g., `int` value).
  - For **references**, the value is a copy of the memory address (reference), not the object itself.
- **Pass by Reference** (Not in Java):
  - In pass by reference, the method directly manipulates the original variable’s memory.
  - Java does not support this; changes to the reference (e.g., reassigning it) do not affect the caller’s reference.

### Key Points
- For primitives, changes in the method do not affect the caller’s variable.
- For objects, changes to the object’s state (via the reference) affect the caller’s object, but reassigning the reference does not.
- No way to modify the caller’s reference itself (e.g., swap two objects).
- Understanding this prevents common bugs in method parameter handling.

### Example
A `Book` class demonstrates pass by value behavior.

```java
public class Book {
    private String title;
    private int copiesAvailable;

    public Book(String title, int copiesAvailable) {
        this.title = title;
        this.copiesAvailable = copiesAvailable;
    }

    public void setTitle(String title) {
        this.title = title;
    }

    public void setCopiesAvailable(int copiesAvailable) {
        this.copiesAvailable = copiesAvailable;
    }

    public void displayInfo() {
        System.out.println("Title: " + title + ", Copies: " + copiesAvailable);
    }
}

class Library {
    public static void updateBook(Book book, int newCopies) {
        book.setCopiesAvailable(newCopies); // Modifies object state
        book = new Book("New Book", 5); // Reassigns local reference, no effect on caller
        System.out.println("Inside method: ");
        book.displayInfo();
    }

    public static void updatePrimitive(int value) {
        value = 100; // Modifies local copy, no effect on caller
    }

    public static void main(String[] args) {
        Book book = new Book("1984", 3);
        System.out.println("Before method: ");
        book.displayInfo();
        // Outputs: Title: 1984, Copies: 3

        updateBook(book, 2);
        // Outputs: Inside method:
        //         Title: New Book, Copies: 5
        System.out.println("After method: ");
        book.displayInfo();
        // Outputs: Title: 1984, Copies: 2

        int copies = 10;
        System.out.println("Before primitive: " + copies);
        updatePrimitive(copies);
        System.out.println("After primitive: " + copies);
        // Outputs: Before primitive: 10
        //         After primitive: 10
    }
}
```

### Explanation
- In `updateBook`, the `book` parameter is a copy of the reference to the `Book` object.
  - `setCopiesAvailable` modifies the object’s state, affecting the caller’s object.
  - Reassigning `book` to a new object changes the local reference, not the caller’s.
- In `updatePrimitive`, the `value` parameter is a copy of the `int`; changes do not affect the caller’s `copies`.
- This demonstrates Java’s pass by value for both primitives and references.

## 2.13. Polymorphism

**Polymorphism** allows objects of different classes to be treated as instances of a common superclass or interface, while executing behavior specific to their actual class. It is a core OOP principle that supports dynamic behavior, enabling a single interface or superclass type to represent multiple implementations.

### Explanation
- **Definition**: Polymorphism, meaning "many forms," refers to the ability of a single method call or reference to operate differently depending on the object’s actual type at runtime.
- **Types**:
  - **Compile-Time Polymorphism** (Static Polymorphism): Achieved through **method overloading**, where multiple methods share the same name but differ in parameter lists (number, type, or order). Resolved at compile time based on the method signature.
  - **Runtime Polymorphism** (Dynamic Polymorphism): Achieved through **method overriding**, where a subclass provides a specific implementation of a superclass or interface method. Resolved at runtime using dynamic method dispatch.
- **Mechanisms**:
  - **Inheritance**: A subclass inherits from a superclass, overriding methods to provide specialized behavior.
  - **Interfaces**: Classes implementing an interface provide concrete implementations of its methods, allowing polymorphic treatment.
  - **Upcasting**: Assigning a subclass object to a superclass or interface reference (implicit and safe).
  - **Downcasting**: Converting a superclass or interface reference back to a subclass type (explicit, requires `instanceof` checks to avoid `ClassCastException`).
- **Purpose**: Enables generic code that works with multiple types, reduces redundancy, and supports extensibility through polymorphism.

### Key Points
- Polymorphism models “is-a” relationships (e.g., a `TextBook` is a `Book`).
- Runtime polymorphism relies on **dynamic binding**, where the JVM determines the method to call based on the object’s actual type, not the reference type.
- Compile-time polymorphism is resolved by the compiler, making it faster but less flexible than runtime polymorphism.
- The `@Override` annotation ensures correct method overriding and improves code clarity.
- Polymorphism is central to design patterns like Strategy, Factory, and Decorator, enabling modular and scalable designs.

### Example
The following example, based on the `Library` system, demonstrates both compile-time and runtime polymorphism using an abstract `LibraryItem` class, a `Book` class, a `TextBook` subclass, and a `Borrowable` interface.

```java
// Interface for borrowable items
public interface Borrowable {
    void borrowItem();
    default void returnItem() {
        System.out.println("Item returned successfully.");
    }
}

// Abstract class for library items
public abstract class LibraryItem {
    protected String title;
    protected int copiesAvailable;

    public LibraryItem(String title, int copiesAvailable) {
        this.title = title;
        this.copiesAvailable = copiesAvailable;
    }

    // Abstract method for runtime polymorphism
    public abstract void displayInfo();

    // Overloaded method for compile-time polymorphism
    public void displayInfo(boolean includeCopies) {
        System.out.println("Title: " + title + (includeCopies ? ", Copies: " + copiesAvailable : ""));
    }
}

// Book class implementing Borrowable
public class Book extends LibraryItem implements Borrowable {
    private String author;

    public Book(String title, String author, int copiesAvailable) {
        super(title, copiesAvailable);
        this.author = author;
    }

    @Override
    public void displayInfo() {
        System.out.println("Book - Title: " + title + ", Author: " + author + ", Copies: " + copiesAvailable);
    }

    @Override
    public void borrowItem() {
        if (copiesAvailable > 0) {
            copiesAvailable--;
            System.out.println("Book borrowed successfully.");
        } else {
            System.out.println("No copies available.");
        }
    }
}

// TextBook subclass
public class TextBook extends Book {
    private String subject;

    public TextBook(String title, String author, int copiesAvailable, String subject) {
        super(title, author, copiesAvailable);
        this.subject = subject;
    }

    @Override
    public void displayInfo() {
        System.out.println("TextBook - Title: " + title + ", Author: " + author + ", Subject: " + subject + ", Copies: " + copiesAvailable);
    }

    // Getter for subject
    public String getSubject() {
        return subject;
    }
}

class Library {
    public static void main(String[] args) {
        // Upcasting to superclass and interface
        LibraryItem book = new Book("1984", "George Orwell", 3);
        LibraryItem textBook = new TextBook("Calculus", "James Stewart", 2, "Mathematics");
        Borrowable borrowable = book;

        // Runtime polymorphism: Method overriding
        book.displayInfo();
        // Outputs: Book - Title: 1984, Author: George Orwell, Copies: 3
        textBook.displayInfo();
        // Outputs: TextBook - Title: Calculus, Author: James Stewart, Subject: Mathematics, Copies: 2

        // Compile-time polymorphism: Method overloading
        book.displayInfo(true);
        // Outputs: Title: 1984, Copies: 3
        book.displayInfo(false);
        // Outputs: Title: 1984

        // Interface-based polymorphism
        borrowable.borrowItem();
        // Outputs: Book borrowed successfully.
        borrowable.returnItem();
        // Outputs: Item returned successfully.

        // Downcasting
        if (textBook instanceof TextBook) {
            TextBook specificTextBook = (TextBook) textBook;
            System.out.println("Subject: " + specificTextBook.getSubject());
            // Outputs: Subject: Mathematics
        }
    }
}
```

### Explanation
- **Compile-Time Polymorphism**:
  - The `displayInfo` method in `LibraryItem` is overloaded with a `boolean` parameter (`includeCopies`). The compiler selects the appropriate method based on the argument provided (`true` or `false`), demonstrating static polymorphism.
- **Runtime Polymorphism**:
  - The `displayInfo` method is overridden in `Book` and `TextBook`. When called on a `LibraryItem` reference, the JVM invokes the method corresponding to the actual object type (`Book` or `TextBook`), showcasing dynamic binding.
- **Interface-Based Polymorphism**:
  - The `Borrowable` interface allows `Book` to be treated as a `Borrowable` type, enabling calls to `borrowItem` and the default `returnItem` method polymorphically.
- **Upcasting**:
  - `Book` and `TextBook` objects are assigned to `LibraryItem` or `Borrowable` references, allowing generic handling without knowing the specific type.
- **Downcasting**:
  - The `textBook` reference (type `LibraryItem`) is explicitly cast to `TextBook` to access the `getSubject` method, with `instanceof` ensuring type safety.

### Additional Notes
- **Field Access**: Fields are not polymorphic; they are resolved based on the reference type, not the object type. In the example, accessing `title` via a `LibraryItem` reference uses the superclass field.
- **Covariant Return Types**: Overridden methods can return a subtype of the superclass method’s return type (since Java 5), enhancing polymorphic flexibility.
- **Limitations**:
  - **Static Methods**: Static methods are bound to the reference type (static binding), not the object type, so they do not participate in runtime polymorphism.
  - **Private/Final Methods**: Methods marked `private` or `final` cannot be overridden, limiting runtime polymorphism.
- **Performance**: Runtime polymorphism incurs a minor overhead due to dynamic method dispatch, but modern JVM optimizations (e.g., inline caching) minimize this impact.
- **Design Patterns**: Polymorphism is foundational to patterns like Strategy, Factory, and Decorator, enabling flexible and extensible code designs.
  
| **Feature**       | **Overriding** | **Overloading**                                                       |
|-------------------|----------------------------|---------------------------------------------------------------------|
| Type       | Runtime polymorphism               | 	Compile-time polymorphism                       |
| When    | Inherited methods                | Same class, different method signatures                                 |
| Purpose	        | Customize behavior	                 | Improve flexibility and readability |

