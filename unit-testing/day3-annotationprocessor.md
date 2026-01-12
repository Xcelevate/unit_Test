In our previous examples, we looked at how to read annotations while the program is **running** (Runtime). An `AnnotationProcessor`, however, is a tool that acts during the **compilation** phase.

Think of an `AnnotationProcessor` as a **compiler plugin**. It is a specialized piece of code that hooks into the Java Compiler () to scan your source files before they are fully turned into bytecode.

### 1. The Core Role: "Code that Writes Code"

The primary job of an `AnnotationProcessor` is to automate repetitive tasks. It can:

* **Generate New Files:** Create new `.java` or `.class` files based on your annotations (like generating a database schema from an `@Entity` class).
* **Validate Code:** Throw a **compile-time error** if an annotation is used incorrectly (e.g., ensuring a `@CourseInfo` annotation isn't placed on a private method).
* **Reduce Boilerplate:** Libraries like **Lombok** use this to generate getters, setters, and constructors so you don't have to type them.

---

### 2. How it Works: The "Round" System

Annotation processing happens in **rounds**.

1. **Round 1:** The compiler finds annotations in your source code and hands them to the Processor.
2. **Processing:** The Processor reads the metadata and might generate a new file (e.g., `GeneratedClass.java`).
3. **Next Round:** Because a new file was created, the compiler runs *another* round to see if that new file also has annotations.
4. **Completion:** This continues until no more new files are generated.

---

### 3. Comparison: Runtime vs. Compile-Time

| Feature | Reflection (Runtime) | AnnotationProcessor (Compile-Time) |
| --- | --- | --- |
| **When it runs** | While the app is running. | While you are building the app (). |
| **Performance** | Slower (requires CPU at startup). | Faster (zero overhead at runtime). |
| **Capabilities** | Inspects and invokes methods. | Generates new source/resource files. |
| **Visibility** | Requires `RUNTIME` retention. | Works with `SOURCE` or `CLASS` retention. |

---

### 4. Simple Concept Example

Imagine you are building a college grading system. You could write a processor that looks for a `@RequiredField` annotation on a class. If a student forgets to add a `studentID` field to that class, the **compiler itself will stop** and tell them: *"Error: Class 'HistoryStudent' is missing a studentID!"*

```java
// What the student writes
@ValidateModel
public class StudentRecord {
    // If this is empty, the AnnotationProcessor triggers a COMPILER error
}

```

By using an `AnnotationProcessor`, you move the "discovery" of errors from the user's screen (Runtime) to the developer's screen (Compile-time), which is much safer and more efficient.

Would you like to see a code snippet of how a `Processor` class is actually structured using the `AbstractProcessor` API?

[Annotation Processing in Java](https://www.youtube.com/watch?v=ja4is9oq37k)
This video provides a practical walkthrough of creating a custom annotation processor, which helps visualize the "code that writes code" concept.