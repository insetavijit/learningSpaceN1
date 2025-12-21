> [!quote] Rasmus Lerdorf (Creator of PHP, Greenland, 1968)  
> **"I don't know how to stop it, there was never any intent to write a programming language [...] I have absolutely no idea how to write a programming language, I just kept adding the next logical step on the way."**  
> _PHP: the pragmatic language that powers the web._

## **1.1 Language Fundamentals & Core Syntax**

### **Learning Intent (~95–105 words)**

This section builds a rigorous mental model of PHP as a server-side scripting language with dynamic typing and flexible syntax. Learners master variables, data types, operators, control flow, and the distinction between superglobals and local scope. The objective is not merely to "write PHP," but to **understand PHP's execution model and web context**—so every script interacts predictably with HTTP requests, forms, and server environments. Strong grounding here prevents security vulnerabilities, establishes clear mental models for type juggling and coercion, and provides the foundation for functions, OOP, and database interactions that define modern PHP development.

|Topic|Focus & Purpose|
|---|---|
|**[[1.1.1 PHP Basics & Script Structure]]**|PHP tags (`<?php ?>`); embedding PHP in HTML; script execution model; server-side vs client-side; file extensions (`.php`); basic output (`echo`, `print`). Establishes how PHP generates dynamic content.|
|**[[1.1.2 Variables & Data Types]]**|Variable declaration (`$variable`); eight primitive data types (integer, float, string, boolean, array, object, null, resource); type juggling; dynamic typing; `gettype()`, `settype()`. Defines PHP's flexible type system.|
|**[[1.1.3 Constants & Superglobals]]**|`define()`, `const`; magic constants (`__FILE__`, `__LINE__`, `__DIR__`); superglobal arrays (`$_GET`, `$_POST`, `$_REQUEST`, `$_SERVER`, `$_SESSION`, `$_COOKIE`, `$_FILES`, `$_ENV`, `$GLOBALS`). Critical for request handling.|
|**[[1.1.4 Operators & Expressions]]**|Arithmetic, comparison, logical, assignment operators; string concatenation (`.`); ternary operator; null coalescing (`??`); nullsafe operator (`?->`); spaceship operator (`<=>`); operator precedence. Defines value manipulation.|
|**[[1.1.5 Control Structures]]**|`if`/`else`/`elseif`, `switch`; `match` expression (PHP 8+); short-circuit evaluation; guard clauses. Encodes conditional logic.|
|**[[1.1.6 Loops & Iteration]]**|`for`, `while`, `do...while`, `foreach`; `break`, `continue`; iterating arrays and objects. Defines repetition patterns.|
|**[[1.1.7 Type Juggling & Comparisons]]**|Automatic type conversion; loose vs strict comparison (`==` vs `===`); truthy/falsy values; type coercion rules; avoiding juggling pitfalls. Critical for predictable comparisons.|

---

## **1.2 Functions & Code Organization**

### **Learning Intent (~85–95 words)**

Functions are PHP's primary unit of code reuse. This section teaches function declaration, parameters, return values, scope rules, and PHP's unique variable handling with pass-by-value vs pass-by-reference. Learners master function arguments, default parameters, variadic functions, and anonymous functions (closures). Understanding scope, static variables, and the `global` keyword is critical for managing state. The goal is to write functions that are predictable, testable, and reusable while avoiding common pitfalls like scope pollution and unintended side effects.

|Topic|Focus & Purpose|
|---|---|
|**[[1.2.1 Function Declaration & Invocation]]**|Defining functions; function naming conventions; calling functions; return values; `return` statement; void functions. Establishes code reuse patterns.|
|**[[1.2.2 Parameters & Arguments]]**|Required vs optional parameters; default values; type declarations (scalar, return, union types in PHP 8+); variadic functions (`...$args`); named arguments (PHP 8+). Defines input flexibility.|
|**[[1.2.3 Scope & Variable Handling]]**|Local vs global scope; `global` keyword; superglobals access; static variables in functions; variable scope lifetime. Critical for state management.|
|**[[1.2.4 Pass-by-Value vs Pass-by-Reference]]**|Default pass-by-value behavior; pass-by-reference with `&`; when to use each; memory implications; avoiding unintended mutations. Defines parameter passing semantics.|
|**[[1.2.5 Anonymous Functions & Closures]]**|Anonymous function syntax; closures with `use` keyword; callbacks; `Closure` class; arrow functions (PHP 7.4+); first-class callable syntax (PHP 8.1+). Enables functional patterns.|
|**[[1.2.6 Built-in Functions & Extensions]]**|Common PHP functions (string, array, math, date/time); function reference; understanding extensions (Core, Standard, optional). Establishes PHP's rich standard library.|

---

## **1.3 Arrays & Data Structures**

### **Learning Intent (~90–100 words)**

Arrays are PHP's most versatile data structure, serving as both indexed lists and associative maps (hashtables). This section teaches array creation, manipulation, iteration, and the powerful array functions that enable functional programming patterns. Learners master indexed vs associative arrays, multidimensional arrays, and when to use arrays vs objects. Understanding array functions like `array_map`, `array_filter`, `array_reduce` enables declarative data transformation. The goal is to leverage arrays as PHP's primary collection type while recognizing their flexibility as both sequences and dictionaries.

|Topic|Focus & Purpose|
|---|---|
|**[[1.3.1 Array Creation & Types]]**|Array literals; `array()` vs `[]` syntax; indexed arrays (numeric keys); associative arrays (string keys); mixed arrays; empty arrays; array initialization patterns. Establishes PHP's flexible array model.|
|**[[1.3.2 Array Access & Manipulation]]**|Accessing elements; adding elements (`[]`, `array_push`, `array_unshift`); removing elements (`unset`, `array_pop`, `array_shift`, `array_splice`); modifying values. Defines array mutation operations.|
|**[[1.3.3 Array Iteration]]**|`foreach` loops (value, key-value); `for` loops; array pointers (`current`, `next`, `prev`, `reset`); iterating nested arrays. Enables array traversal.|
|**[[1.3.4 Array Functions]]**|`array_map`, `array_filter`, `array_reduce`; `array_merge`, `array_combine`; `in_array`, `array_search`; `array_keys`, `array_values`; `sort`, `asort`, `ksort`, `usort`; `count`, `sizeof`. Critical for array processing.|
|**[[1.3.5 Multidimensional Arrays]]**|Creating nested arrays; accessing nested elements; iterating multidimensional arrays; flattening arrays; practical use cases (tables, trees). Defines complex data structures.|
|**[[1.3.6 Array vs Object Trade-offs]]**|When to use arrays vs objects; performance considerations; type safety; serialization; arrays as data transfer objects (DTOs). Clarifies collection design choices.|

---

## **1.4 Object-Oriented Programming**

### **Learning Intent (~95–105 words)**

PHP's OOP features evolved significantly from PHP 5 through PHP 8, bringing modern class-based programming to the language. This section teaches class definition, properties, methods, inheritance, interfaces, traits, and PHP 8's constructor property promotion. Learners understand visibility modifiers, static members, abstract classes, and when to use interfaces vs traits. Mastery here means writing maintainable object-oriented code that leverages encapsulation, inheritance, and polymorphism while avoiding common anti-patterns like god classes or deep inheritance hierarchies. Modern PHP development heavily relies on OOP, especially in frameworks like Laravel.

|Topic|Focus & Purpose|
|---|---|
|**[[1.4.1 Classes & Objects]]**|Class declaration; instantiation with `new`; properties and methods; `$this` keyword; constructors (`__construct`); destructors (`__destruct`); object lifecycle. Establishes OOP fundamentals.|
|**[[1.4.2 Visibility & Encapsulation]]**|`public`, `private`, `protected` modifiers; property and method visibility; asymmetric visibility (PHP 8.4); getters/setters; encapsulation principles. Defines access control.|
|**[[1.4.3 Inheritance & Polymorphism]]**|Extending classes (`extends`); overriding methods; parent class access (`parent::`); `final` keyword; abstract classes; polymorphism through inheritance. Enables code reuse through hierarchy.|
|**[[1.4.4 Interfaces & Contracts]]**|Interface declaration (`interface`); implementing interfaces (`implements`); multiple interface implementation; interface inheritance; type hinting with interfaces. Defines behavioral contracts.|
|**[[1.4.5 Traits & Code Composition]]**|Trait declaration (`trait`); using traits (`use`); trait conflict resolution (`insteadof`, `as`); trait methods and properties; composition over inheritance. Enables horizontal code reuse.|
|**[[1.4.6 Static Members & Late Static Binding]]**|Static properties and methods; `self::` vs `static::`; late static binding; static constructors; singleton pattern; when to use static. Defines class-level members.|
|**[[1.4.7 Magic Methods]]**|`__get`, `__set`, `__call`, `__toString`, `__invoke`, `__clone`, `__sleep`, `__wakeup`, `__serialize`, `__unserialize`; when to use magic methods; performance implications. Enables dynamic behavior.|
|**[[1.4.8 Modern PHP OOP Features]]**|Constructor property promotion (PHP 8); readonly properties (PHP 8.1); readonly classes (PHP 8.2); property hooks (PHP 8.4); enumerations (PHP 8.1); attributes/annotations (PHP 8). Leverages PHP 8+ enhancements.|

---

## **1.5 Error Handling & Debugging**

### **Learning Intent (~80–90 words)**

Robust PHP applications require systematic error handling and debugging strategies. This section teaches PHP's error levels, exceptions, try-catch blocks, custom exception classes, and error reporting configuration. Learners master the distinction between errors and exceptions, understand error suppression trade-offs, and use debugging tools effectively. The goal is to write defensive code that fails gracefully, provides meaningful error messages, logs appropriately, and enables rapid debugging through proper error handling and development tools.

|Topic|Focus & Purpose|
|---|---|
|**[[1.5.1 Error Types & Levels]]**|Error types (Parse, Fatal, Warning, Notice, Deprecated); error reporting (`error_reporting()`, `ini_set()`); `display_errors` vs `log_errors`; production vs development settings. Defines error taxonomy.|
|**[[1.5.2 Exceptions & Try-Catch]]**|`Exception` class; `try`, `catch`, `finally` blocks; throwing exceptions (`throw`); exception hierarchy; multiple catch blocks; rethrowing exceptions. Establishes exception handling patterns.|
|**[[1.5.3 Custom Exceptions]]**|Creating exception classes; extending `Exception`; exception properties (`getMessage()`, `getCode()`, `getFile()`, `getLine()`, `getTrace()`); domain-specific exceptions. Enables semantic error handling.|
|**[[1.5.4 Error Suppression & Handlers]]**|Error suppression operator (`@`); when suppression is acceptable; `set_error_handler()`; `set_exception_handler()`; `register_shutdown_function()`; converting errors to exceptions. Defines error management strategies.|
|**[[1.5.5 Debugging Techniques]]**|`var_dump()`, `print_r()`, `debug_backtrace()`; Xdebug extension; breakpoints; step debugging; logging (`error_log()`); debugging tools (PHPStorm, VS Code). Enables systematic bug isolation.|

---

## **1.6 Working with Strings**

### **Learning Intent (~75–85 words)**

String manipulation is fundamental to web development. This section teaches PHP's extensive string functions, regular expressions, encoding handling, and string formatting. Learners master string creation, concatenation, searching, replacing, parsing, and validation using both built-in functions and PCRE regex. Understanding character encoding (UTF-8) and escaping is critical for security and internationalization. The goal is to process text data effectively while avoiding common pitfalls like SQL injection and XSS through proper string handling.

|Topic|Focus & Purpose|
|---|---|
|**[[1.6.1 String Basics & Manipulation]]**|String literals (single quotes, double quotes); heredoc, nowdoc; concatenation (`.`); string length (`strlen`, `mb_strlen`); substrings (`substr`, `mb_substr`); trimming (`trim`, `ltrim`, `rtrim`). Establishes string handling.|
|**[[1.6.2 String Searching & Replacing]]**|`strpos`, `stripos`, `strrpos`; `str_contains`, `str_starts_with`, `str_ends_with` (PHP 8); `str_replace`, `str_ireplace`; `substr_replace`; case conversion. Defines string operations.|
|**[[1.6.3 String Parsing & Formatting]]**|`explode`, `implode`; `sprintf`, `printf`; `number_format`; `parse_str`; `str_split`; CSV parsing. Enables string transformation.|
|**[[1.6.4 Regular Expressions (PCRE)]]**|PCRE syntax; `preg_match`, `preg_match_all`, `preg_replace`, `preg_split`; pattern modifiers; capture groups; regex performance considerations. Critical for pattern matching.|
|**[[1.6.5 Character Encoding & Multibyte Strings]]**|UTF-8 handling; `mb_*` functions; character set conversions; `htmlspecialchars`, `htmlentities`; escaping for security; internationalization concerns. Ensures proper text processing.|

---

## **1.7 File System & I/O Operations**

### **Learning Intent (~80–90 words)**

Server-side PHP requires robust file and directory handling. This section teaches reading, writing, uploading, and managing files and directories. Learners master file permissions, path operations, file locking, and stream handling. Understanding file upload security, temporary files, and proper resource cleanup is critical for production applications. The goal is to perform I/O operations safely and efficiently while respecting file system permissions, handling errors gracefully, and preventing common vulnerabilities like directory traversal attacks.

|Topic|Focus & Purpose|
|---|---|
|**[[1.7.1 Reading & Writing Files]]**|`file_get_contents`, `file_put_contents`; `fopen`, `fread`, `fwrite`, `fclose`; reading line-by-line (`fgets`, `file`); file pointers; binary mode. Establishes file operations.|
|**[[1.7.2 File Information & Permissions]]**|`file_exists`, `is_file`, `is_dir`, `is_readable`, `is_writable`; `filesize`, `filemtime`; `chmod`, `chown`; permission modes (Unix). Defines file metadata access.|
|**[[1.7.3 Directory Operations]]**|`opendir`, `readdir`, `closedir`; `scandir`; `mkdir`, `rmdir`; recursive directory iteration; `DirectoryIterator`, `RecursiveDirectoryIterator`. Enables directory traversal.|
|**[[1.7.4 File Uploads]]**|`$_FILES` superglobal; upload handling; `move_uploaded_file`; validation (file type, size); security considerations (MIME type checking, extension validation); temporary files. Critical for user file handling.|
|**[[1.7.5 Streams & Contexts]]**|Stream wrappers (`file://`, `http://`, `https://`, `php://`); stream contexts; stream filters; `php://input`, `php://output`; remote file access. Enables advanced I/O patterns.|

---

## **1.8 Database Interaction (MySQL/PDO)**

### **Learning Intent (~95–105 words)**

Modern PHP applications require robust database interaction. This section teaches PDO (PHP Data Objects) as the standard database abstraction layer, along with mysqli for MySQL-specific operations. Learners master prepared statements, parameter binding, transaction handling, and connection management. Understanding SQL injection prevention through prepared statements is paramount. The goal is to interact with databases securely and efficiently using parameterized queries, handle errors appropriately, and leverage database features like transactions while maintaining code portability across different database systems through PDO abstraction.

|Topic|Focus & Purpose|
|---|---|
|**[[1.8.1 Database Connection (PDO)]]**|PDO instantiation; connection strings (DSN); connection options; persistent connections; error modes (`ERRMODE_EXCEPTION`, `ERRMODE_WARNING`, `ERRMODE_SILENT`); closing connections. Establishes database connectivity.|
|**[[1.8.2 Executing Queries]]**|`query()` for simple queries; `exec()` for non-SELECT queries; fetching results (`fetch`, `fetchAll`, `fetchColumn`); fetch modes (`FETCH_ASSOC`, `FETCH_OBJ`, `FETCH_NUM`). Defines query execution.|
|**[[1.8.3 Prepared Statements]]**|`prepare()` and `execute()`; named placeholders (`:param`); positional placeholders (`?`); binding parameters (`bindParam`, `bindValue`); why prepared statements prevent SQL injection. Critical for security.|
|**[[1.8.4 Transactions]]**|`beginTransaction()`, `commit()`, `rollBack()`; ACID properties; transaction isolation; savepoints; when to use transactions. Ensures data integrity.|
|**[[1.8.5 Error Handling in Database Operations]]**|PDO exceptions; `try-catch` for database errors; `errorInfo()`, `errorCode()`; logging database errors; graceful degradation. Defines database error management.|
|**[[1.8.6 MySQLi (Alternative)]]**|MySQLi vs PDO; procedural vs OOP interface; `mysqli_connect`, `mysqli_query`; prepared statements in mysqli; when mysqli is appropriate. Provides MySQL-specific alternative.|

---

## **1.9 Data Structures & Algorithms**

### **Learning Intent (~100–110 words)**

Algorithmic thinking and data structure knowledge separate competent programmers from interview-ready developers. This section teaches PHP's SPL (Standard PHP Library) data structures, algorithmic problem-solving patterns, and complexity analysis. Learners master how PHP arrays internally work as hash tables, when to use SPL structures over arrays, and common algorithmic techniques applicable to web development scenarios. Understanding Big O notation enables reasoning about performance trade-offs in real applications. The goal is to approach coding challenges methodically, optimize database-heavy operations, and implement efficient solutions using appropriate data structures—skills tested in 60-70% of PHP backend developer interviews.

|Topic|Focus & Purpose|
|---|---|
|**[[1.9.1 Time & Space Complexity (Big O)]]**|Big O notation; analyzing algorithm efficiency; common complexities (O(1), O(n), O(log n), O(n²)); space-time trade-offs; understanding PHP array operation costs. Critical for performance optimization.|
|**[[1.9.2 PHP Arrays as Hash Tables]]**|How PHP arrays internally use hash tables; collision handling; why array access is O(1); when to use arrays vs SPL structures; memory implications of large arrays. Fundamental to PHP performance.|
|**[[1.9.3 Array Algorithms]]**|Two-pointer technique; array searching and sorting; frequency counting with arrays; array merging strategies; optimizing nested loops; practical web application scenarios. Foundational interview patterns.|
|**[[1.9.4 SPL Data Structures]]**|`SplStack`, `SplQueue`, `SplHeap`, `SplFixedArray`, `SplObjectStorage`; when to use over arrays; memory efficiency; performance characteristics; implementing custom iterators. Enables specialized collection operations.|
|**[[1.9.5 String Algorithms]]**|Pattern matching beyond regex; substring search optimization; string manipulation for form processing; anagram detection; palindrome checking. Common in web development interviews.|
|**[[1.9.6 Linked Lists (Conceptual)]]**|Linked list concepts; implementing singly/doubly linked lists in PHP; when linked lists matter; understanding `SplDoublyLinkedList`; comparison with PHP arrays. Classic data structure understanding.|
|**[[1.9.7 Trees & Hierarchies]]**|Tree structures for categories/menus; recursive tree traversal; binary trees concepts; implementing hierarchical data (parent-child relationships); breadth-first vs depth-first traversal. Essential for CMS/e-commerce.|
|**[[1.9.8 Recursion]]**|Recursive thinking; base cases; recursive file/directory operations; tree traversal with recursion; avoiding stack overflow; when recursion is appropriate. Enables divide-and-conquer solutions.|
|**[[1.9.9 Sorting & Searching]]**|Understanding PHP's `sort()`, `usort()`; binary search; when to sort; search optimization in large datasets; custom comparators. Algorithm fundamentals.|

---

# **SECTION 2 — APPLIED (Real-World PHP Development)**

## **Learning Intent (~90–100 words)**

This section converts theory into practical web application development. Learners build forms, handle user input, manage sessions and authentication, work with APIs, and apply security best practices. The focus shifts from language features to using PHP in production environments—building dynamic websites, processing data, securing applications, and integrating with external services. Applied mastery means shipping features that are secure, performant, and maintainable while understanding common vulnerabilities and how to prevent them through proper input validation, output escaping, and secure coding practices.

### **2.1 Forms & User Input Handling**

|Area|Purpose|
|---|---|
|**2.1.1 Form Processing**|Accessing form data (`$_GET`, `$_POST`); form submission methods; processing text, checkboxes, radio buttons, select menus; file uploads; form redirection.|
|**2.1.2 Input Validation**|Server-side validation; required fields; data type validation; format validation (email, URL, phone); length constraints; custom validation rules; preventing injection attacks.|
|**2.1.3 Input Sanitization**|`filter_var()` with sanitization filters; `htmlspecialchars()`, `strip_tags()`; trimming whitespace; removing dangerous characters; context-specific sanitization.|
|**2.1.4 Displaying Errors**|Collecting validation errors; displaying error messages; preserving form values; user-friendly error presentation; avoiding information disclosure.|

### **2.2 Sessions & State Management**

|Area|Purpose|
|---|---|
|**2.2.1 Session Basics**|`session_start()`; `$_SESSION` superglobal; storing session data; session lifetime; `session_destroy()`; session configuration (`session.cookie_*`).|
|**2.2.2 Authentication**|Login/logout implementation; password hashing (`password_hash`, `password_verify`); session-based authentication; remember me functionality; logout cleanup; preventing brute force.|
|**2.2.3 Authorization**|Role-based access control; permission checking; protecting pages; middleware pattern; separating authentication from authorization.|
|**2.2.4 Session Security**|Session hijacking prevention; session fixation prevention; regenerating session IDs (`session_regenerate_id()`); secure and HttpOnly cookies; CSRF protection.|

### **2.3 Security Best Practices**

|Area|Purpose|
|---|---|
|**2.3.1 SQL Injection Prevention**|Using prepared statements; parameterized queries; avoiding dynamic SQL construction; input validation for database operations; understanding attack vectors.|
|**2.3.2 Cross-Site Scripting (XSS) Prevention**|Output escaping with `htmlspecialchars()`; Content Security Policy headers; context-aware escaping; JavaScript injection prevention; sanitizing user-generated content.|
|**2.3.3 Cross-Site Request Forgery (CSRF) Prevention**|CSRF tokens; token generation and validation; SameSite cookie attribute; verifying request origin; synchronizer token pattern.|
|**2.3.4 Additional Security Measures**|Password best practices (bcrypt, Argon2); rate limiting; input length limits; file upload security; directory traversal prevention; avoiding `eval()` and `exec()`; security headers (X-Frame-Options, X-Content-Type-Options).|

### **2.4 Working with JSON & APIs**

|Area|Purpose|
|---|---|
|**2.4.1 JSON Handling**|`json_encode()`, `json_decode()`; handling encoding errors; `JSON_*` constants; working with JSON options; nested JSON structures; type preservation.|
|**2.4.2 RESTful API Consumption**|Making HTTP requests with `curl`; `file_get_contents()` with stream contexts; handling API responses; authentication (API keys, Bearer tokens, OAuth); error handling; timeout configuration.|
|**2.4.3 Building REST APIs**|Routing HTTP methods (GET, POST, PUT, DELETE); accepting JSON input; returning JSON responses; HTTP status codes; API authentication (JWT, tokens); rate limiting; versioning; CORS handling; API documentation.|

### **2.5 Email & Communication**

|Area|Purpose|
|---|---|
|**2.5.1 Sending Email**|`mail()` function; email headers; PHPMailer/SwiftMailer libraries; SMTP configuration; HTML emails; attachments; email validation; handling bounces.|
|**2.5.2 Email Security**|SPF, DKIM, DMARC awareness; preventing email injection; validating email addresses; rate limiting email sends; avoiding spam filters.|

### **2.6 Mini Projects (PHP-Focused)**

|Level|Project|Outcome|
|---|---|---|
|Beginner|Contact form with validation and email|Reinforces form handling, validation, sanitization, email sending, error display.|
|Intermediate|User authentication system with sessions|Builds session management, password hashing, database interaction, security practices.|
|Advanced|REST API with JWT authentication|Applies routing, JSON handling, prepared statements, token authentication, rate limiting.|
|Production|Blog CMS with admin panel|Applies full CRUD, authorization, XSS/CSRF prevention, file uploads, pagination, search.|

---

# **SECTION 3 — PROFESSIONAL (Frameworks, Patterns & Tooling)**

## **Learning Intent (~85–95 words)**

This section positions PHP within professional development ecosystems. Learners master design patterns, understand modern PHP frameworks (especially Laravel in depth), use Composer for dependency management, implement testing strategies, and leverage debugging tools. The goal is long-term competence—writing maintainable code that scales across teams, integrates with frameworks and libraries, and follows industry best practices. Professional PHP developers understand MVC architecture, service containers, ORM patterns, use version control, write tests, and deploy applications using modern DevOps practices.

|Area|Focus|
|---|---|
|**3.1 Design Patterns**|MVC (Model-View-Controller); repository pattern; factory pattern; singleton; dependency injection; service container; observer pattern; patterns in Laravel context.|
|**3.2 Composer & Dependency Management**|`composer.json`; installing packages; autoloading (PSR-4); semantic versioning; `composer.lock`; `require` vs `require-dev`; package discovery; packagist.org.|

### **3.3 Laravel Framework (Deep Dive)**

|Sub-Topic|Focus & Purpose|
|---|---|
|**[[3.3.1 Service Container & Dependency Injection]]**|Understanding Laravel's IoC container; binding interfaces to implementations; automatic dependency resolution (zero-configuration); service providers (`register()`, `boot()` methods); contextual binding; when to use `app()` vs constructor injection vs facades; singleton vs transient bindings. Critical for Laravel architecture understanding.|
|**[[3.3.2 Eloquent ORM Deep Dive]]**|Model relationships (hasMany, belongsTo, belongsToMany, hasOneThrough, hasManyThrough, morphMany); eager loading vs lazy loading; N+1 query problem detection and prevention (`Model::preventLazyLoading()`); query scopes (local, global); accessors and mutators; model events; mass assignment protection; soft deletes. Most common interview topic.|
|**[[3.3.3 Database Migrations & Seeding]]**|Creating and running migrations; rollback strategies; migration best practices; database seeders for test data; factories for model generation; `php artisan migrate` workflow; production migration considerations.|
|**[[3.3.4 Request Lifecycle & Middleware]]**|HTTP request to response flow; kernel handling; service provider boot sequence; middleware execution order (global, route, controller); creating custom middleware; middleware groups; terminable middleware.|
|**[[3.3.5 Routing Advanced]]**|Route model binding (implicit, explicit); route caching for performance; resource controllers (`php artisan make:controller --resource`); API resources; route naming; route groups; fallback routes; rate limiting with throttle middleware.|
|**[[3.3.6 Queues & Background Jobs]]**|Queue drivers (database, Redis, SQS, Beanstalkd); dispatching jobs (`dispatch()`, `dispatchAfter()`); handling failed jobs; job batching; queue workers (`php artisan queue:work`); job chaining; job middleware; retry strategies.|
|**[[3.3.7 Events & Listeners]]**|Creating events and listeners; event discovery; event broadcasting (WebSockets, Pusher, Laravel Echo); real-time features; queued event listeners; event subscribers.|
|**[[3.3.8 Authorization & Policies]]**|Gates for simple authorization; policies for model authorization; `@can` blade directives; policy methods (`view`, `create`, `update`, `delete`); policy auto-discovery; before methods; middleware authorization.|
|**[[3.3.9 API Development with Laravel]]**|API resources for response transformation; Eloquent API resources; API authentication with Laravel Sanctum (token-based, SPA authentication); API rate limiting; versioning strategies; RESTful conventions in Laravel; API error handling; pagination.|
|**[[3.3.10 Blade Templating]]**|Blade syntax; template inheritance (`@extends`, `@section`, `@yield`); components and slots; conditional directives (`@if`, `@auth`, `@guest`); loops (`@foreach`, `@forelse`); including subviews; escaping (`{{ }}` vs `{!! !!}`); custom Blade directives.|
|**[[3.3.11 Validation in Laravel]]**|Form request validation; validation rules; custom validation rules; conditional validation; displaying validation errors in views; `$errors` variable; `old()` helper for repopulating forms; API validation responses.|

|Area|Focus|
|---|---|
|**3.4 PSR Standards**|PHP-FIG standards; PSR-1/PSR-12 (coding standards); PSR-4 (autoloading); PSR-3 (logging); PSR-7 (HTTP messages); PSR-11 (container interface); adhering to community standards.|
|**3.5 Testing PHP**|PHPUnit; writing test cases; assertions; mocking; test-driven development (TDD); integration testing; feature testing in Laravel (HTTP tests, database tests); Pest (modern testing framework); test coverage.|
|**3.6 Debugging & Profiling**|Xdebug configuration; breakpoints; step debugging; profiling performance bottlenecks; stack traces; memory usage analysis; Laravel Telescope for debugging; `dd()` and `dump()` helpers.|
|**3.7 Version Control & Git**|Git basics; branching strategies (Git Flow, GitHub Flow); pull requests; `.gitignore` for PHP projects; collaborative workflows; GitHub/GitLab integration; commit best practices.|
|**3.8 Deployment & DevOps**|Server requirements (PHP version, extensions); Apache/Nginx configuration; PHP-FPM; environment variables (`.env`); Laravel deployment (Forge, Envoyer, Vapor); Docker for PHP; CI/CD pipelines; zero-downtime deployment.|

---

# **SECTION 4 — REFERENCE & ECOSYSTEM CONTEXT**

## **Learning Intent (~75–85 words)**

This section defines PHP's boundaries, ecosystem position, and evolution. Learners understand PHP's role as a server-side language, its relationship with web servers, modern PHP versions (PHP 8.x features), and where PHP fits in the web development landscape alongside JavaScript frameworks, databases, and cloud platforms. Understanding PHP's strengths, limitations, and best use cases enables informed technology selection. Ecosystem literacy helps developers navigate frameworks, libraries, hosting options, and community resources effectively.

|Area|Focus|
|---|---|
|**4.1 PHP Versions & Evolution**|PHP 7.x improvements (type declarations, null coalescing, spaceship operator); PHP 8.0 (named arguments, match expression, attributes, union types, JIT compiler); PHP 8.1 (enums, fibers, readonly properties, first-class callables, intersection types); PHP 8.2 (readonly classes, DNF types); PHP 8.3-8.4 features; migration considerations; staying current.|
|**4.2 PHP & Web Servers**|Apache mod_php vs PHP-FPM; Nginx with PHP-FPM; CGI/FastCGI; web server configuration; `.htaccess` usage; `php.ini` settings; opcache configuration; server requirements for hosting.|
|**4.3 PHP Ecosystem**|Popular frameworks (Laravel 85%, Symfony, CodeIgniter); CMSs (WordPress, Drupal); testing tools (PHPUnit, Pest); quality tools (PHPStan, Psalm, PHP CS Fixer); packagist.org repository; PHP community resources.|
|**4.4 Modern PHP Context**|PHP in 2024-2025; performance improvements (PHP 8 JIT); type safety enhancements; PHP vs Node.js/Python/Go; when PHP is appropriate; PHP for APIs vs full-stack; PHP with frontend frameworks (React, Vue); serverless PHP (AWS Lambda, Bref).|
|**4.5 Security Considerations**|OWASP Top 10 for PHP; common PHP vulnerabilities (file inclusion, code injection, insecure deserialization); security-focused `php.ini` configurations; staying updated with security patches; security resources (PHP Security Consortium).|
|**4.6 Performance Optimization**|OPcache configuration and tuning; database query optimization; caching strategies (Redis, Memcached); profiling tools (Blackfire, Xdebug); lazy loading; minimizing file I/O; HTTP caching; CDN usage.|
|**4.7 PHP Limitations & Boundaries**|Single-threaded execution model (blocking I/O); shared-nothing architecture; statelessness between requests; when to use Node.js/Go for real-time applications; PHP as backend for SPAs; understanding PHP's sweet spot (CRUD applications, content management).|

---