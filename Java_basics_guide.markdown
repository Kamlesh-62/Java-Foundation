# Java Basics Guide

This guide provides an introduction to the fundamental concepts of Java programming, designed for beginners. Each section covers a core topic with explanations and examples where necessary.

## 1. Basic Syntax
Java syntax is derived from C and C++, making it structured and readable. A Java program consists of classes and methods, with the main method serving as the entry point.

- **Class Declaration**: Every Java program is enclosed in a class.
- **Main Method**: The `public static void main(String[] args)` method is where execution begins.
- **Case Sensitivity**: Java is case-sensitive (`MyClass` is different from `myclass`).
- **Semicolons**: Statements end with a semicolon (`;`).

**Example**:
```java
public class HelloWorld {
    public static void main(String[] args) {
        System.out.println("Hello, World!");
    }
}
```
In this example, the program prints "Hello, World!" to the console. The class `HelloWorld` contains the `main` method, and `System.out.println` is used for output.

## 2. Lifecycle of a Java Program
A Java program's lifecycle involves several stages from writing code to execution:

1. **Writing**: The programmer writes source code in a `.java` file (e.g., `HelloWorld.java`).
2. **Compilation**: The Java compiler (`javac`) translates the source code into bytecode, stored in a `.class` file (e.g., `HelloWorld.class`).
3. **Execution**: The Java Virtual Machine (JVM) interprets or compiles the bytecode into machine code, which the computer executes.
4. **Runtime**: The JVM manages memory, garbage collection, and runtime operations.

The use of bytecode ensures Java's platform independence, as the JVM can run the same bytecode on any device with a compatible JVM.

## 3. Data Types
Java is a strongly typed language, meaning every variable must have a declared type. Data types are divided into two categories:

- **Primitive Types**:
  - `byte`: 8-bit integer (-128 to 127)
  - `short`: 16-bit integer (-32,768 to 32,767)
  - `int`: 32-bit integer (-2^31 to 2^31-1)
  - `long`: 64-bit integer (-2^63 to 2^63-1)
  - `float`: 32-bit floating-point
  - `double`: 64-bit floating-point
  - `char`: 16-bit Unicode character
  - `boolean`: `true` or `false`

- **Reference Types**: Objects, such as arrays, strings, or custom classes, that reference memory locations.

**Example**:
```java
int age = 25;
double salary = 50000.50;
char grade = 'A';
boolean isEmployed = true;
```

## 4. Variables and Scopes
Variables store data and must be declared with a type. Java supports three types of variables:

- **Local Variables**: Declared inside a method, only accessible within that method.
- **Instance Variables**: Declared in a class but outside methods, tied to an object.
- **Static Variables**: Declared with the `static` keyword, shared across all instances of a class.

**Scope** refers to the visibility of a variable:
- Local variables are limited to their block or method.
- Instance variables are accessible throughout the object’s lifecycle.
- Static variables are accessible as long as the class is loaded.

**Example**:
```java
public class VariableExample {
    int instanceVar = 10; // Instance variable
    static int staticVar = 20; // Static variable

    public void method() {
        int localVar = 30; // Local variable
        System.out.println(localVar); // Accessible only here
    }
}
```

## 5. Type Casting
Type casting converts a variable from one data type to another. Java supports two types:

- **Implicit Casting** (Widening): Automatically converts a smaller type to a larger type (e.g., `int` to `double`).
- **Explicit Casting** (Narrowing): Requires manual casting to convert a larger type to a smaller type, which may result in data loss.

**Example**:
```java
public class TypeCasting {
    public static void main(String[] args) {
        int myInt = 9;
        double myDouble = myInt; // Implicit casting
        System.out.println(myDouble); // Outputs 9.0

        double anotherDouble = 9.78;
        int anotherInt = (int) anotherDouble; // Explicit casting
        System.out.println(anotherInt); // Outputs 9
    }
}
```

## 6. Strings and Methods
Strings in Java are objects of the `String` class, used to represent text. They are immutable, meaning their value cannot be changed after creation.

- **Common String Methods**:
  - `length()`: Returns the string’s length.
  - `toUpperCase()`: Converts to uppercase.
  - `substring(int begin, int end)`: Extracts a portion of the string.
  - `equals(String other)`: Compares two strings for equality.

**Example**:
```java
public class StringExample {
    public static void main(String[] args) {
        String text = "Hello, Java!";
        System.out.println(text.length()); // Outputs 12
        System.out.println(text.toUpperCase()); // Outputs HELLO, JAVA!
        System.out.println(text.substring(0, 5)); // Outputs Hello
    }
}
```

## 7. Math Operations
Java supports standard arithmetic operations and provides the `Math` class for advanced calculations.

- **Arithmetic Operators**:
  - `+` (addition), `-` (subtraction), `*` (multiplication), `/` (division), `%` (modulus)
- **Math Class Methods**:
  - `Math.abs(int x)`: Returns the absolute value.
  - `Math.pow(double a, double b)`: Returns `a` raised to the power of `b`.
  - `Math.sqrt(double x)`: Returns the square root.

**Example**:
```java
public class MathExample {
    public static void main(String[] args) {
        int sum = 5 + 3; // 8
        double power = Math.pow(2, 3); // 8.0
        System.out.println(sum);
        System.out.println(power);
    }
}
```

## 8. Arrays
An array is a fixed-size collection of elements of the same data type. Arrays are zero-indexed, meaning the first element is at index 0.

- **Declaration**: `type[] arrayName = new type[size];`
- **Initialization**: Assign values during or after declaration.
- **Access**: Use `arrayName[index]` to access elements.

**Example**:
```java
public class ArrayExample {
    public static void main(String[] args) {
        int[] numbers = {1, 2, 3, 4, 5};
        System.out.println(numbers[0]); // Outputs 1
        numbers[1] = 10; // Update index 1
        System.out.println(numbers[1]); // Outputs 10
    }
}
```

## 9. Conditionals
Conditionals control the flow of execution based on conditions, using `if`, `else if`, `else`, and `switch`.

- **If Statement**:
```java
int age = 18;
if (age >= 18) {
    System.out.println("Adult");
} else {
    System.out.println("Minor");
}
```
- **Switch Statement**:
```java
int day = 2;
switch (day) {
    case 1:
        System.out.println("Monday");
        break;
    case 2:
        System.out.println("Tuesday");
        break;
    default:
        System.out.println("Invalid day");
}
```

## 10. Loops
Loops allow repeated execution of code. Java supports `for`, `while`, and `do-while` loops.

- **For Loop**: Used when the number of iterations is known.
```java
for (int i = 0; i < 5; i++) {
    System.out.println(i); // Outputs 0 to 4
}
```
- **While Loop**: Used when the number of iterations is unknown.
```java
int i = 0;
while (i < 5) {
    System.out.println(i); // Outputs 0 to 4
    i++;
}
```
- **Do-While Loop**: Executes at least once.
```java
int j = 0;
do {
    System.out.println(j); // Outputs 0 to 4
    j++;
} while (j < 5);
```

## 11. Basics of Object-Oriented Programming (OOP)
Java is an object-oriented language, built on four key principles:

- **Encapsulation**: Bundling data (fields) and methods that operate on it, often using access modifiers (`private`, `public`).
- **Inheritance**: Allows a class to inherit properties and methods from another using the `extends` keyword.
- **Polymorphism**: Enables objects to be treated as instances of their parent class, often through method overriding or overloading.
- **Abstraction**: Hides complex details and exposes only necessary parts, often using abstract classes or interfaces.

**Example** (Encapsulation and Class):
```java
public class Person {
    private String name; // Encapsulated field
    private int age;

    // Constructor
    public Person(String name, int age) {
        this.name = name;
        this.age = age;
    }

    // Getter
    public String getName() {
        return name;
    }

    // Setter
    public void setName(String name) {
        this.name = name;
    }
}

class Main {
    public static void main(String[] args) {
        Person person = new Person("Alice", 25);
        System.out.println(person.getName()); // Outputs Alice
    }
}
```

This guide provides a foundation for understanding Java programming. Each topic builds on the previous, preparing you for more advanced concepts.
