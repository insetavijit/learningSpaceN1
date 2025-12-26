## 🔹 1.1 PHP Fundamentals & Setup

- **History & Versions** : PHP evolution, PHP 8.x features
    
- **Execution Model** : server-side scripting
    
- **Embedding in HTML** : `<?php ... ?>`, short tags
    
- **Script Delimiters** : PHP open/close tags
    
- **Comments** : `//`, `#`, `/* ... */`
    
- **Output** : `echo` vs `print`
    
- **Variables** : `$var`, dynamic typing, no declaration
    
- **Data Types**
    
    - **Scalar** : `int`, `float`, `string`, `bool`
        
    - **Compound** : `array`, `object`
        
    - **Special** : `null`, `resource`
        
- **Type System** : type juggling vs `declare(strict_types=1)`
    
- **Constants** : `define()`, `const`
    
- **Operators** : arithmetic, assignment, comparison, logical, bitwise, spaceship `<=>`
    
- **Superglobals** : `$_GET`, `$_POST`, `$_SERVER`, `$_SESSION`, etc.
    
- **Configuration** : `php.ini` basics
    

---

## 🔹 1.2 Control Structures & Loops

- **Conditionals** : `if / elseif / else`, `switch`
    
- **Alternative Syntax** : `endif`, `endswitch`, etc.
    
- **Loops** : `while`, `do-while`, `for`, `foreach`
    
- **Flow Control** : `break`, `continue`, `goto`
    
- **Match Expression** : `match` (PHP 8+)
    
- **Nullsafe Operator** : `?->`
    
- **Error Suppression** : `@`
    
- **File Inclusion**
    
    - `include`, `require`
        
    - `include_once`, `require_once`
        
- **Best Practices** : safe inclusion patterns
    

---

## 🔹 1.3 Functions & Scope

- **Function Declaration** : `function`, `return`
    
- **Parameters** : typed, default, variadic `...$args`
    
- **Return Types** : type hints
    
- **Arrow Functions** : `fn()`
    
- **Anonymous Functions** : closures
    
- **Scope** : `global`, `static`, local
    
- **Closure Use** : `use ($var)`
    
- **Callable Type** : `callable`
    
- **Attributes** : `#[Attribute]` (PHP 8+)
    
- **Generators** : `yield`, `yield from`
    

---

## 🔹 1.4 Arrays & Data Structures

- **Array Types** : indexed, associative, multidimensional
    
- **Syntax** : `[]` vs `array()`
    
- **Core Functions**
    
    - `count`, `sort`, `in_array`
        
    - `array_merge`, `array_filter`, `array_map`, `array_reduce`
        
- **Destructuring** : `list()`
    
- **Spread Operator** : `...$arr`
    
- **Keyed vs Short Syntax**
    
- **SPL Structures** : `SplFixedArray`, `ArrayObject`
    

---

## 🔹 1.5 Object-Oriented Programming

- **Classes & Objects**
    
- **Properties & Methods**
    
- **Visibility** : `public`, `protected`, `private`
    
- **Constructors** : `__construct`, promotion (PHP 8)
    
- **Static Members** : `static`
    
- **Inheritance** : `extends`
    
- **Interfaces** : `implements`
    
- **Traits**
    
- **Abstract Classes**
    
- **Final Keyword**
    
- **Magic Methods** : `__get`, `__set`, `__toString`, `__invoke`, etc.
    
- **Anonymous Classes**
    
- **Readonly Classes** : PHP 8.2+
    
- **Enums** : pure & backed (PHP 8.1+)
    

---

## 🔹 1.6 Error Handling & Exceptions

- **Error Reporting** : `error_reporting`, `display_errors`
    
- **Exceptions** : `try / catch / throw / finally`
    
- **Custom Exceptions**
    
- **Error vs Exception**
    
- **Throwable Interface**
    
- **Error Conversion** : errors → exceptions
    
- **Handlers** : `set_exception_handler`
    
- **Assertions** : `assert()`
    

---

## 🔹 1.7 Working with Forms & HTTP

- **Request Data** : `$_GET`, `$_POST`
    
- **File Uploads** : `$_FILES`
    
- **Validation & Sanitization** : `filter_var`, `FILTER_*`
    
- **CSRF Protection** : tokens
    
- **Sessions** : `session_start()`, `$_SESSION`
    
- **Cookies** : `setcookie()`
    
- **Headers & Redirects** : `header()`, `http_response_code()`
    
- **REST Concepts** : verbs, status codes
    

---

## 🔹 1.8 Strings & Regular Expressions

- **String Functions** : `strlen`, `strpos`, `substr`, `str_replace`
    
- **Split & Join** : `explode`, `implode`
    
- **Heredoc / Nowdoc**
    
- **Interpolation** : `"Hello $name"`
    
- **PCRE**
    
    - `preg_match`
        
    - `preg_replace`
        
    - `preg_split`
        
- **Patterns** : modifiers, flags
    
- **Named Groups** : `(?<name>...)`
    

---

## 🔹 1.9 Databases & PDO

- **Drivers** : MySQLi vs PDO
    
- **Connections** : `new PDO()`
    
- **Prepared Statements**
    
- **Fetching** : `FETCH_ASSOC`, `FETCH_OBJ`
    
- **Transactions** : `beginTransaction`, `commit`, `rollBack`
    
- **Error Modes** : `PDO::ERRMODE_EXCEPTION`
    
- **Extensions** : SQLite, PostgreSQL
    

---

## 🔹 1.10 Security & Best Practices

- **SQL Injection** : prepared statements
    
- **XSS Prevention** : `htmlspecialchars`, `strip_tags`
    
- **Passwords** : `password_hash`, `password_verify`
    
- **Session Security** : cookie flags, regeneration
    
- **Input Validation** : filter APIs
    
- **Composer** : dependency management
    
- **Autoloading** : PSR-4
    
- **Namespaces**
    
- **Performance** : OPcache, PHP-FPM
    
- **Security Headers**
    
- **Vulnerabilities** : OWASP Top 10
    

---

## 🔹 1.11 Modern PHP Features & Ecosystem

- **Attributes & Reflection**
    
- **Fibers** : async foundations (PHP 8.1+)
    
- **First-Class Callables**
    
- **Match & Nullsafe** : modern control
    
- **JIT** : PHP 8+ performance
    
- **Composer & Packagist**
    
- **Frameworks** : Laravel, Symfony, Slim
    
- **Testing** : PHPUnit, Pest
    
- **Static Analysis** : PHPStan, Psalm
    
- **Refactoring** : Rector
    

---

If you want, I can next:

- Add **`[[Wiki Links]]`** for each PHP chapter.
    
- Merge PHP with your **WordPress / backend notes** into an index.
    
- Or generate a **PHP cheatsheet** distilled from this map.