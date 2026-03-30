# JVM (Java Virtual Machine) 
- The JVM is an abstract computing machine that enables a computer to run Java programs

```
Chrome → asks OS → OS talks to real CPU
```

## It has its own:
- 📋 Instruction set (bytecode) — like a CPU has instructions
- 🧠 Memory model — like a machine manages memory
- 📦 Stack & Registers — like real hardware has
```
Java Program → talks to JVM (fake machine)
                    ↓
              JVM talks to real OS + CPU
```

---

# JVM Architecture Overview

1. Class Loader
2. Memory Area
3. Execution Engine

```
┌─────────────────────────────────────────────┐
│                  JVM                        │
│  ┌──────────────────────────────────────┐   │
│  │        Class Loader Subsystem        │   │
│  └──────────────────────────────────────┘   │
│  ┌──────────────────────────────────────┐   │
│  │          Runtime Data Areas          │   │
│  │  Method Area │ Heap │ Stack │ PC Reg │   │
│  └──────────────────────────────────────┘   │
│  ┌──────────────────────────────────────┐   │
│  │        Execution Engine              │   │
│  │  Interpreter │ JIT Compiler │ GC     │   │
│  └──────────────────────────────────────┘   │
│  ┌──────────────────────────────────────┐   │
│  │       Native Interface (JNI)         │   │
│  └──────────────────────────────────────┘   │
└─────────────────────────────────────────────┘
```




# Class Loader
- Responsible for loading, linking, and initializing classes at runtime.

1. BootStrap ClassLoader : responsible for loading core java classes
   - it has highest priority because Java CANNOT run without them.
   - implemented in native code
   - import java.base module after java 9 and before rt.jar

2. plateform classLoader(Extension Class Loader):
   - Java CAN run without them.
   - Load plateform specific modules
   - **`javax.*`** (the x means extension!)
   - java.sql, javax.crypto.*   → encryption, javax.xml.*   → XML parsing
   - lib/ext backward compatebility

3. Application classes
   - Every class you write + every library you download (Spring, Hibernate, Gson, etc.) is loaded by Application ClassLoader.

## Delegation Model:
- A class loader first delegates to its parent before trying to load itself. This prevents rogue classes from overriding core Java classes.

class Loader also have Linking step
1. Verification : is this byte code is good
2. Preparation : intitialization of static variable with default value
3. Resolution : variable to actual refernce

Initialization by ACTUAL VALUE

## Quick Picture
```
Bootstrap loads:      String, Object, Integer, System
                      ↑ JVM cannot start without these

Extension loads:      Cipher, SSLSocket, DocumentBuilder
                      ↑ Extra JDK features, pre-installed

Application loads:    BankAccount, SpringApplication, Gson
                      ↑ Your code + libraries you added
```

# Java Memory: PermGen vs Metaspace

## 🧠 Overview

In Java, the **Method Area** is a part of the JVM memory specification used to store:

*  class metadata - class name, fields, constant
*  method metadata - it stores the byte code for static & instance method, method name ,signature, method access modifier, construtor bytes code information about reflective object such as method fields constructor also proxy  (static variables before java 8)
*  anatotation metadata -
*  classloader metadata 
* Static variables
* Runtime constant pool

The implementation of the Method Area has changed over Java versions:

* **Before Java 8 → PermGen (Permanent Generation)**
* **Java 8 and later → Metaspace**

---

## 🔹 Before Java 8: PermGen

### 📌 Key Characteristics

* Part of **Heap memory**
* Stores:

  * Class metadata
  * Static variables
  * Interned strings (partially)

### ⚠️ Limitations

* Fixed size (configured using `-XX:MaxPermSize`)
* Cannot grow dynamically
* Prone to memory issues

### ❌ Common Error

```
java.lang.OutOfMemoryError: PermGen space
```

---

## 🚀 Java 8 and Later: Metaspace

### 📌 Key Characteristics

* Stored in **Native Memory (outside heap)**
* Automatically grows as needed
* More flexible memory management

### ⚙️ Configuration

* `-XX:MaxMetaspaceSize` (optional limit)

### ❌ Possible Error

```
java.lang.OutOfMemoryError: Metaspace
```

---

## 🔁 Comparison Table

| Feature          | PermGen (Before Java 8) | Metaspace (Java 8+) |
| ---------------- | ----------------------- | ------------------- |
| Memory Location  | Heap                    | Native Memory       |
| Size             | Fixed                   | Dynamic             |
| Tuning Parameter | `MaxPermSize`           | `MaxMetaspaceSize`  |
| OOM Error        | PermGen space           | Metaspace           |

---


---

## 🧠 Interview One-Liner

> Before Java 8, Method Area was implemented using PermGen (heap-based, fixed size). From Java 8 onward, it is implemented using Metaspace, which resides in native memory and grows dynamically.

---


---

# 🧩 Heap Area

## 📌 What is Heap?

Heap is the memory area where:

* All **objects** are stored
* **Instance variables** are stored
* **Static variables** are stored (in method area/metaspace logically, but associated with class in heap lifecycle)

> When we create an object, memory is allocated in the heap.

```java
Person p = new Person();
```

* `p` → stored in stack (reference)
* Object → stored in heap

---

## 🧠 Important Concept

When a class is loaded:

* A **class metadata structure** is created (in Method Area / Metaspace)
* Objects of that class are created in **Heap**

---

## 🔁 Heap Division

Heap is divided into generations for efficient garbage collection:

### 1️⃣ Young Generation (New Objects)

* Where newly created objects go

#### Sub-parts:

* **Eden Space** → new objects created here
* **Survivor Space (S0, S1)** → objects that survive GC

---

### 2️⃣ Old Generation (Tenured)

* Stores **long-lived objects**
* Objects promoted here after surviving multiple GC cycles

---

## ⚡ Simple Flow

1. Object created → Eden
2. Survives GC → Survivor
3. Survives multiple times → Old Generation

---

## 🧠 Interview One-Liner

> Local variables are stored in stack memory, while objects and instance variables are stored in heap memory. The heap is divided into young and old generations for efficient garbage collection.

---

## ✅ Summary

* Stack → method variables & references
* Heap → objects & instance data
* Young Gen → new objects
* Old Gen → long-lived objects

# JVM Memory: Stack Area & Related Components

## 🧩 Stack Area

### 📌 What is Stack Memory?

- Each **thread has its own stack**
- Used to store **method-specific data**

### 🧠 What is stored in Stack?

- Method parameters
- Local variables
- Object references (not actual objects)
- Intermediate / immediate results of calculations

### 📌 Example

```java
void test() {
    int a = 10;              // stored in stack
    int b = 20;              // stored in stack
    int c = a + b;           // intermediate result stored in stack
}
```
# ⚡ Key Points
- Created when a thread starts
- Destroyed when thread ends
- Works in LIFO (Last In First Out) manner
- Each method call creates a stack frame

# 🧭 Program Counter (PC Register)
## 📌 What is PC Register?
- Keeps track of:
1. Current executing instruction
2. Next instruction to execute
# ⚡ Key Points
- Each thread has its own PC Register
- Helps JVM manage execution flow
- Essential for multi-threading

# ⚙️ Native Method Stack
## 📌 What is Native Method Stack?
- Used for executing non-Java methods
- Handles code written in languages like: C, C++
# ⚡ Key Points
- Separate from Java stack
- Used when JVM interacts with native libraries (JNI)

---

-- Are all above thing occur in compile time because here byte code is converted to machine code line by line

# JVM Execution Engine

## 🧩 What is Execution Engine?

- The **Execution Engine** is a part of JVM responsible for:
  - Executing the **bytecode** loaded into the JVM
  - Converting bytecode into **machine code**

---

## ⚙️ Components of Execution Engine

---

## 🔹 1. Interpreter

### 📌 What it does

- Converts **bytecode → machine code line by line**
- Executes instructions one at a time

### ⚡ Pros

- Simple and fast to start execution

### ❌ Cons

- Slower for repeated method calls (same code executed again and again)

---

## 🚀 2. JIT (Just-In-Time Compiler)

### 📌 What it does

- Works along with the interpreter
- Identifies **frequently used code (hotspots)**
- Converts that bytecode into **native machine code once**
- Reuses compiled code for better performance

### ⚡ Key Points

- Improves execution speed
- Avoids repeated interpretation
- Stores optimized native code

---

## 🔗 3. JNI (Java Native Interface)

### 📌 What it does

- Allows Java programs to interact with **native (non-Java) code**
- Used to call code written in:
  - C
  - C++

### ⚡ Use Case

- Access OS-level features
- Use platform-specific libraries

---

## 📚 4. Native Method Libraries

### 📌 What it is

- Collection of **native code libraries**
- Used by JNI

### ⚡ Example

- System-level operations
- Hardware interaction

---

## 🧵 5. Thread Manager

### 📌 What it does

- Manages execution of multiple threads
- Handles:
  - Thread scheduling
  - Synchronization
  - Memory allocation per thread

---

## 🧠 Execution Flow (Simple)

1. Bytecode loaded into JVM  
2. Interpreter starts execution  
3. JIT identifies hotspots  
4. Converts them to native code  
5. JNI used if native code required  

---

## 🧠 Interview One-Liner

> The Execution Engine executes bytecode using an interpreter and JIT compiler, supports native calls via JNI, and manages threads for efficient program execution.

---

## ✅ Summary

- Interpreter → line-by-line execution  
- JIT → optimizes frequently used code  
- JNI → connects Java with native code  
- Native Libraries → support JNI  
- Thread Manager → handles multithreading  
























  
