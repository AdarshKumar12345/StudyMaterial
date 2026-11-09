# Modern Java File Handling & NIO.2 Architecture

## 1. Introduction & Prerequisites
Modern enterprise Java applications interact extensively with file systems for configuration loading, batch processing, document generation, and log analysis. Java NIO.2 (`java.nio.file`), introduced in Java 7 and refined in recent releases, replaces legacy `java.io.File` with non-blocking, asynchronous, and scalable file system operations.

- **Prerequisites:** Java Fundamentals, Streams, Exception Handling.
- **Estimated Study Time:** 8 hours
- **Difficulty:** Intermediate

---

## 2. Detailed Learning Objectives
- Master the modern NIO.2 abstractions: `Path`, `Paths`, and the `Files` utility class.
- Read and write files efficiently using `Files.lines()`, `Files.readAllBytes()`, and buffered streams.
- Navigate directories using `Files.walk()`, `Files.find()`, and directory streams.
- Understand low-level I/O primitives: `FileChannel`, `ByteBuffer`, and Memory-Mapped Files (`MappedByteBuffer`).
- Monitor directory changes in real time using the `WatchService` API.

---

## 3. Beginner-to-Advanced Topic Checklist
- [ ] Legacy `java.io.File` vs Modern `java.nio.file.Path`
- [ ] Resolving, normalizing, and relativizing paths
- [ ] Atomicity in file operations (`StandardCopyOption.ATOMIC_MOVE`)
- [ ] Reading large files with memory efficiency (`Files.lines()` lazy Stream)
- [ ] Managing file attributes, POSIX permissions, and ownership
- [ ] ByteBuffers: Capacity, Position, Limit, Mark, and `flip()`
- [ ] FileChannel for zero-copy file transfers (`transferTo` / `transferFrom`)
- [ ] Memory-Mapped Files for ultra-low latency reads/writes
- [ ] File locking (`FileLock`) for multi-process safety
- [ ] Real-time directory monitoring with `WatchService`

---

## 4. Free Documentation Resources
- **Oracle Java NIO.2 Tutorial:** [https://docs.oracle.com/javase/tutorial/essential/io/fileio.html](https://docs.oracle.com/javase/tutorial/essential/io/fileio.html)
- **Baeldung — Java NIO Technology Guide:** [https://www.baeldung.com/java-nio-2-file-api](https://www.baeldung.com/java-nio-2-file-api)
- **Dev.java — File I/O (Featuring NIO.2):** [https://dev.java/learn/api/file-io/](https://dev.java/learn/api/file-io/)

---

## 5. Free Video Courses & Tutorials
- **Java Brains:** Java File I/O and NIO.2
- **Amigoscode:** Java File Handling Tutorial
- **Defog Tech:** Memory-Mapped Files in Java

---

## 6. Important Concepts Explained

### ByteBuffer Mechanics: Position, Limit, and Capacity
A `ByteBuffer` is a contiguous block of memory with three critical pointers:
1. **Capacity:** Maximum number of bytes the buffer can hold.
2. **Position:** Next byte to be read or written.
3. **Limit:** First byte that cannot be read or written.

When transitioning from writing into the buffer to reading from the buffer, you must call `buffer.flip()`. This sets `limit = position` and resets `position = 0`.

```
Buffer state after writing 4 bytes:
[ B0 | B1 | B2 | B3 |   |   |   ]
                      ▲           ▲
                   Position      Limit / Capacity

Buffer state after flip() [Ready to Read]:
[ B0 | B1 | B2 | B3 |   |   |   ]
  ▲                   ▲           ▲
Position            Limit      Capacity
```

---

## 7. Practical Code Examples

### Memory-Efficient Large Log Analyzer using `Files.lines()`
```java
package com.roadmap.nio;

import java.io.IOException;
import java.nio.file.*;
import java.util.Map;
import java.util.stream.Collectors;
import java.util.stream.Stream;

public class FastLogScanner {

    public static void main(String[] args) throws IOException {
        Path logFilePath = Path.of("/var/log/application.log");

        if (!Files.exists(logFilePath)) {
            System.err.println("File not found: " + logFilePath);
            return;
        }

        // Files.lines() lazily streams lines without loading the whole file into RAM
        try (Stream<String> lines = Files.lines(logFilePath)) {
            Map<String, Long> errorFrequency = lines
                .filter(line -> line.contains("ERROR"))
                .map(line -> extractErrorCode(line))
                .collect(Collectors.groupingBy(code -> code, Collectors.counting()));

            System.out.println("Error Frequency: " + errorFrequency);
        }
    }

    private static String extractErrorCode(String logLine) {
        return logLine.replaceAll(".*(ERR_[0-9]+).*", "$1");
    }
}
```

---

## 8. Practice Assignments
1. **Assignment 1:** Build a recursive file search CLI that searches a directory for files matching a regex pattern and size threshold using `Files.walkFileTree()`.
2. **Assignment 2:** Implement a real-time folder watcher using `WatchService` that logs whenever a new file is created or deleted in a directory.

---

## 9. Mini-Projects
- **High-Performance CSV Ingestion Service:** Build an ingestion pipeline that reads a 500MB CSV file using `FileChannel` and `ByteBuffer`, parses records into Java Records, and computes summary statistics.

---

## 10. Interview Questions & Detailed Answers

### Q1: Why should you prefer `Files.lines()` over `Files.readAllLines()` for large files?
**Answer:** `Files.readAllLines()` reads the entire file into an `ArrayList<String>` in memory simultaneously. If the file is 2GB, it consumes several gigabytes of JVM heap, risking an `OutOfMemoryError`. `Files.lines()` returns a lazy `Stream<String>` backed by a `BufferedReader`, streaming lines on-demand with constant $O(1)$ memory consumption.

### Q2: What is the purpose of `FileChannel.transferTo()` and zero-copy?
**Answer:** In standard I/O, copying data from a file to a network socket involves multiple context switches between user space and kernel space and redundant buffer copies. `transferTo()` invokes the operating system's native `sendfile()` system call, allowing the OS kernel to transfer bytes directly from the page cache to the network socket buffer without copying data into JVM user memory ("Zero-Copy").

---

## 11. Revision Notes
- Always wrap `Files.lines()` in a `try-with-resources` block to close the underlying file descriptor.
- Use `Path.of("a", "b", "c")` for cross-platform file path construction.
- Use `StandardCopyOption.REPLACE_EXISTING` when overwriting destination files.

---

## 12. Topic Completion Checklist
- [ ] Mastered Path, Paths, and Files APIs
- [ ] Used `Files.lines()` for memory-safe processing
- [ ] Understood ByteBuffer flip and position markers
- [ ] Implemented a directory watcher with `WatchService`
