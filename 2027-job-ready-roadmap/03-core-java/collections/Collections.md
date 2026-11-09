# Java Core & Collections Framework – Important Topics & Learning Resources

## 1. Java Fundamentals

- [ ] Introduction to Java
- [ ] Features of Java
- [ ] Java Architecture (JDK, JRE, JVM)
- [ ] Java Compilation and Execution
- [ ] Bytecode
- [ ] Java Data Types
- [ ] Primitive vs Reference Types
- [ ] Variables and Constants
- [ ] Type Casting
- [ ] Operators and Expressions
- [ ] Conditional Statements
- [ ] Loops (for, while, do-while)
- [ ] Break and Continue
- [ ] Arrays
- [ ] Multidimensional Arrays
- [ ] Strings
- [ ] String Immutability
- [ ] String Pool
- [ ] StringBuilder vs StringBuffer
- [ ] Wrapper Classes
- [ ] Autoboxing and Unboxing
- [ ] Command-Line Arguments
- [ ] Scanner and Input Handling

**Resources:**
- [Java Official Documentation](https://docs.oracle.com/en/java/)
- [W3Schools – Java](https://www.w3schools.com/java/)
- [Java Programming – University of Helsinki](https://java-programming.mooc.fi/)

## 2. Object-Oriented Programming (OOP)

- [ ] Classes and Objects
- [ ] Constructors
- [ ] Default and Parameterized Constructors
- [ ] `this` Keyword
- [ ] `static` Keyword
- [ ] Instance vs Static Members
- [ ] Encapsulation
- [ ] Inheritance
- [ ] Single, Multilevel and Hierarchical Inheritance
- [ ] Multiple Inheritance Limitations
- [ ] Polymorphism
- [ ] Compile-Time Polymorphism
- [ ] Runtime Polymorphism
- [ ] Method Overloading
- [ ] Method Overriding
- [ ] Abstraction
- [ ] Abstract Classes
- [ ] Interfaces
- [ ] Interface Default and Static Methods
- [ ] `super` Keyword
- [ ] Access Modifiers (public, private, protected, default)
- [ ] Packages
- [ ] Association, Aggregation and Composition
- [ ] Object Class
- [ ] `equals()` and `hashCode()`
- [ ] `toString()`
- [ ] `final` Keyword
- [ ] Nested and Inner Classes
- [ ] Anonymous Classes
- [ ] Enum

**Resources:**
- [Oracle – Classes and Objects](https://docs.oracle.com/javase/tutorial/java/javaOO/)
- [Java OOP Concepts](https://www.geeksforgeeks.org/object-oriented-programming-oops-concept-in-java/)

## 3. Java Memory Management and JVM Internals

- [ ] JVM Architecture
- [ ] Class Loader
- [ ] Loading, Linking and Initialization
- [ ] Runtime Data Areas
- [ ] Heap Memory
- [ ] Stack Memory
- [ ] Method Area
- [ ] Program Counter Register
- [ ] Native Method Stack
- [ ] Execution Engine
- [ ] JIT Compiler
- [ ] Garbage Collection
- [ ] Garbage Collection Algorithms
- [ ] Young and Old Generation
- [ ] Stack vs Heap
- [ ] Memory Leaks
- [ ] Object Lifecycle
- [ ] Java Pass-by-Value
- [ ] Strong, Weak, Soft and Phantom References

**Resources:**
- [JVM Specification](https://docs.oracle.com/javase/specs/)
- [Oracle – Java Garbage Collection](https://docs.oracle.com/en/java/javase/)

## 4. Exception Handling

- [ ] Introduction to Exceptions
- [ ] Exception Hierarchy
- [ ] Checked Exceptions
- [ ] Unchecked Exceptions
- [ ] Errors vs Exceptions
- [ ] try-catch Blocks
- [ ] Multiple Catch Blocks
- [ ] Nested try-catch
- [ ] finally Block
- [ ] throw Keyword
- [ ] throws Keyword
- [ ] Custom Exceptions
- [ ] Exception Propagation
- [ ] Try-with-Resources
- [ ] AutoCloseable Interface
- [ ] Best Practices for Exception Handling

**Resources:**
- [Oracle – Exceptions](https://docs.oracle.com/javase/tutorial/essential/exceptions/)

## 5. Java Collections Framework

- [ ] Introduction to Collections Framework
- [ ] Collection vs Collections
- [ ] Iterable Interface
- [ ] Collection Interface
- [ ] List Interface
- [ ] Set Interface
- [ ] Queue Interface
- [ ] Deque Interface
- [ ] Map Interface
- [ ] Iterator
- [ ] ListIterator
- [ ] Comparable Interface
- [ ] Comparator Interface

### List Implementations

- [ ] ArrayList
- [ ] LinkedList
- [ ] Vector
- [ ] Stack
- [ ] ArrayList vs LinkedList
- [ ] Vector vs ArrayList
- [ ] Stack vs Deque
- [ ] List Traversal
- [ ] Sorting Lists
- [ ] Removing Duplicates

### Set Implementations

- [ ] HashSet
- [ ] LinkedHashSet
- [ ] TreeSet
- [ ] EnumSet
- [ ] HashSet vs LinkedHashSet
- [ ] HashSet vs TreeSet
- [ ] Duplicate Prevention
- [ ] Ordering and Sorting

### Queue and Deque Implementations

- [ ] PriorityQueue
- [ ] ArrayDeque
- [ ] LinkedList as Queue
- [ ] Queue Operations
- [ ] Deque Operations
- [ ] FIFO and LIFO
- [ ] Priority-Based Processing

### Map Implementations

- [ ] HashMap
- [ ] LinkedHashMap
- [ ] TreeMap
- [ ] Hashtable
- [ ] ConcurrentHashMap
- [ ] HashMap vs Hashtable
- [ ] HashMap vs LinkedHashMap
- [ ] HashMap vs TreeMap
- [ ] Hashing Mechanism
- [ ] Hash Collisions
- [ ] Internal Working of HashMap
- [ ] Load Factor
- [ ] Initial Capacity
- [ ] Rehashing
- [ ] `hashCode()` and `equals()`
- [ ] Null Keys and Values

### Advanced Collections Concepts

- [ ] Fail-Fast vs Fail-Safe Iterators
- [ ] ConcurrentModificationException
- [ ] Iterator vs ListIterator
- [ ] Collections Utility Class
- [ ] `Collections.sort()`
- [ ] `Collections.reverse()`
- [ ] `Collections.shuffle()`
- [ ] `Collections.unmodifiableList()`
- [ ] Immutable Collections
- [ ] Synchronized Collections
- [ ] Concurrent Collections
- [ ] CopyOnWriteArrayList
- [ ] BlockingQueue
- [ ] WeakHashMap
- [ ] IdentityHashMap

**Resources:**
- [Oracle – Collections Framework](https://docs.oracle.com/javase/tutorial/collections/)
- [Java Collections API](https://docs.oracle.com/en/java/javase/17/docs/api/java.base/java/util/package-summary.html)
- [Baeldung – Java Collections](https://www.baeldung.com/java-collections)

## 6. Generics in Java

- [ ] Introduction to Generics
- [ ] Generic Classes
- [ ] Generic Methods
- [ ] Generic Interfaces
- [ ] Type Parameters
- [ ] Bounded Type Parameters
- [ ] Upper Bounded Wildcards
- [ ] Lower Bounded Wildcards
- [ ] Unbounded Wildcards
- [ ] Type Erasure
- [ ] Generic Inheritance
- [ ] PECS Principle (Producer Extends, Consumer Super)
- [ ] Raw Types
- [ ] Generic Type Safety

**Resources:**
- [Oracle – Generics](https://docs.oracle.com/javase/tutorial/java/generics/)

## 7. Java 8+ Features and Functional Programming

- [ ] Lambda Expressions
- [ ] Functional Interfaces
- [ ] Built-in Functional Interfaces
- [ ] Predicate
- [ ] Function
- [ ] Consumer
- [ ] Supplier
- [ ] BiFunction
- [ ] Method References
- [ ] Constructor References
- [ ] Stream API
- [ ] Intermediate Operations
- [ ] Terminal Operations
- [ ] filter()
- [ ] map()
- [ ] flatMap()
- [ ] reduce()
- [ ] collect()
- [ ] sorted()
- [ ] distinct()
- [ ] limit() and skip()
- [ ] forEach()
- [ ] findFirst() and findAny()
- [ ] anyMatch(), allMatch() and noneMatch()
- [ ] Collectors API
- [ ] groupingBy()
- [ ] partitioningBy()
- [ ] Parallel Streams
- [ ] Optional Class
- [ ] Date and Time API
- [ ] LocalDate
- [ ] LocalTime
- [ ] LocalDateTime
- [ ] ZonedDateTime
- [ ] Instant
- [ ] Default and Static Interface Methods

**Modern Java Features:**

- [ ] var Keyword (Java 10)
- [ ] Text Blocks (Java 15)
- [ ] Records (Java 16)
- [ ] Sealed Classes (Java 17)
- [ ] Switch Expressions
- [ ] Pattern Matching for instanceof
- [ ] Pattern Matching for switch
- [ ] Virtual Threads (Java 21)
- [ ] Record Patterns
- [ ] Sequenced Collections (Java 21)

**Resources:**
- [Oracle – Java 8 Features](https://www.oracle.com/java/technologies/javase/8-whats-new.html)
- [Dev.java](https://dev.java/learn/)
- [Baeldung – Java Streams](https://www.baeldung.com/java-8-streams)

## 8. Multithreading and Concurrency

- [ ] Process vs Thread
- [ ] Thread Lifecycle
- [ ] Creating Threads
- [ ] Thread Class
- [ ] Runnable Interface
- [ ] Callable Interface
- [ ] Thread Priority
- [ ] Thread Scheduling
- [ ] start() vs run()
- [ ] sleep() and join()
- [ ] Synchronization
- [ ] Synchronized Methods and Blocks
- [ ] Race Conditions
- [ ] Deadlocks
- [ ] Volatile Keyword
- [ ] Atomic Variables
- [ ] wait(), notify() and notifyAll()
- [ ] Executor Framework
- [ ] ExecutorService
- [ ] Future and CompletableFuture
- [ ] Thread Pools
- [ ] ForkJoinPool
- [ ] Concurrent Collections
- [ ] Locks and ReentrantLock
- [ ] ReadWriteLock
- [ ] Semaphore
- [ ] CountDownLatch
- [ ] CyclicBarrier
- [ ] BlockingQueue
- [ ] Virtual Threads

**Resources:**
- [Oracle – Concurrency](https://docs.oracle.com/javase/tutorial/essential/concurrency/)
- [Baeldung – Java Concurrency](https://www.baeldung.com/java-concurrency)

## 9. Java Input/Output and File Handling

- [ ] Introduction to Java I/O
- [ ] Byte Streams
- [ ] Character Streams
- [ ] InputStream and OutputStream
- [ ] Reader and Writer
- [ ] File Class
- [ ] FileInputStream
- [ ] FileOutputStream
- [ ] BufferedReader
- [ ] BufferedWriter
- [ ] FileReader and FileWriter
- [ ] Serialization
- [ ] Deserialization
- [ ] transient Keyword
- [ ] NIO Fundamentals
- [ ] Path and Paths
- [ ] Files API
- [ ] File Channels
- [ ] ByteBuffer
- [ ] WatchService

**Resources:**
- [Oracle – Basic I/O](https://docs.oracle.com/javase/tutorial/essential/io/)
- [Java NIO Documentation](https://docs.oracle.com/en/java/javase/17/docs/api/java.base/java/nio/package-summary.html)

## 10. Java Networking

- [ ] Networking Fundamentals
- [ ] TCP vs UDP
- [ ] IP Address and Ports
- [ ] Socket Programming
- [ ] ServerSocket
- [ ] Socket
- [ ] URL and URI
- [ ] HTTP Fundamentals
- [ ] HttpURLConnection
- [ ] Java HTTP Client API
- [ ] GET and POST Requests
- [ ] Request and Response Handling
- [ ] JSON Processing
- [ ] Client-Server Architecture
- [ ] Basic REST API Communication

**Resources:**
- [Oracle – Networking](https://docs.oracle.com/javase/tutorial/networking/)
- [Java HTTP Client API](https://docs.oracle.com/en/java/javase/17/docs/api/java.net.http/java/net/http/package-summary.html)

## 11. Java Annotations and Reflection

- [ ] Introduction to Annotations
- [ ] Built-in Annotations
- [ ] @Override
- [ ] @Deprecated
- [ ] @SuppressWarnings
- [ ] Custom Annotations
- [ ] Annotation Retention
- [ ] Annotation Targets
- [ ] Reflection API
- [ ] Class Object
- [ ] Accessing Methods and Fields
- [ ] Dynamic Object Creation
- [ ] Java Reflection Limitations
- [ ] Dynamic Proxies

**Resources:**
- [Oracle – Annotations](https://docs.oracle.com/javase/tutorial/java/annotations/)
- [Oracle – Reflection](https://docs.oracle.com/javase/tutorial/reflect/)

## 12. Java Database Connectivity (JDBC)

- [ ] Introduction to JDBC
- [ ] JDBC Architecture
- [ ] JDBC Drivers
- [ ] Database Connection
- [ ] DriverManager
- [ ] Connection Interface
- [ ] Statement
- [ ] PreparedStatement
- [ ] CallableStatement
- [ ] ResultSet
- [ ] CRUD Operations
- [ ] SQL Injection Prevention
- [ ] Transactions
- [ ] Commit and Rollback
- [ ] Batch Processing
- [ ] Connection Pooling
- [ ] JDBC Exception Handling

**Resources:**
- [Oracle – JDBC Basics](https://docs.oracle.com/javase/tutorial/jdbc/basics/)
- [Baeldung – JDBC](https://www.baeldung.com/java-jdbc)

## 13. Java Testing and Development Tools

- [ ] JDK Installation and Configuration
- [ ] IntelliJ IDEA
- [ ] Eclipse
- [ ] VS Code Java Extensions
- [ ] Maven
- [ ] Gradle
- [ ] Dependency Management
- [ ] Build Lifecycle
- [ ] JUnit 5
- [ ] Unit Testing
- [ ] Integration Testing
- [ ] Mockito
- [ ] Debugging
- [ ] Breakpoints
- [ ] Logging
- [ ] SLF4J
- [ ] Logback
- [ ] Git Integration

**Resources:**
- [Maven Documentation](https://maven.apache.org/guides/)
- [Gradle Documentation](https://docs.gradle.org/current/userguide/userguide.html)
- [JUnit 5](https://junit.org/junit5/docs/current/user-guide/)
- [Mockito](https://site.mockito.org/)

## 14. Important Java Interview Topics

- [ ] JDK vs JRE vs JVM
- [ ] How Java Code Executes
- [ ] Why Java Is Platform Independent
- [ ] String vs StringBuilder vs StringBuffer
- [ ] String Pool
- [ ] == vs equals()
- [ ] equals() and hashCode() Contract
- [ ] Overloading vs Overriding
- [ ] Abstract Class vs Interface
- [ ] Composition vs Inheritance
- [ ] Checked vs Unchecked Exceptions
- [ ] final vs finally vs finalize()
- [ ] Stack vs Heap
- [ ] Garbage Collection
- [ ] HashMap Internal Working
- [ ] HashMap vs Hashtable
- [ ] ArrayList vs LinkedList
- [ ] HashSet vs TreeSet
- [ ] Comparable vs Comparator
- [ ] Fail-Fast vs Fail-Safe
- [ ] ConcurrentHashMap Internal Working
- [ ] Iterator vs ListIterator
- [ ] Lambda Expressions
- [ ] Functional Interfaces
- [ ] Stream API
- [ ] map() vs flatMap()
- [ ] Optional Class
- [ ] Thread vs Runnable
- [ ] Synchronization
- [ ] Deadlock
- [ ] volatile vs synchronized
- [ ] ExecutorService
- [ ] CompletableFuture
- [ ] Virtual Threads

## Best Overall Learning Resources

| Resource | Best For |
|---|---|
| [Dev.java](https://dev.java/learn/) | Official modern Java learning |
| [Oracle Java Tutorials](https://docs.oracle.com/javase/tutorial/) | Core Java concepts |
| [Java Programming MOOC](https://java-programming.mooc.fi/) | Practical Java exercises |
| [Baeldung](https://www.baeldung.com/) | Advanced Java concepts |
| [GeeksforGeeks Java](https://www.geeksforgeeks.org/java/) | Interview preparation |
| [Java Collections Documentation](https://docs.oracle.com/en/java/javase/17/docs/api/java.base/java/util/package-summary.html) | Collections reference |
| [LeetCode](https://leetcode.com/) | Java DSA practice |
| [CodingBat](https://codingbat.com/java) | Java coding exercises |

## Important Java Collections – Quick Reference

### List

```java
List<Integer> list = new ArrayList<>();
list.add(10);
list.add(20);
list.remove(0);
list.get(0);
```

### Set

```java
Set<Integer> set = new HashSet<>();
set.add(10);
set.add(20);
set.add(10);
System.out.println(set);
```

### Map

```java
Map<String, Integer> map = new HashMap<>();
map.put("Java", 100);
map.put("Python", 90);

System.out.println(map.get("Java"));
System.out.println(map.containsKey("Java"));
```

### Queue

```java
Queue<Integer> queue = new LinkedList<>();
queue.offer(10);
queue.offer(20);
queue.poll();
queue.peek();
```

### PriorityQueue

```java
PriorityQueue<Integer> pq = new PriorityQueue<>();
pq.offer(30);
pq.offer(10);
pq.offer(20);

System.out.println(pq.poll());
```

### Stack Using ArrayDeque

```java
Deque<Integer> stack = new ArrayDeque<>();
stack.push(10);
stack.push(20);
stack.pop();
stack.peek();
```

### Sorting with Comparator

```java
List<Integer> numbers = Arrays.asList(5, 2, 8, 1);

numbers.sort(Comparator.naturalOrder());

numbers.sort(Comparator.reverseOrder());
```

### Stream API

```java
List<Integer> numbers = List.of(1, 2, 3, 4, 5);

List<Integer> result = numbers.stream()
    .filter(n -> n % 2 == 0)
    .map(n -> n * 2)
    .toList();

System.out.println(result);
```

## Important Concepts to Master

- **JVM:** Executes Java bytecode and manages runtime resources.
- **JDK:** Development kit containing tools for developing Java applications.
- **JRE:** Runtime environment for running Java applications.
- **OOP:** Programming approach based on objects and classes.
- **Encapsulation:** Bundling data and methods while controlling access.
- **Inheritance:** Reusing and extending existing class functionality.
- **Polymorphism:** One interface or method contract with different implementations.
- **Abstraction:** Hiding implementation details behind essential interfaces.
- **Collections Framework:** Provides reusable data structures and algorithms.
- **HashMap:** Key-value data structure based on hashing.
- **Generics:** Provides compile-time type safety and reusable code.
- **Stream API:** Supports declarative processing of data collections.
- **Lambda:** Concise way to represent functional behavior.
- **Multithreading:** Running multiple threads within one process.
- **Garbage Collection:** Automatically reclaims unreachable objects.
- **JDBC:** Connects Java applications to databases.