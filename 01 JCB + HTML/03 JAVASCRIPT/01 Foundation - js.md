## **[[1.1 Definitions & Keywords — JavaScript]]**

```
JavaScript · ECMAScript · Programming Language ·
Interpreted / JIT-Compiled Language ·
High-Level Language ·
Dynamic Typing ·
Prototype-Based Object Model ·
First-Class Functions ·
Lexical Scope · Closures ·
Execution Context · Call Stack ·
Single-Threaded Execution Model ·
Event Loop · Task Queue · Microtask Queue ·
Asynchronous Programming · Promises · async / await ·
Callbacks ·
Hoisting ·
Garbage Collection ·
Global Object ·
Strict Mode ·
Standard Library · Web APIs ·
Runtime Environment · Browser Runtime · Node.js Runtime ·
Script Loading · Module System (ES Modules)
```

---

## **[[1.2 Core Principles — JavaScript]]**

```
1. Imperative and Declarative Hybrid — Supports procedural, functional, and object-oriented styles
2. Single-Threaded Execution — One call stack with asynchronous concurrency via the event loop
3. Non-Blocking I/O — Long-running operations delegated to host environment APIs
4. Lexical Scoping — Variable scope determined by source code structure
5. Prototype-Based Inheritance — Objects inherit directly from other objects
6. First-Class Functions — Functions treated as values and composable units
7. Dynamic Typing — Types associated with values, not variables
8. Garbage-Collected Memory — Automatic memory management
9. Host-Dependent Capabilities — Behavior extended by browser or server runtimes
10. Standardized Evolution — ECMAScript governs language specification independently of hosts
```

---

## **[[1.3 Mental Models — JavaScript]]**

```
1. Execution Stack Model — Code executes frame-by-frame on a single call stack
2. Event Loop Model — Tasks queued and executed asynchronously
3. Scope Chain Model — Identifier resolution through nested lexical environments
4. Object Delegation Model — Behavior shared through prototypes, not classes
```

---

## **[[1.4 Architecture Overview — JavaScript]]**

### **High-Level Diagram**

```
Source Code →
Parsing →
Execution Context Creation →
Call Stack Execution →
Async Tasks Registered →
Event Loop Schedules Callbacks →
DOM / State Updates →
Observable Program Behavior
```

---

### **[[1.4.2 Components & Responsibilities — JavaScript]]**

```
1. JavaScript Engine — Parses, compiles, and executes code
2. Execution Contexts — Manage scope, variables, and `this` binding
3. Call Stack — Tracks active function invocations
4. Heap — Stores objects and function closures
5. Event Loop — Coordinates asynchronous execution
6. Task Queues — Manage macro- and microtasks
7. Standard Library — Provides built-in objects and functions
8. Host APIs — Extend JS with environment-specific capabilities
```

---

### **[[1.4.3 Data / Execution Flow — JavaScript]]**

```
Script Loaded →
Execution Context Initialized →
Synchronous Code Executes →
Async Operations Delegated →
Callbacks / Promises Queued →
Event Loop Dispatches →
Stack Executes Handlers →
Program State Updates
```

---

## **[[1.5 Internals & Mechanics — JavaScript]]**

1. **Parsing and AST Generation** — Source code transformed into abstract syntax trees
    
2. **Just-In-Time Compilation** — Hot code optimized during execution
    
3. **Execution Context Lifecycle** — Creation, execution, and destruction of contexts
    
4. **Closure Formation** — Functions retain access to lexical environments
    
5. **Prototype Chain Resolution** — Property lookup through object delegation
    
6. **Event Loop Scheduling** — Prioritization of microtasks over macrotasks
    
7. **Memory Allocation and Garbage Collection** — Automatic reclamation of unused objects
    
8. **Module Resolution and Loading** — Dependency graph construction and evaluation order
    

---

## **[[1.6 Limitations & Trade-offs — JavaScript]]**

|Limitation|Impact / Trade-off|
|---|---|
|**Single-Threaded Execution**|CPU-bound tasks can block the main thread|
|**Dynamic Typing**|Increased risk of runtime type errors|
|**Loose Language Semantics**|Historical quirks and edge cases|
|**Async Complexity**|Requires careful control flow management|
|**Host-Dependent APIs**|Behavior varies between environments|
|**Security Restrictions**|Sandbox limits direct system access|
|**Performance Variability**|Dependent on engine optimizations|

---

## 🎓 **Micro-Conclusion (Inline Insight)**

> Section 1 positions JavaScript as a **single-threaded, event-driven programming language** whose expressive power derives from **functions, closures, and asynchronous execution**.  
> Its architecture enables responsive, interactive applications while imposing structural discipline on concurrency and state management.

---

If you want, I can now generate the **same-grade document** for:

- Browser Rendering Engine
    
- Web APIs
    
- Node.js Runtime
    
- React / Vue / Angular
    

State the next subject only.