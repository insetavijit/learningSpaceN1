
>[!quote] चरक संहिता (Charaka Samhita, Ayurveda, India, ~200 BCE)  
**“सिद्धिर्भवति कर्मजा।”**  
_“Success is born from consistent action.”_

---
# **[[1.0 FOUNDATIONS — PHP]] (ALL)**

1.1 **Definitions** – What is PHP? Server-side scripting language for dynamic web applications  
1.2 **Core Principles** – Variables, scope, arrays, functions, strings, OOP fundamentals  
1.3 **Mental Models** – “Interpreter that executes logic”, “template engine for servers”, “request-response processor”  
1.4 **Architecture Overview** – How PHP runs inside the web stack  
   1.4.1 **High-Level Diagram** – Browser → Request → PHP runtime → Server logic → Response/HTML  
   1.4.2 **Components & Responsibilities** – Core engine, file execution, extensions, frameworks  
   1.4.3 **Data Flow** – Input ($_GET/$_POST) → Processing → Output (HTML/JSON)

1.5 **Internals & Mechanics** – Zend Engine, opcode generation, memory model, error levels  
1.6 **Limitations & Trade-offs** – Weak typing issues, mixing logic & template, execution per request

---

# **[[2.0 APPLIED PRACTICE — PHP]] (4 Sheets)**

2.1 **Code Examples** – Syntax, variables, arrays, loops, functions, OOP  
   2.1.1 **Basic Examples** – print, echo, array operations  
   2.1.2 **Intermediate Examples** – includes, forms handling, sessions  
   2.1.3 **Advanced Examples** – namespaces, traits, dependency injection

2.2 **Hands-on Mini Projects**  
   2.2.1 **Beginner Project** – Form handler + Validation + Output response  
   2.2.2 **Intermediate Project** – CRUD app with MySQL PDO  
   2.2.3 **Production Project** – MVC structure + routing + OOP services

2.3 **Patterns & Workflows**  
   2.3.1 **Design Patterns** – MVC, Singleton, Repository, Factory, Service/Controller  
   2.3.2 **Common Workflows** – Request → sanitize → controller → view  
   2.3.3 **Anti-patterns** – SQL inside views, global variables, spaghetti code

2.4 **Tools, Tips & Debugging Notes** – Xdebug, logs, error handling, profiling  
2.5 **Real-World Use Cases** – CMS, REST APIs, dashboards, ecommerce, automation

---

# **[[3.0 QUICK REFERENCE — PHP]]**

3.1 **Cheatsheets** – Operators, string functions, array functions, OOP keywords  
3.2 **Snippets** – Connect DB, route requests, upload file  
3.3 **Templates Collection**  
   3.3.1 **Common Tasks** – routing, sanitizing, validating, sessions  
   3.3.2 **Security Blocks** – CSRF token, password hashing  
   3.3.3 **Boilerplates** – Minimal MVC skeleton

3.4 **Condensed Architecture Diagrams** – PHP lifecycle simplified  
3.5 **Error & Issue Playbook**  
   3.5.1 **Common Errors** – undefined index, header already sent  
   3.5.2 **Causes** – missing checks, whitespace output, uninitialized variables  
   3.5.3 **Fixes** – default values, buffer control, strict mode

3.6 **Best Practices**  
   3.6.1 **Do’s & Don’ts** – sanitize input, minimize globals, separate layers  
   3.6.2 **Performance Guidelines** – caching, optimized loops, prepared statements  
   3.6.3 **Security** – SQL injection prevention, XSS filtering, HTTPS only

---

# **[[4.0 ACTIVE RECALL — PHP]] (3 Sheets)**

4.1 **Flashcards (Q/A)** – variables, arrays, OOP, sessions, security ✔️  
4.2 **Quizzes**  
   4.2.1 **Beginner Quiz** – syntax, variables, loops, scope  
   4.2.2 **Intermediate Quiz** – sessions, forms, DB operations  
   4.2.3 **Expert Quiz** – OOP architecture & debugging scenarios

4.3 **Challenges & Exercises**  
4.4 **Memory Anchors**  
   4.4.1 **Analogies** – PHP runtime like a waiter: request → execute order → deliver dish  
   4.4.2 **Visual Mnemonics** – MVC triangles  
   4.4.3 **Comparison Table** – PHP vs Node.js vs Python

4.5 **Study Loop**  
Daily snippets → weekly project → 30-day refactor → 90-day mastery

---

# **[[5.0 STAYING CURRENT — PHP Ecosystem]]**

5.1 **New Features** – Enums, Fibers, Attributes, JIT engine  
5.2 **Breaking Changes** – migration from PHP 5.x → 7/8 strict typing  
5.3 **Framework Shifts** – Laravel, Symfony vs raw PHP  
5.4 **Industry Trends** – MVC → API-first → microservices  
5.5 **Monthly Upgrade Ritual**  