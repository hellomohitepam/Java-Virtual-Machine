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

# Memory Area/class Area/ MetaSpace
> before java 8 method area use PermGen - it is a part of heap and we can not grow and shink
> now method area implementation is metaspace
## information related to the 
- class metadata - class name, fields, constant
- method metadata - it stores the byte code for static & instance method, method name ,signature, method access modifier, construtor bytes code information about reflective object such as method fields constructor also proxy  (static variables before java 8)
- anatotation metadata -
- classloader metadata 

---
where variables in the method stores
---
# Heap Area
- here all objects as well as instance, static variable stores
- Theory object of class type is created which stores the information of that class which we created

Heap Area is also divided in 3 areas 
1. Young Generation - new objects
   > Eden
   > survior
2. old generation - long lived objects 

# Stack Area
- Each thread have its own stack.
- stores methed specific variables - parameter, local variable
- immediate result also store here

# Program counter register
- keep track for currently executing instructions and next executing instructions 
- Each thread have own PC registor

# Native method Stack
- for non java methods

---

-- Are all above thing occur in compile time because here byte code is converted to machine code line by line

# Execution Engine
- Execute the byte code loaded in to the JVm
- Interpretor - here byte code is converted to machine code line by line
- JIT (Just in Time) from the Interpretor - frequently used byte code into native machine (speed), find code hot spot (which require again and again)
- JNI - to interact with native code (oprating specific feature whic coded in C)
- Native method libraries (used by JNI)
- Thread manager
























  
