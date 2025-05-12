# Java Keyword

## 1. `void`
The `void` keyword in Java is used in method declarations to specify that the method does not return any value. It indicates that the method performs an action but does not produce a result to be returned to the caller.

### Usage
- **Method Declaration**: Use `void` when defining a method that executes a task without returning data.
- **Syntax**: 
  ```java
  public void methodName() {
      // Method body
  }
  ```
- **Example**:
  ```java
  public void printMessage() {
      System.out.println("Hello, Java!");
  }
  ```
  In this example, `printMessage` prints a message but does not return a value.

### Key Points
- **No Return Statement Required**: Methods with `void` do not need a `return` statement, but an empty `return;` can be used to exit early.
- **Common Use Cases**: Used for methods that modify object state, perform I/O operations, or trigger actions (e.g., logging, updating UI).
- **Not Applicable for Constructors**: Constructors cannot use `void` as they do not have return types.

### Notes
- If a method needs to return a value, use a specific return type (e.g., `int`, `String`) instead of `void`.
- Misusing `void` for methods that should return data can lead to poor code design.

## 2. `public` Keyword

### Overview
The `public` keyword is an access modifier that indicates a class, method, or variable is accessible from any other class in the program.

### Usage
- **Applied To**: Classes, methods, fields, and constructors.
- **Syntax**:
  ```java
  public class MyClass {
      public int myVariable;
      public void myMethod() {
          // Method body
      }
  }
  ```
- **Example**:
  ```java
  public class Greeting {
      public void sayHello() {
          System.out.println("Hello!");
      }
  }
  ```
  Here, `sayHello` can be called from any other class.

### Key Points
- **Accessibility**: No restrictions; accessible everywhere.
- **Use Case**: Used for methods or fields intended for external use, such as API methods or public interfaces.
- **Caution**: Overusing `public` can break encapsulation, exposing internal details unnecessarily.

## 3. `private` Keyword

### Overview
The `private` keyword is an access modifier that restricts access to a class, method, or variable to only within the declaring class.

### Usage
- **Applied To**: Methods, fields, and constructors (rarely classes, except nested classes).
- **Syntax**:
  ```java
  public class MyClass {
      private int myVariable;
      private void myMethod() {
          // Method body
      }
  }
  ```
- **Example**:
  ```java
  public class BankAccount {
      private double balance;
      private void updateBalance(double amount) {
          balance += amount;
      }
  }
  ```
  Here, `balance` and `updateBalance` are only accessible within `BankAccount`.

### Key Points
- **Accessibility**: Limited to the declaring class.
- **Use Case**: Protects internal data and implementation details, enforcing encapsulation.
- **Accessing Private Members**: Use public getter/setter methods to interact with private fields.

## 4. `final` Keyword

### Overview
The `final` keyword indicates that a variable, method, or class cannot be modified or extended after its initial definition.

### Usage
- **Applied To**: Variables, methods, and classes.
- **Syntax**:
  ```java
  public final class MyClass {
      final int CONSTANT = 10;
      final void myMethod() {
          // Method body
      }
  }
  ```
- **Example**:
  ```java
  public class Circle {
      final double PI = 3.14159;
      final void calculateArea(double radius) {
          System.out.println("Area: " + (PI * radius * radius));
      }
  }
  ```
  Here, `PI` cannot be reassigned, and `calculateArea` cannot be overridden.

### Key Points
- **Variables**: Makes them constants (value cannot change after initialization).
- **Methods**: Prevents overriding in subclasses.
- **Classes**: Prevents inheritance (e.g., `final class` cannot be extended).
- **Use Case**: Ensures immutability or protects critical methods/classes from modification.


