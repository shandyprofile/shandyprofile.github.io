---
title: Input and Output
description: >-
  Java provides standard ways to interact.
author: [shandy]
date: 2025-06-17
updateDate: 
categories: [(Java) Object-oriented programming, (Java) Basic Language]
tags: [(Java) Basic Language]
sort_index: 107
# pin: true
# media_subpath: '/posts/02'
---

# A. Input and Output (Standard IO)
Java provides standard ways to interact with the user via console:
- **Input**: Reading data from the user (keyboard).
- **Output**: Displaying data to the screen.

## 1. Standard Output
- Used to print data to the screen using System.out.

| Method                 | Description                      |
| ---------------------- | -------------------------------- |
| `System.out.print()`   | Prints **without** newline       |
| `System.out.println()` | Prints **with** newline          |
| `System.out.printf()`  | Prints formatted output (like C) |

- Examples:
```java
System.out.print("Hello ");
System.out.println("World!");
System.out.printf("Price: %.2f VND%n", 12000.0);
```
🧪 Output:
```console
Hello World!
Price: 12000.00 VND
```
## 2. Standard Input
To read user input, we commonly use the Scanner class.


1. Import Scanner

```java
import java.util.Scanner;
```
2. Create Scanner object

```java
Scanner sc = new Scanner(System.in);
```
3. Use input methods

| Method          | Reads             | Example Input |
| --------------- | ----------------- | ------------- |
| `next()`        | A single word     | `hello`       |
| `nextLine()`    | Whole line        | `hello world` |
| `nextInt()`     | Integer           | `123`         |
| `nextDouble()`  | Decimal number    | `3.14`        |
| `nextBoolean()` | `true` or `false` | `true`        |

## Code demo:

```java
import java.util.Scanner;

public class StandardIOExample {
    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);

        // Input
        System.out.print("Enter your name: ");
        String name = sc.nextLine();

        System.out.print("Enter your age: ");
        int age = sc.nextInt();

        // Output
        System.out.println("Hello, " + name + "!");
        System.out.printf("You are %d years old.%n", age);
    }
}
```
## Sample Run:
```console
Enter your name: Alice
Enter your age: 22
Hello, Alice!
You are 22 years old.
```
## Close Scanner
Always close the Scanner when done:

```java
sc.close();
```

---
---
# B. File I/O – Overview
## 1. What is File I/O?
File I/O (Input/Output) allows Java programs to:
- Read data from files (Input)
- Write data to files (Output)

Java provides a powerful set of classes in the `java.io` and `java.nio` packages to handle both text and binary file operations.

## 2. File Types
| Type       | Description                             | Example          |
| ---------- | --------------------------------------- | ---------------- |
| **Text**   | Human-readable (e.g., `.txt`, `.csv`)   | `"Hello\nWorld"` |
| **Binary** | Machine-readable (e.g., `.jpg`, `.dat`) | `01001011`       |

## 3. Common Classes for File I/O
### Reading from Files:

| Class / Interface | Use Case                           |
| ----------------- | ---------------------------------- |
| `FileReader`      | Read text files (character stream) |
| `BufferedReader`  | Efficient reading of text lines    |
| `FileInputStream` | Read binary files (byte stream)    |
| `Scanner`         | Simple text file reader            |

### Writing to Files

| Class / Interface  | Use Case                            |
| ------------------ | ----------------------------------- |
| `FileWriter`       | Write text files (character stream) |
| `BufferedWriter`   | Efficient writing of text lines     |
| `FileOutputStream` | Write binary files (byte stream)    |
| `PrintWriter`      | Advanced text output to file        |


## 4. Key Concepts
### 1. Files and Paths
Java uses File and Path objects to work with file system locations.

Use File.exists() to check if a file exists.

```java
File file = new File("data.txt");
if (file.exists()) {
    System.out.println("File found!");
}
```
### 2. Streams
Java I/O is based on streams:
- Byte streams (InputStream, OutputStream) – for binary data.
- Character streams (Reader, Writer) – for text data.

### 3. Basic Flow for Reading and Writing
- Reading:
  - Open file (e.g., FileReader)
  - Read data
  - Close file

- Writing:
  - Open file (e.g., FileWriter)
  - Write data
  - Close file

> Always close streams to avoid resource leaks.

## 5. Exceptions to Handle

| Exception Type          | Reason                           |
| ----------------------- | -------------------------------- |
| `FileNotFoundException` | File does not exist              |
| `IOException`           | General I/O problem (read/write) |

## Use Cases

| Task                  | Tools                             |
| --------------------- | --------------------------------- |
| Read a config file    | `BufferedReader`, `Scanner`       |
| Write logs to file    | `BufferedWriter`, `PrintWriter`   |
| Process binary images | `FileInputStream`, `OutputStream` |
| Append text to file   | `FileWriter(file, true)`          |


## 6. How to Read/Write a Binary File?
### 1. What is a Binary File?
A binary file stores data in raw bytes (not human-readable), such as:
- Images (.jpg, .png)
- Audio (.mp3)
- Video (.mp4)
- Serialized data (.dat, .bin)

### 2. Tools for Binary File I/O
Operation	Class
Read binary	FileInputStream
Write binary	FileOutputStream

Both classes belong to java.io and operate on byte streams.

### 3. Reading a Binary File
Example: Read bytes from a file

```java
import java.io.FileInputStream;
import java.io.IOException;

public class BinaryFileReader {
    public static void main(String[] args) {
        try (FileInputStream fis = new FileInputStream("data.bin")) {
            int byteData;
            while ((byteData = fis.read()) != -1) {
                System.out.print(byteData + " ");
            }
        } catch (IOException e) {
            e.printStackTrace();
        }
    }
}
```
> **Notes:**
> - fis.read() reads one byte at a time (0–255), returns -1 at end.
> - Try-with-resources auto-closes the stream.

### 4. Writing a Binary File
Example: Write bytes to a file

```java
import java.io.FileOutputStream;
import java.io.IOException;

public class BinaryFileWriter {
    public static void main(String[] args) {
        byte[] data = {65, 66, 67, 68}; // A B C D

        try (FileOutputStream fos = new FileOutputStream("output.bin")) {
            fos.write(data);
            System.out.println("Binary data written.");
        } catch (IOException e) {
            e.printStackTrace();
        }
    }
}
```
> Notes:
> - fos.write(byte[]) writes an entire array of bytes.
> - You can also write a single byte: fos.write(65);

### 5. Copy a Binary File
```java
import java.io.FileInputStream;
import java.io.FileOutputStream;
import java.io.IOException;

public class BinaryFileCopy {
    public static void main(String[] args) {
        try (
            FileInputStream fis = new FileInputStream("source.jpg");
            FileOutputStream fos = new FileOutputStream("copy.jpg")
        ) {
            byte[] buffer = new byte[1024];
            int bytesRead;
            
            while ((bytesRead = fis.read(buffer)) != -1) {
                fos.write(buffer, 0, bytesRead);
            }

            System.out.println("File copied successfully.");
        } catch (IOException e) {
            e.printStackTrace();
        }
    }
}
```

### 6. Common Errors and Solutions

| Problem                     | Solution                                  |
| --------------------------- | ----------------------------------------- |
| File not found              | Check the file path and name              |
| File not closing            | Use `try-with-resources`                  |
| Incomplete file write       | Always flush or close the stream          |
| Writing text to binary file | Don’t use character methods like `Writer` |

## 7. How to Read/Write a Text File
### 1. What is a Text File?
A text file contains readable characters encoded as Unicode or ASCII. Examples:
- .txt
- .csv
- .json

### 2. Tools for Text File I/O

| Operation    | Class              |
| ------------ | ------------------ |
| Read binary  | `FileInputStream`  |
| Write binary | `FileOutputStream` |

### 3. Reading a Text File
- **Option 1: BufferedReader (line-by-line)**
```java
import java.io.BufferedReader;
import java.io.FileReader;
import java.io.IOException;

public class ReadTextFile {
    public static void main(String[] args) {
        try (BufferedReader reader = new BufferedReader(new FileReader("example.txt"))) {
            String line;
            while ((line = reader.readLine()) != null) {
                System.out.println(line); // Print each line
            }
        } catch (IOException e) {
            e.printStackTrace();
        }
    }
}
```
- **Option 2: Scanner (flexible parsing)**
```java
import java.io.File;
import java.io.FileNotFoundException;
import java.util.Scanner;

public class ReadWithScanner {
    public static void main(String[] args) {
        try (Scanner sc = new Scanner(new File("example.txt"))) {
            while (sc.hasNextLine()) {
                String line = sc.nextLine();
                System.out.println(line);
            }
        } catch (FileNotFoundException e) {
            e.printStackTrace();
        }
    }
}
```
### 4. Writing to a Text File
- **Option 1: BufferedWriter**
```java
import java.io.BufferedWriter;
import java.io.FileWriter;
import java.io.IOException;

public class WriteTextFile {
    public static void main(String[] args) {
        try (BufferedWriter writer = new BufferedWriter(new FileWriter("output.txt"))) {
            writer.write("Hello, world!");
            writer.newLine();
            writer.write("This is line 2.");
        } catch (IOException e) {
            e.printStackTrace();
        }
    }
}
```
- **Option 2: PrintWriter (convenient for formatted output)**
```java
import java.io.PrintWriter;
import java.io.IOException;

public class WriteWithPrintWriter {
    public static void main(String[] args) {
        try (PrintWriter writer = new PrintWriter("output.txt")) {
            writer.println("Name: John");
            writer.printf("Score: %.2f%n", 95.5);
        } catch (IOException e) {
            e.printStackTrace();
        }
    }
}
```
## 8. Append Mode
To append to a file instead of overwriting it:

```java
new FileWriter("output.txt", true); // append = true
 ```
> Notes:
> BufferedReader is more efficient for large files than Scanner.
> - Always close streams (or use try-with-resources).
> - If file doesn’t exist, Java creates it when writing.

## 8. Other IO Supported Classes
Java provides many additional classes to handle advanced or specialized I/O operations. These classes are mostly found in:
- java.io
- java.nio.file (New I/O API introduced in Java 7)


**Categories of Supported IO Classes**
| Category             | Description                          | Example Classes                                 |
| -------------------- | ------------------------------------ | ----------------------------------------------- |
| High-performance I/O | Buffered, piped, or object I/O       | `BufferedStream`, `PipedStream`, `ObjectStream` |
| Data-oriented I/O    | Read/write Java primitives           | `DataInputStream`, `DataOutputStream`           |
| Object serialization | Save/load full Java objects          | `ObjectInputStream`, `ObjectOutputStream`       |
| NIO (New IO)         | Modern file API with `Path`, `Files` | `Files`, `Paths`, `FileChannel`                 |
| In-memory I/O        | Use memory instead of disk           | `ByteArrayInputStream`, `StringReader`          |
| Inter-thread I/O     | For thread communication via pipes   | `PipedInputStream`, `PipedOutputStream`         |

### 1. DataInputStream / DataOutputStream
Used to read/write primitive data types (int, float, double, etc.) in binary form.

Example:
```java
import java.io.*;

public class DataStreamDemo {
    public static void main(String[] args) throws IOException {
        try (DataOutputStream dos = new DataOutputStream(new FileOutputStream("data.bin"))) {
            dos.writeInt(123);
            dos.writeDouble(45.67);
        }

        try (DataInputStream dis = new DataInputStream(new FileInputStream("data.bin"))) {
            int num = dis.readInt();
            double val = dis.readDouble();
            System.out.println("Read: " + num + ", " + val);
        }
    }
}
```
### 2. ObjectInputStream / ObjectOutputStream
Used for object serialization – saving/loading objects to/from files.

Requirements: Class must implement Serializable

Example:
```java
import java.io.*;

class Student implements Serializable {
    String name;
    int age;
    Student(String name, int age) {
        this.name = name; this.age = age;
    }
}

public class ObjectIOExample {
    public static void main(String[] args) throws IOException, ClassNotFoundException {
        // Write object
        try (ObjectOutputStream oos = new ObjectOutputStream(new FileOutputStream("student.dat"))) {
            Student s = new Student("Alice", 20);
            oos.writeObject(s);
        }

        // Read object
        try (ObjectInputStream ois = new ObjectInputStream(new FileInputStream("student.dat"))) {
            Student s = (Student) ois.readObject();
            System.out.println("Name: " + s.name + ", Age: " + s.age);
        }
    }
}
```
### 3. Files and Paths (NIO)
Modern way of file handling using java.nio.file.

Example: Read all lines
```java
import java.nio.file.*;
import java.io.IOException;
import java.util.List;

public class NIORead {
    public static void main(String[] args) throws IOException {
        List<String> lines = Files.readAllLines(Paths.get("example.txt"));
        lines.forEach(System.out::println);
    }
}
```
Example: Write string
```java
Files.write(Paths.get("output.txt"), "Hello NIO".getBytes());
```
### 4. ByteArrayInputStream / ByteArrayOutputStream
Used to read/write data from/to memory buffers instead of files.

Example:
```java
import java.io.*;

public class ByteArrayExample {
    public static void main(String[] args) throws IOException {
        ByteArrayOutputStream bos = new ByteArrayOutputStream();
        bos.write("Hello".getBytes());
        
        ByteArrayInputStream bis = new ByteArrayInputStream(bos.toByteArray());
        int b;
        while ((b = bis.read()) != -1) {
            System.out.print((char) b);
        }
    }
}
```
### 5. BufferedInputStream / BufferedOutputStream
Wrap around FileInputStream and FileOutputStream to improve performance with internal buffering.