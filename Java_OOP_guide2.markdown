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
- **Overriding**: A subclass can provide a specific implementation of a superclass method using the same signature.

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
