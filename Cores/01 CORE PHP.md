### **1.1 PHP 8+ Language Fundamentals**

Master modern PHP (8.0-8.3) as a professional server-side language. Learn variables, types, operators, and PHP 8's powerful features (named arguments, union types, match expressions, enums). Think in modern PHP—not legacy PHP 5, but type-safe, performant PHP 8+ used in production. Understand strict types, nullsafe operators, and modern syntax. This prevents writing outdated code and establishes patterns for Laravel/Symfony. Jobs expect PHP 8 knowledge; legacy PHP 5/7 skills are insufficient.

|Topic|Focus & Purpose|
|---|---|
|**[[1.1.1 Modern PHP Setup]]**|PHP 8.3 installation; Composer (dependency management); local development (Laravel Valet, Docker, XAMPP); PHP CLI vs web server; php.ini configuration; Xdebug; version managers (phpenv). Professional environment setup.|
|**[[1.1.2 Variables & Type System]]**|Variable syntax ($var); scalar types (int, float, string, bool); compound types (array, object); type juggling vs strict types (`declare(strict_types=1)`); union types (PHP 8); intersection types (PHP 8.1); nullable types; type coercion rules. Modern type safety.|
|**[[1.1.3 Operators & Control Flow]]**|Arithmetic, comparison, logical operators; null coalescing (`??`); nullsafe operator (`?->` PHP 8); spaceship (`<=>`); if/else, switch; **match expression (PHP 8)**; ternary; loops (for, foreach, while, do-while). Modern syntax patterns.|
|**[[1.1.4 Functions & Closures]]**|Function declaration; parameters; **named arguments (PHP 8)**; return type declarations; variadic functions; closures; arrow functions (PHP 7.4); first-class callable syntax (PHP 8.1); scope (local, global, static); anonymous functions. Functional programming in PHP.|
|**[[1.1.5 Arrays & Collections]]**|Indexed arrays; associative arrays; multidimensional arrays; array functions (map, filter, reduce, walk); spread operator in arrays; array unpacking; when arrays vs objects; array performance. PHP's primary data structure.|
|**[[1.1.6 Error Handling]]**|Error types (Fatal, Warning, Notice); exceptions; try-catch-finally; custom exceptions; `set_error_handler()`; error suppression (@); converting errors to exceptions; production vs development error handling. Robust error management.|

### **1.2 Object-Oriented PHP (Production Patterns)**

Modern PHP is object-oriented. Master classes, inheritance, interfaces, traits, and PHP 8's OOP enhancements. Learn encapsulation, polymorphism, and composition essential for Laravel/Symfony. Understand OOP is non-negotiable—frameworks are built on it, jobs require it. Write maintainable, testable object-oriented code using readonly properties, enums, attributes. Know SOLID principles, dependency injection, and when OOP vs procedural. This section bridges basic PHP to professional framework development.

|Topic|Focus & Purpose|
|---|---|
|**[[1.2.1 Classes & Objects Basics]]**|Class declaration; properties and methods; `$this` keyword; constructors; **constructor property promotion (PHP 8)**; visibility (public, private, protected); destructors; object instantiation; static members. OOP foundation.|
|**[[1.2.2 Inheritance & Polymorphism]]**|Extending classes; method overriding; `parent::` keyword; abstract classes; final classes/methods; polymorphism; when inheritance is appropriate; inheritance vs composition. Code reuse through hierarchy.|
|**[[1.2.3 Interfaces & Type Contracts]]**|Interface declaration; implementing interfaces; multiple interface implementation; interface inheritance; type hinting with interfaces; design by contract; interface segregation principle. Behavioral contracts.|
|**[[1.2.4 Traits & Horizontal Reuse]]**|Trait declaration; using traits; conflict resolution (`insteadof`, `as`); traits vs inheritance; multiple traits; when traits solve problems; trait composition. Flexible code sharing.|
|**[[1.2.5 Modern PHP 8+ OOP Features]]**|**Readonly properties (PHP 8.1)**; **readonly classes (PHP 8.2)**; **enums (PHP 8.1)**; **attributes/annotations (PHP 8)**; property hooks (PHP 8.4); asymmetric visibility (PHP 8.4); first-class callables. Cutting-edge OOP.|
|**[[1.2.6 Magic Methods]]**|`__construct`, `__get`, `__set`, `__call`, `__toString`, `__invoke`, `__serialize`, `__unserialize`, `__clone`; when to use vs avoid; performance implications; magic method use cases. Dynamic behavior.|
|**[[1.2.7 SOLID Principles in PHP]]**|Single Responsibility; Open/Closed; Liskov Substitution; Interface Segregation; Dependency Inversion; applying SOLID to PHP classes; recognizing violations; refactoring to SOLID. Professional OOP design.|

### **1.3 Working with Databases (SQL & Eloquent)**

Database interaction is central to PHP applications. Master PDO for raw SQL, understand prepared statements for security, and learn Eloquent ORM (Laravel's database layer). Understand SQL fundamentals, relationships, transactions, and query optimization. Learn when to use raw SQL vs ORM, how to prevent SQL injection, and optimize database performance. This section covers both traditional PHP database access and modern ORM patterns essential for Laravel development.

|Topic|Focus & Purpose|
|---|---|
|**[[1.3.1 PDO Fundamentals]]**|PDO instantiation; DSN connection strings; connection options; error modes (exception, warning, silent); prepared statements; parameter binding (named, positional); fetching results (FETCH_ASSOC, FETCH_OBJ); transactions; when PDO vs mysqli. Secure database access.|
|**[[1.3.2 SQL Injection Prevention]]**|Understanding SQL injection attacks; prepared statements as defense; parameter binding; input validation; escaping (when absolutely necessary); never concatenating SQL; security best practices. Critical security skill.|
|**[[1.3.3 Database Relationships]]**|One-to-one, one-to-many, many-to-many; foreign keys; JOIN operations; relationship modeling; denormalization decisions; when to normalize vs denormalize. Data modeling.|
|**[[1.3.4 Transactions & ACID]]**|BEGIN, COMMIT, ROLLBACK; transaction isolation levels; ACID properties; when transactions are necessary; savepoints; handling transaction errors; nested transactions. Data integrity.|
|**[[1.3.5 Query Optimization]]**|EXPLAIN queries; indexing strategies; N+1 query problem; query caching; avoiding SELECT *; pagination; query performance profiling; database-specific optimizations. Performance tuning.|
|**[[1.3.6 Eloquent ORM (Laravel)]]**|Model definition; CRUD operations; relationships (hasMany, belongsTo, belongsToMany); eager loading vs lazy loading; N+1 prevention; query scopes; accessors/mutators; model events. Modern ORM patterns.|
|**[[1.3.7 Database Migrations]]**|Migration files; schema definition; running/rolling back migrations; seeding; database version control; team collaboration with migrations; production migration strategies. Database versioning.|

### **1.4 Web Development Fundamentals**

Understand HTTP protocol, forms, sessions, cookies, and file uploads. Master request-response cycle, GET vs POST, form validation, and state management. Learn security essentials: XSS prevention, CSRF protection, input sanitization. Understand web fundamentals before frameworks—this knowledge transfers across all PHP applications. Know when to use sessions vs cookies, how to handle file uploads securely, and HTTP status codes. Essential for any PHP web developer.

|Topic|Focus & Purpose|
|---|---|
|**[[1.4.1 HTTP & Request-Response]]**|HTTP methods (GET, POST, PUT, DELETE); status codes (200, 404, 500); headers; `$_SERVER` superglobal; request lifecycle; PHP as request handler; stateless HTTP. Protocol understanding.|
|**[[1.4.2 Forms & Input Handling]]**|Processing $_GET, $_POST; form submission; input validation; sanitization (filter_var, htmlspecialchars); validation rules; error display; preserving form values. User input processing.|
|**[[1.4.3 Sessions & Cookies]]**|`session_start()`; $_SESSION management; session configuration; session security; cookies (setcookie); cookie attributes (httpOnly, secure, SameSite); session vs cookies; session hijacking prevention. State management.|
|**[[1.4.4 File Uploads]]**|$_FILES superglobal; move_uploaded_file(); file validation (type, size); secure upload handling; directory permissions; storing files (filesystem vs database); upload security risks. File handling.|
|**[[1.4.5 Authentication Basics]]**|Password hashing (password_hash, password_verify); bcrypt; login/logout flow; session-based auth; "remember me" functionality; password reset; brute force prevention. User authentication.|
|**[[1.4.6 Security Essentials]]**|XSS prevention (output escaping); CSRF protection (tokens); SQL injection (prepared statements); input validation vs sanitization; secure session handling; security headers; OWASP Top 10. Critical security knowledge.|

### **1.5 Modern PHP Development Tools**

Professional PHP requires tooling: Composer for dependencies, Git for version control, PHPStan/Psalm for static analysis, PHPUnit for testing. Master command-line PHP, understand package management, write tests, and use quality tools. Learn debugging with Xdebug, profiling performance, and continuous integration. Modern PHP development is disciplined, tested, and uses industry-standard tools. This separates hobbyists from professionals.

|Topic|Focus & Purpose|
|---|---|
|**[[1.5.1 Composer & Dependencies]]**|composer.json; installing packages; autoloading (PSR-4); semantic versioning; composer.lock; scripts; packagist.org; managing dependencies; require vs require-dev. Package management.|
|**[[1.5.2 Git Version Control]]**|Git basics for PHP projects; branching; .gitignore for PHP; committing best practices; GitHub/GitLab workflows; collaboration; code review. Version control essentials.|
|**[[1.5.3 Testing with PHPUnit]]**|Unit testing; test cases; assertions; mocking; test doubles; test organization; TDD workflow; code coverage; testing best practices. Ensuring code quality.|
|**[[1.5.4 Static Analysis]]**|PHPStan; Psalm; type checking; configuration levels; integrating with CI; fixing violations; benefits of static analysis. Code quality tools.|
|**[[1.5.5 Code Quality Tools]]**|PHP_CodeSniffer; PHP CS Fixer; coding standards (PSR-1, PSR-12); automated formatting; linting; integrating in workflow. Maintaining standards.|
|**[[1.5.6 Debugging & Profiling]]**|Xdebug configuration; breakpoints; step debugging; profiling performance; var_dump vs proper debugging; logging strategies. Development efficiency.|
