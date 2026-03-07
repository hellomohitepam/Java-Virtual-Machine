## 2 Types of Memory in RAM
- **Stack**
- **Heap**

**Note:**  
- Both **Stack** and **Heap** are created by the **JVM** and stored in **RAM**.
- Generally Heap has more memory then Stack

## Stack Memory

- Stores **temporary variables** and provides a **separate memory block for methods**.
- Stores **primitive data types**.
- Stores **references to heap objects**:
  - Strong reference - when GC run do not delete it.
  - Weak reference - when GC run just delete.
  - Soft reference - you are allowed to delete but do it when it is very very urgent

- Each **thread has its own stack memory**.
- Variables within a **scope** are only visible within that scope.  
  When a variable goes **out of scope**, it is **deleted from the stack** (in **LIFO order**).

- When stack memory becomes full, it throws:  
  `java.lang.StackOverflowError`

> String literal store the data into string pool which is in Heap only

## Heap Memory

- Stores **objects**.
- There is **no specific order** for allocating memory.
- **Garbage Collector (GC)** is used to delete **unreferenced objects** from the heap.
  - **Mark and Sweep Algorithm**

- **Types of GC:**
  - Single GC
  - Parallel GC
  - CMS (Concurrent Mark Sweep)
  - G1

- **Heap memory is shared among all threads**.
- Heap also contains the **String Pool**.

- When heap memory becomes full, it throws:  
  `java.lang.OutOfMemoryError`

- Heap memory is further divided into:

  - **Young Generation** (minor GC happens here)
    - Eden
    - Survivor

  - **Old / Tenured Generation** (major GC happens here)

  - **Permanent Generation** it is a part of Heap & it is not expendable (out of memory error)

# Heap internals
## Heap is divided into parts 
- Young Generation
  - Eden
  - s0 (Surviour Space0)
  - s1 (Surviour Space1)
- Old Generation

## Young Generation
- When ever a new object is created it create memory in the Eden space
- Mark & Sweap Algo
- So this algo mark all the object which is no more refernced and it is allowed to delete
- then swap firstly delete unreachable objects and sweap that object to s0 or s1 and age is add with start of 1
- This process is called minor GC it is very fast and happen very periodiclly
- Threshold age value 

## Old Generation
- Here major GC runs
- It runs very less periodically than Old GC
- Here we have kind of big objects
- 

# Non Heap (MetaSpace) - it is expendable as we require
- class variables
- class metadata
- stores constant (static final)

# Garbag collection Algorithm and its type
- mark and swep
- make and swep with compact memory - like making used memory comes together so that holes do not make problem 


# version of GC
- serial GC - only one tread works, which is slow
- Parallel Gc (default version in Java 8)
- Concurrent Mark & Sweap (java17) - GC tries to not stop the Application thread here but no compaction happens here
- G1 GC (throughput inc and latency dec)

## GC is very Expensive
- when GC work starts all application thread paused  (Stop the world).


















































  
