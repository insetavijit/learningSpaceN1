## **[[1.1 Definitions & Keywords — PHP]]**

```
PHP (PHP: Hypertext Preprocessor) · Server-Side Scripting Language ·General-Purpose Programming Language · Interpreted Language ·Dynamic Typing ·Weak Typing ·Imperative Programming ·Object-Oriented Programming ·Procedural Programming ·Server-Side Execution ·Request–Response Model ·Embedded Scripting ·PHP Runtime Engine · Zend Engine ·Execution Context ·Superglobals · $_GET · $_POST · $_REQUEST · $_SESSION · $_COOKIE · $_SERVER ·Stateless Execution Model ·File-Based Scripts ·
Standard Library · Built-in Functions ·Extensions ·Composer · Dependency Management ·Namespaces · Autoloading (PSR-4) ·Error Handling · Exceptions ·Output Buffering ·PHP-FPM · Web Server Integration (Apache / Nginx)
```

## **[[1.2 Core Principles — PHP]]**

```
1. Server-Side Execution — Code runs on the server and outputs HTML or other responses
2. Request-Scoped Lifecycle — Script execution is bound to a single HTTP request
3. Dynamic and Weak Typing — Types resolved at runtime with implicit coercion
4. Multi-Paradigm Support — Procedural, object-oriented, and functional styles supported
5. Embedded HTML Integration — PHP can be interleaved directly within HTML
6. Stateless by Default — Persistence achieved via sessions, storage, or external services
7. Shared-Nothing Architecture — Each request isolated from others
8. Standard Library Richness — Extensive built-in functionality for common tasks
9. Extension-Based Capabilities — Core functionality extended via modules
10. Backward-Compatible Evolution — Legacy code preserved across versions
```

## **[[1.3 Mental Models — PHP]]**

```
1. Request–Response Script Model — Each request triggers a fresh execution
2. Template Processing Model — PHP generates text (HTML, JSON) as output
3. Runtime Environment Model — Code executes within a server-managed runtime
4. Object Lifecycle Model — Objects exist only for the duration of a request
```

## **[[1.4 Architecture Overview — PHP]]**

### **High-Level Diagram**

```
HTTP Request →
Web Server →
PHP Runtime (Zend Engine) →
Script Execution →
Output Generation →
HTTP Response Sent →
Process Ends
```

### **[[1.4.2 Components & Responsibilities — PHP]]**

```
1. PHP Runtime Engine — Parses, compiles, and executes PHP code
2. Zend Engine — Core execution engine managing memory and opcode execution
3. Script Files — Contain executable PHP and embedded output
4. Superglobals — Provide access to request, session, and server data
5. Standard Library — Built-in functions and core utilities
6. Extensions — Add database, networking, and system capabilities
7. Output Buffer — Controls response generation and manipulation
8. Web Server Interface — Connects PHP with HTTP servers
```

### **[[1.4.3 Data / Execution Flow — PHP]]**

```
Client Request →
Web Server Routes Request →
PHP Script Loaded →
Parsing and Opcode Compilation →
Execution Context Created →
Business Logic Runs →
Output Buffered →
Response Returned →
Execution Context Destroyed
```

## **[[1.5 Internals & Mechanics — PHP]]**

1. **Lexical Parsing and Opcode Generation** — Source code compiled into executable opcodes
    
2. **Zend Virtual Machine Execution** — Opcodes executed sequentially within the runtime
    
3. **Memory Management and Garbage Collection** — Reference counting and cycle detection
    
4. **Variable Handling and Symbol Tables** — Runtime resolution of variable scope
    
5. **Superglobal Population** — Automatic hydration of request-related variables
    
6. **Session Handling Mechanism** — Server-managed persistence across requests
    
7. **Error Handling and Reporting** — Runtime warnings, notices, and exceptions
    
8. **Extension Loading Model** — Modular capability injection at startup or runtime
## **[[1.6 Limitations & Trade-offs — PHP]]**

|Limitation|Impact / Trade-off|
|---|---|
|**Request-Bound Execution**|No long-lived application state by default|
|**Weak Typing Semantics**|Increased risk of subtle runtime errors|
|**Shared Hosting Constraints**|Limited control over runtime configuration|
|**Performance Variability**|Dependent on opcode caching and configuration|
|**Concurrency Handling**|Relies on server/process model, not language primitives|
|**Historical Inconsistencies**|Legacy design decisions affect modern usage|
|**Security Responsibility**|Input validation and sanitization are developer-managed|
## 🎓 **Micro-Conclusion (Inline Insight)**

> Section 1 defines PHP as a **request-scoped, server-side execution environment** optimized for **dynamic content generation** within the HTTP lifecycle.  
> Its architecture emphasizes simplicity, extensibility, and integration with web servers, while trading off long-lived state and intrinsic concurrency control.

---
