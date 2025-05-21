# Concurrency in Java

## Overview
Concurrency in Java enables multiple tasks to run simultaneously, leveraging multi-core processors or improving responsiveness. Key components include threads, virtual threads (introduced in Java 19), the Java Memory Model (JMM), and synchronization mechanisms like the `volatile` keyword. Concurrency is critical for performance but introduces complexity, such as race conditions and deadlocks.

## Core Concepts
- **Threads**: Independent paths of execution within a program.
- **Virtual Threads**: Lightweight threads for high-throughput concurrency (Java 19+).
- **Java Memory Model (JMM)**: Rules governing how threads interact with memory.
- **volatile Keyword**: Ensures visibility and ordering of variable updates across threads.

## Threads
### Purpose
Threads allow concurrent execution of tasks, such as handling multiple user requests or parallelizing computations.

### Creation
- **Extend `Thread`**:
  ```java
  class MyThread extends Thread {
      public void run() {
          System.out.println("Running in thread: " + Thread.currentThread().getName());
      }
  }
  MyThread thread = new MyThread();
  thread.start();
  ```
- **Implement `Runnable`** (preferred):
  ```java
  class MyRunnable implements Runnable {
      public void run() {
          System.out.println("Running in thread: " + Thread.currentThread().getName());
      }
  }
  Thread thread = new Thread(new MyRunnable());
  thread.start();
  ```

### Key Methods
- `start()`: Begins thread execution.
- `run()`: Defines thread’s task.
- `join()`: Waits for thread completion.
- `sleep(long millis)`: Pauses thread execution.
- `interrupt()`: Signals thread to stop (cooperative).

### Analogy
Threads are like workers in a kitchen, each handling a separate task (e.g., chopping, cooking). They share resources (e.g., ingredients), requiring coordination to avoid conflicts.

## Virtual Threads
### Purpose
Introduced in Java 19 (Project Loom), virtual threads are lightweight, managed by the JVM, not the OS. They simplify high-concurrency applications (e.g., web servers) by reducing overhead compared to platform threads.

### Creation
```java
Thread virtualThread = Thread.ofVirtual().start(() -> {
    System.out.println("Running in virtual thread: " + Thread.currentThread().getName());
});
```

### Benefits
- **Scalability**: Thousands of virtual threads vs. limited platform threads.
- **Simplicity**: Write blocking code without performance penalties.
- **Low Overhead**: Managed by JVM, not OS threads.

### Example
```java
public class VirtualThreadDemo {
    public static void main(String[] args) throws InterruptedException {
        Runnable task = () -> {
            System.out.println("Task in: " + Thread.currentThread());
            try {
                Thread.sleep(1000); // Blocking is efficient with virtual threads
            } catch (InterruptedException e) {
                Thread.currentThread().interrupt();
            }
        };

        // Start 1000 virtual threads
        Thread[] threads = new Thread[1000];
        for (int i = 0; i < 1000; i++) {
            threads[i] = Thread.ofVirtual().start(task);
        }
        for (Thread t : threads) {
            t.join();
        }
        System.out.println("All tasks completed");
    }
}
```
**Explanation**: Virtual threads handle thousands of tasks efficiently, as they’re lightweight and multiplexed onto fewer OS threads.

### Gotchas
- Virtual threads are best for I/O-bound tasks, not CPU-bound tasks.
- Pinned operations (e.g., native calls) can block carrier threads.

## Java Memory Model (JMM)
### Purpose
The JMM defines how threads interact with main memory, ensuring predictable behavior for shared variables. It addresses:
- **Visibility**: When one thread’s changes are visible to others.
- **Ordering**: Sequence of operations across threads.

### Key Rules
- **Happens-Before Relationship**: Guarantees visibility and ordering (e.g., `synchronized` blocks, `volatile` writes).
- Without synchronization, updates may not be visible due to caching or reordering.

### Analogy
The JMM is like a shared notebook in the kitchen. Without rules (e.g., locking the notebook), workers might see outdated or inconsistent notes.

## volatile Keyword
### Purpose
The `volatile` keyword ensures:
- **Visibility**: Changes to a `volatile` variable are immediately visible to all threads.
- **Ordering**: Prevents reordering of `volatile` reads/writes.

### When to Use
- For simple flags or status variables shared across threads.
- When you need visibility without full synchronization.

### Example
```java
public class VolatileDemo {
    private volatile boolean running = true;

    public void start() {
        // Worker thread
        new Thread(() -> {
            while (running) {
                System.out.println("Working...");
                try {
                    Thread.sleep(500);
                } catch (InterruptedException e) {
                    Thread.currentThread().interrupt();
                }
            }
            System.out.println("Stopped");
        }).start();
    }

    public void stop() {
        running = false; // Visible to all threads due to volatile
    }

    public static void main(String[] args) throws InterruptedException {
        VolatileDemo demo = new VolatileDemo();
        demo.start();
        Thread.sleep(2000);
        demo.stop();
    }
}
```
**Explanation**:
- `running` is `volatile`, ensuring the worker thread sees the update when `stop()` is called.
- Without `volatile`, the worker might cache `running` and never stop.

### Gotchas
- `volatile` doesn’t provide atomicity (e.g., `++counter` isn’t thread-safe).
- Use for simple flags, not complex state.

## Comprehensive Example
A thread-safe task manager using threads, virtual threads, and `volatile`:
```java
import java.util.ArrayList;
import java.util.List;
import java.util.concurrent.atomic.AtomicInteger;

public class TaskManager {
    private volatile boolean shutdown = false;
    private final List<Thread> threads = new ArrayList<>();
    private final AtomicInteger taskCount = new AtomicInteger();

    public void startTasks(int numTasks, boolean useVirtualThreads) {
        Runnable task = () -> {
            while (!shutdown) {
                System.out.println(Thread.currentThread() + ": Task " + taskCount.incrementAndGet());
                try {
                    Thread.sleep(500);
                } catch (InterruptedException e) {
                    Thread.currentThread().interrupt();
                    break;
                }
            }
        };

        for (int i = 0; i < numTasks; i++) {
            Thread thread = useVirtualThreads ? Thread.ofVirtual().start(task) : new Thread(task);
            threads.add(thread);
        }
    }

    public void shutdown() throws InterruptedException {
        shutdown = true; // Visible to all threads
        for (Thread t : threads) {
            t.join();
        }
        System.out.println("Total tasks processed: " + taskCount.get());
    }

    public static void main(String[] args) throws InterruptedException {
        TaskManager manager = new TaskManager();
        manager.startTasks(5, true); // Use virtual threads
        Thread.sleep(2000);
        manager.shutdown();
    }
}
```
**Explanation**:
- Uses `volatile` for `shutdown` to ensure visibility.
- Supports both platform and virtual threads.
- `AtomicInteger` ensures thread-safe task counting.
- Demonstrates clean shutdown with `join()`.

## Best Practices
- **Use `Runnable` over `Thread`**: More flexible and reusable.
- **Prefer Virtual Threads for I/O**: Ideal for tasks like HTTP requests or file I/O.
- **Use `volatile` Sparingly**: For flags, not complex operations.
- **Synchronize Carefully**: Use `synchronized`, `Lock`, or `Atomic` classes for atomicity.
- **Avoid Shared Mutable State**: Prefer immutable objects or thread-local variables.
- **Handle Interruptions**: Check `Thread.interrupted()` in loops.
- **Use Executors**: `ExecutorService` simplifies thread management:
  ```java
  ExecutorService executor = Executors.newVirtualThreadPerTaskExecutor();
  executor.submit(task);
  executor.shutdown();
  ```

## Common Pitfalls
- **Race Conditions**: Unsynchronized access to shared data (use `synchronized` or `Lock`).
- **Deadlocks**: Multiple threads waiting on each other (avoid circular locks).
- **Starvation**: Low-priority threads never run (balance thread priorities).
- **Overusing `volatile`**: Doesn’t ensure atomicity for operations like increments.
- **Ignoring Interruptions**: Always handle `InterruptedException`.

## Performance Considerations
- **Platform Threads**: Limited by OS resources, expensive for thousands of threads.
- **Virtual Threads**: Low overhead, ideal for high concurrency.
- **JMM Overhead**: Synchronization or `volatile` adds slight overhead but ensures correctness.

## Side-by-Side Comparison
| Feature             | Platform Threads                | Virtual Threads                |
|---------------------|---------------------------------|--------------------------------|
| Creation            | `new Thread()`                 | `Thread.ofVirtual()`           |
| Overhead            | High (OS-level)                | Low (JVM-managed)              |
| Scalability         | Limited (100s)                 | High (1000s+)                  |
| Use Case            | CPU-bound tasks                | I/O-bound tasks                |
| Example             | `new Thread(runnable).start()` | `Thread.ofVirtual().start(r)`  |

## Visual Explanation
Sketch:
- **Threads**: Multiple workers (threads) sharing a workbench (memory).
- **Virtual Threads**: A manager (JVM) assigning tasks to a few workers, multiplexing many virtual tasks.
- **JMM**: A rulebook ensuring workers see the latest notes in the shared notebook.
- **volatile**: A highlighted note in the rulebook, instantly visible to all workers.

## Learning Path
1. **Master Threads**:
   - Practice creating threads with `Runnable` and `Thread`.
   - Experiment with `join()`, `sleep()`, and `interrupt()`.
   - Example: Build a producer-consumer system using `synchronized`.
2. **Explore Virtual Threads**:
   - Use Java 21+ to experiment with `Thread.ofVirtual()`.
   - Example: Simulate a web server handling 1000 concurrent requests.
3. **Understand JMM**:
   - Study happens-before rules in the Java Language Specification.
   - Experiment with `volatile` vs. `synchronized`.
4. **Use Concurrency Utilities**:
   - Learn `ExecutorService`, `ForkJoinPool`, and `CompletableFuture`.
   - Example: Parallelize a file-processing task with `Executors.newVirtualThreadPerTaskExecutor()`.
5. **Tools**:
   - **IDE**: IntelliJ IDEA for thread debugging.
   - **Libraries**: Java Concurrency Utilities (`java.util.concurrent`).
   - **Open-Source**: Study Spring’s `@Async` or Reactor for reactive programming.
6. **Projects**:
   - Build a chat server using virtual threads.
   - Implement a parallel matrix multiplication with `ForkJoinPool`.

## Conclusion
Concurrency in Java, through threads, virtual threads, the JMM, and `volatile`, enables efficient multitasking but requires careful management. Use platform threads for CPU-bound tasks, virtual threads for I/O-bound tasks, and `volatile` for simple shared flags. By mastering these tools and following best practices, you can build scalable, thread-safe applications. Start with basic threading, then explore virtual threads and advanced utilities like `ExecutorService`.