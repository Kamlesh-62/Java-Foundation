# Java Collections Guide

This guide provides an in-depth exploration of the Java **Collections Framework**, focusing on Arrays vs. ArrayLists, Sets, Maps, Queues, Deques, Stacks, Iterators, and Generic Collections. Each section is tailored to align with the `Library` system used in prior responses, ensuring continuity. Examples illustrate key concepts, and key points facilitate understanding.

## 1. Array vs. ArrayList
Arrays and ArrayLists are used to store multiple elements, but they differ significantly in functionality and flexibility.

### Explanation
- **Array**:
  - A fixed-size, low-level data structure that stores elements of the same type.
  - Declared with a specific size (e.g., `String[] books = new String[10];`).
  - Accessed via indices (0-based).
  - Part of Java’s core language, not the Collections Framework.
- **ArrayList**:
  - A dynamic, resizable list implementation in the Collections Framework (`java.util.ArrayList`).
  - Grows or shrinks automatically as elements are added or removed.
  - Provides methods for manipulation (e.g., `add`, `remove`, `get`).
  - Implements the `List` interface, supporting ordered collections.

### Key Points
- **Size**: Arrays have a fixed size; ArrayLists are dynamic.
- **Type Safety**: Arrays are type-safe at compile time; ArrayLists use generics for type safety.
- **Performance**: Arrays are slightly faster for fixed-size data due to direct memory access; ArrayLists have overhead but offer flexibility.
- **Functionality**: ArrayLists provide rich methods (e.g., `contains`, `indexOf`); arrays rely on manual iteration or `Arrays` utility methods.
- **Use Case**: Use arrays for fixed, performance-critical data; use ArrayLists for dynamic, general-purpose lists.

### Example
Managing books in the `Library` system using an array and an ArrayList.

```java
import java.util.ArrayList;
import java.util.Arrays;

public class Book {
    private String title;

    public Book(String title) {
        this.title = title;
    }

    public String getTitle() {
        return title;
    }

    @Override
    public String toString() {
        return "Book: " + title;
    }
}

class Library {
    public static void main(String[] args) {
        // Array
        Book[] bookArray = new Book[3];
        bookArray[0] = new Book("1984");
        bookArray[1] = new Book("Pride and Prejudice");
        // Fixed size, cannot add more than 3
        System.out.println("Array Books:");
        for (Book book : bookArray) {
            System.out.println(book); // Null for unassigned index
        }

        // ArrayList
        ArrayList<Book> bookList = new ArrayList<>();
        bookList.add(new Book("1984"));
        bookList.add(new Book("Pride and Prejudice"));
        bookList.add(new Book("Calculus")); // Dynamic size
        System.out.println("\nArrayList Books:");
        for (Book book : bookList) {
            System.out.println(book);
        }

        // ArrayList methods
        bookList.remove(1); // Remove "Pride and Prejudice"
        System.out.println("\nAfter Removal:");
        System.out.println(bookList);
    }
}
```

### Explanation
- **Array**: `bookArray` is fixed at size 3; unassigned indices (e.g., index 2) are `null`. Adding more books requires a new array.
- **ArrayList**: `bookList` grows dynamically with `add`. Methods like `remove` simplify manipulation.
- Output:
  ```
  Array Books:
  Book: 1984
  Book: Pride and Prejudice
  null

  ArrayList Books:
  Book: 1984
  Book: Pride and Prejudice
  Book: Calculus

  After Removal:
  [Book: 1984, Book: Calculus]
  ```

## 2. Set
A **Set** is a collection that stores unique elements, with no duplicates, and is part of the Collections Framework (`java.util.Set`).

### Explanation
- **Characteristics**: Does not maintain insertion order (except for `LinkedHashSet`) and ensures uniqueness.
- **Implementations**:
  - `HashSet`: Uses hashing for O(1) average-case operations; no order.
  - `LinkedHashSet`: Maintains insertion order.
  - `TreeSet`: Maintains sorted order (natural or custom); O(log n) operations.
- **Use Cases**: Storing unique items (e.g., unique book ISBNs), removing duplicates.

### Key Points
- Use `HashSet` for fast operations when order doesn’t matter.
- Use `TreeSet` for sorted elements; requires elements to implement `Comparable` or a `Comparator`.
- Sets do not support index-based access (not a `List`).
- Methods: `add`, `remove`, `contains`, `size`.

### Example
Tracking unique book ISBNs in the `Library`.

```java
import java.util.HashSet;
import java.util.Set;

class Library {
    public static void main(String[] args) {
        Set<String> isbnSet = new HashSet<>();
        isbnSet.add("978-0451524935"); // 1984
        isbnSet.add("978-0141439518"); // Pride and Prejudice
        isbnSet.add("978-0451524935"); // Duplicate, ignored
        System.out.println("Unique ISBNs: " + isbnSet);
        // Outputs: Unique ISBNs: [978-0141439518, 978-0451524935]

        boolean exists = isbnSet.contains("978-0451524935");
        System.out.println("ISBN exists: " + exists); // Outputs: true
    }
}
```

### Explanation
- `HashSet` ensures no duplicate ISBNs are stored.
- `contains` checks for existence efficiently.

## 3. Map
A **Map** is a collection that stores key-value pairs, where each key is unique, and is part of the Collections Framework (`java.util.Map`).

### Explanation
- **Characteristics**: Maps keys to values; keys are unique, but values can be duplicated.
- **Implementations**:
  - `HashMap`: Uses hashing for O(1) average-case operations; no order.
  - `LinkedHashMap`: Maintains insertion order.
  - `TreeMap`: Maintains sorted order by keys; O(log n) operations.
- **Use Cases**: Associating data (e.g., ISBN to book objects).

### Key Points
- Keys must be unique; adding a duplicate key overwrites the value.
- Use `TreeMap` for sorted keys; requires keys to implement `Comparable` or a `Comparator`.
- Methods: `put`, `get`, `remove`, `containsKey`, `keySet`, `entrySet`.

### Example
Mapping ISBNs to books in the `Library`.

```java
import java.util.HashMap;
import java.util.Map;

public class Book {
    private String title;

    public Book(String title) {
        this.title = title;
    }

    @Override
    public String toString() {
        return "Book: " + title;
    }
}

class Library {
    public static void main(String[] args) {
        Map<String, Book> bookMap = new HashMap<>();
        bookMap.put("978-0451524935", new Book("1984"));
        bookMap.put("978-0141439518", new Book("Pride and Prejudice"));
        bookMap.put("978-0451524935", new Book("New 1984")); // Overwrites

        System.out.println("Book Map: " + bookMap);
        // Outputs: Book Map: {978-0141439518=Book: Pride and Prejudice, 978-0451524935=Book: New 1984}

        Book book = bookMap.get("978-0141439518");
        System.out.println("Found: " + book); // Outputs: Found: Book: Pride and Prejudice
    }
}
```

### Explanation
- `HashMap` maps ISBNs to `Book` objects; duplicate keys overwrite values.
- `get` retrieves a book by ISBN.

## 4. Queue
A **Queue** is a collection designed for holding elements prior to processing, typically following First-In-First-Out (FIFO) order (`java.util.Queue`).

### Explanation
- **Characteristics**: Elements are added at the rear and removed from the front.
- **Implementations**:
  - `LinkedList`: Implements `Queue` with flexible size.
  - `PriorityQueue`: Orders elements by natural order or a `Comparator`.
- **Use Cases**: Task scheduling, waiting lists (e.g., book borrowing queue).

### Key Points
- Methods: `offer` (add), `poll` (remove and return head), `peek` (view head).
- `PriorityQueue` does not guarantee FIFO; it prioritizes based on ordering.
- Throws exceptions for illegal operations (e.g., `add` on full queue) or returns `null`/false (e.g., `offer`).

### Example
Managing a borrowing queue in the `Library`.

```java
import java.util.LinkedList;
import java.util.Queue;

class Library {
    public static void main(String[] args) {
        Queue<String> borrowQueue = new LinkedList<>();
        borrowQueue.offer("Alice"); // Add borrower
        borrowQueue.offer("Bob");
        System.out.println("Borrow Queue: " + borrowQueue);
        // Outputs: Borrow Queue: [Alice, Bob]

        String next = borrowQueue.poll(); // Remove and return head
        System.out.println("Next Borrower: " + next); // Outputs: Next Borrower: Alice
        System.out.println("Updated Queue: " + borrowQueue); // Outputs: Updated Queue: [Bob]
    }
}
```

### Explanation
- `LinkedList` as a `Queue` maintains FIFO order.
- `offer` adds borrowers; `poll` processes the first borrower.

## 5. Deque
A **Deque** (Double-Ended Queue) is a collection that allows elements to be added or removed from both ends (`java.util.Deque`).

### Explanation
- **Characteristics**: Supports operations at both the front and rear, enabling FIFO (queue) or LIFO (stack) behavior.
- **Implementations**:
  - `ArrayDeque`: Resizable, efficient for both ends.
  - `LinkedList`: Flexible but less efficient for some operations.
- **Use Cases**: Undo operations, bidirectional task processing.

### Key Points
- Methods: `addFirst`, `addLast`, `removeFirst`, `removeLast`, `peekFirst`, `peekLast`.
- More versatile than `Queue`, supporting both queue and stack operations.
- `ArrayDeque` is preferred for performance over `LinkedList`.

### Example
Tracking book returns in the `Library`.

```java
import java.util.ArrayDeque;
import java.util.Deque;

class Library {
    public static void main(String[] args) {
        Deque<String> returnQueue = new ArrayDeque<>();
        returnQueue.addFirst("1984"); // Add to front
        returnQueue.addLast("Calculus"); // Add to rear
        System.out.println("Return Queue: " + returnQueue);
        // Outputs: Return Queue: [1984, Calculus]

        String firstReturn = returnQueue.removeFirst();
        System.out.println("First Return: " + firstReturn); // Outputs: First Return: 1984
    }
}
```

### Explanation
- `ArrayDeque` allows adding books at both ends.
- `removeFirst` processes the first book returned.

## 6. Stack
A **Stack** is a LIFO (Last-In-First-Out) collection, part of the Collections Framework (`java.util.Stack`).

### Explanation
- **Characteristics**: Elements are added (pushed) and removed (popped) from the top.
- **Implementation**: `Stack` class extends `Vector`, but `ArrayDeque` is preferred for modern stack implementations due to better performance.
- **Use Cases**: Undo operations, expression evaluation.

### Key Points
- Methods: `push` (add), `pop` (remove and return top), `peek` (view top).
- `Stack` is synchronized, making it slower; use `ArrayDeque` for non-threaded applications.
- Throws `EmptyStackException` if `pop` or `peek` is called on an empty stack.

### Example
Managing a stack of borrowed books in the `Library`.

```java
import java.util.ArrayDeque;
import java.util.Deque;

class Library {
    public static void main(String[] args) {
        Deque<String> bookStack = new ArrayDeque<>(); // Using ArrayDeque as Stack
        bookStack.push("1984");
        bookStack.push("Calculus");
        System.out.println("Book Stack: " + bookStack);
        // Outputs: Book Stack: [Calculus, 1984]

        String lastBorrowed = bookStack.pop();
        System.out.println("Last Borrowed: " + lastBorrowed); // Outputs: Last Borrowed: Calculus
    }
}
```

### Explanation
- `ArrayDeque` simulates a stack with `push` and `pop`.
- LIFO order ensures the last book added is the first removed.

## 7. Iterator
An **Iterator** is an object that enables traversal of a collection’s elements, typically for reading or removing elements (`java.util.Iterator`).

### Explanation
- **Characteristics**: Provides a uniform way to iterate over collections (e.g., `List`, `Set`, `Map` entries).
- **Methods**:
  - `hasNext()`: Returns `true` if more elements exist.
  - `next()`: Returns the next element.
  - `remove()`: Removes the last element returned by `next` (optional).
- **Enhanced For Loop**: Internally uses `Iterator` for collections implementing `Iterable`.

### Key Points
- Iterators are fail-fast; concurrent modification (e.g., adding elements during iteration) throws `ConcurrentModificationException`.
- Use `ListIterator` for `List` to support bidirectional traversal and modification.
- Preferred over manual index-based loops for collections without indices (e.g., `Set`).

### Example
Iterating over books in the `Library`.

```java
import java.util.ArrayList;
import java.util.Iterator;

public class Book {
    private String title;

    public Book(String title) {
        this.title = title;
    }

    @Override
    public String toString() {
        return "Book: " + title;
    }
}

class Library {
    public static void main(String[] args) {
        ArrayList<Book> books = new ArrayList<>();
        books.add(new Book("1984"));
        books.add(new Book("Calculus"));

        Iterator<Book> iterator = books.iterator();
        System.out.println("Books via Iterator:");
        while (iterator.hasNext()) {
            Book book = iterator.next();
            System.out.println(book);
            if (book.getTitle().equals("1984")) {
                iterator.remove(); // Remove 1984
            }
        }
        // Outputs: Book: 1984
        //         Book: Calculus

        System.out.println("After Removal: " + books);
        // Outputs: After Removal: [Book: Calculus]
    }
}
```

### Explanation
- `Iterator` traverses `books`, allowing safe removal during iteration.
- `remove` deletes “1984” without causing concurrency issues.

## 8. Generic Collections
**Generic Collections** use Java generics to enforce type safety, ensuring collections store only specified types.

### Explanation
- **Generics**: Introduced in Java 5, generics allow type parameters (e.g., `List<String>`) to specify the type of elements a collection can hold.
- **Benefits**:
  - Compile-time type checking prevents runtime errors (e.g., `ClassCastException`).
  - Eliminates explicit casting when retrieving elements.
- **Syntax**: Use angle brackets (e.g., `ArrayList<Book>`, `Map<String, Book>`).

### Key Points
- Use generics for all collections to ensure type safety.
- Raw types (e.g., `ArrayList` without `<Type>`) are discouraged; they cause warnings and potential errors.
- Wildcards (`?`, `? extends Type`, `? super Type`) provide flexibility for polymorphic collections.
- Generic methods and classes can further enhance type safety.

### Example
Using generic collections in the `Library`.

```java
import java.util.ArrayList;
import java.util.HashMap;
import java.util.List;
import java.util.Map;

public class Book {
    private String title;

    public Book(String title) {
        this.title = title;
    }

    @Override
    public String toString() {
        return "Book: " + title;
    }
}

class Library {
    public static void main(String[] args) {
        // Generic List
        List<Book> bookList = new ArrayList<>();
        bookList.add(new Book("1984"));
        bookList.add(new Book("Calculus"));
        // bookList.add("Invalid"); // Compile-time error

        System.out.println("Book List: " + bookList);
        // Outputs: Book List: [Book: 1984, Book: Calculus]

        // Generic Map
        Map<String, Book> bookMap = new HashMap<>();
        bookMap.put("978-0451524935", new Book("1984"));
        Book book = bookMap.get("978-0451524935"); // No casting needed
        System.out.println("Found: " + book);
        // Outputs: Found: Book: 1984
    }
}
```

### Explanation
- `List<Book>` ensures only `Book` objects are added, with compile-time checks.
- `Map<String, Book>` maps ISBN strings to `Book` objects, with type-safe retrieval.

## Summary
The Java Collections Framework provides versatile data structures for managing groups of objects:
- **Array vs. ArrayList**: Arrays are fixed-size and low-level; ArrayLists are dynamic and feature-rich.
- **Set**: Stores unique elements (e.g., `HashSet` for ISBNs).
- **Map**: Maps unique keys to values (e.g., `HashMap` for ISBN-to-book).
- **Queue**: FIFO processing (e.g., `LinkedList` for borrowing).
- **Deque**: Double-ended operations (e.g., `ArrayDeque` for returns).
- **Stack**: LIFO processing (e.g., `ArrayDeque` for borrowing history).
- **Iterator**: Safe traversal and modification of collections.
- **Generic Collections**: Type-safe collections using generics.

The `Library` examples demonstrate these collections in practical scenarios, highlighting their utility in managing books, ISBNs, and borrowing processes. These concepts are essential for building efficient, type-safe, and maintainable Java applications.