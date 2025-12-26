### **Overview**

String and array operations form the backbone of real-world PHP development—powering everything from request handling, database formatting, and API responses to data parsing, validation, and transformation. Mastering these operations allows you to manipulate text, handle lists, structure complex data, and build highly expressive, performant logic. Clean and efficient use of strings and arrays is one of the strongest indicators of mature PHP skill.
Got it — you want **the same table**, but with each topic using the pattern:
### **Topics to Learn — String & Array Operations**

|**Topic**|**Improved Brief Description (25+ Words)**|
|---|---|
|**[[String Declaration.spl]]**|Understanding how PHP treats single-quoted and double-quoted strings is essential for controlling interpolation, escaping rules, and performance. Proper declaration forms the foundation of all text processing throughout an application.|
|**[[Interpolation.spl]]**|Interpolation simplifies dynamic string creation by embedding variables directly inside double quotes, reducing concatenation overhead and making templates, logs, and user-facing messages significantly more readable and maintainable.|
|**[[String Concatenation.spl]]**|Concatenation joins multiple text fragments using `.` or `.=` and is crucial for building dynamic output, assembling responses, generating messages, and formatting data for interfaces or logs efficiently.|
|**[[String Length & Inspection.spl]]**|Tools like `strlen`, `mb_strlen`, `substr`, `strpos`, and `str_contains` let you inspect content, measure length, slice text, and locate substrings—core skills for validation, parsing, and processing.|
|**[[String Manipulation.spl]]**|Involves trimming whitespace, converting case, slicing segments, replacing patterns, and formatting values. These operations enable clean input handling, text normalization, and predictable content transformation in any PHP system.|
|**[[Regular Expressions.spl]]**|Regex enables complex pattern matching and replacement using `preg_match` and `preg_replace`. It powers validation, data extraction, content cleanup, and rule-driven text processing through highly expressive patterns.|
|**[[Encoding & Multibyte Handling.spl]]**|Multibyte-safe operations via `mb_*` ensure correct handling of UTF-8 characters, emojis, and multilingual text—essential for modern applications that rely on accurate Unicode processing and stable string behavior.|
|**[[Array Declaration.spl]]**|Arrays—indexed, associative, or multi-dimensional—store structured collections of data. They are PHP’s most versatile container type, powering configurations, payloads, query results, and application state.|
|**[[Array Access & Mutation.spl]]**|Reading, updating, pushing, popping, shifting, or removing array values dynamically shapes and manipulates datasets, forming the core of request handling, API processing, and runtime modifications.|
|**[[Array Iteration.spl]]**|Iteration with `foreach`, internal pointers, or SPL iterators enables structured processing of lists and datasets. It is central to transformation, filtering, output generation, and algorithmic workflows.|
|**[[Array Search & Filters.spl]]**|Functions like `array_filter`, `array_map`, `array_reduce`, `in_array`, and `array_search` enable powerful functional-style transformations, cleanup, searching, and discovery of patterns within collections.|
|**[[Sorting Arrays.spl]]**|Sorting tools—`sort`, `asort`, `ksort`, and custom comparators—organize values for display, decision-making, or processing workflows, ensuring predictable ordering across all data-driven operations.|
|**[[Merging & Combining Arrays.spl]]**|Combining arrays using `array_merge`, spread operator `[...]`, or union operator `+` helps assemble configurations, merge datasets, unify inputs, and produce larger coherent structures from multiple sources.|
|**[[Destructuring.spl]]**|`list()` and array unpacking simplify extracting structured values into variables, making API responses, config arrays, and nested data far more readable and convenient to work with.|
|**[[JSON Encoding-Decoding.spl]]**|Encoding/decoding JSON with `json_encode` and `json_decode` enables seamless communication with APIs, JavaScript clients, logs, and storage layers, ensuring smooth cross-system data exchange.|
|**[[Performance Considerations.spl]]**|Efficient manipulation reduces memory copies, avoids unnecessary loops, and leverages PHP’s optimized built-ins—significantly improving responsiveness and scalability in large or high-throughput applications.|
|**[[Common Pitfalls.spl]]**|Pitfalls like numeric-string key collisions, type juggling quirks, overwritten values during merges, and misunderstanding array unions lead to subtle bugs unless consciously avoided.|
|**[[Best Practices.spl]]**|Best practices include using `mb_*` for UTF-8, preferring immutability, avoiding deep nesting, writing predictable transformations, and validating input to maintain clean, stable codebases.|

---
# String and Array Operations in PHP: A Comprehensive Analysis

## Abstract

String and array operations constitute the fundamental building blocks of modern PHP development, powering critical functionalities ranging from HTTP request processing and database interaction to API response generation and complex data transformations. This comprehensive research paper examines the complete spectrum of string and array manipulation techniques available in PHP, with particular emphasis on features introduced in PHP 8.0 and later versions. Through detailed analysis of 18 core operational categories, this study explores declaration patterns, manipulation techniques, performance characteristics, security implications, and best practices that distinguish professional-grade PHP code from amateur implementations.

The paper addresses both foundational concepts and advanced patterns, incorporating insights from official PHP documentation, security frameworks like OWASP, performance benchmarks, and real-world implementation scenarios. Key findings reveal that proper handling of multibyte character encodings, understanding of type juggling behaviors in array operations, and strategic use of native PHP functions can significantly impact application performance and security posture. The research demonstrates that many common vulnerabilities and performance bottlenecks stem from misunderstanding subtle differences in string interpolation, array merging behaviors, and encoding handling. By synthesizing current best practices with emerging PHP 8+ features such as named arguments, the nullsafe operator, and enhanced type system support, this paper provides developers with actionable guidance for writing robust, secure, and performant PHP applications.

## 1. Introduction

### 1.1 The Evolution of String and Array Operations in PHP

PHP has evolved dramatically since its inception in 1994 as a simple templating language. String and array operations, being central to web development tasks, have undergone significant refinement across major PHP versions. Early PHP versions (3.x and 4.x) provided basic string concatenation and array manipulation functions, but lacked sophisticated handling of character encodings and type safety.

The introduction of PHP 5 brought substantial improvements including better object-oriented programming support and enhanced array functions. However, it was PHP 7's release in 2015 that marked a watershed moment for performance, delivering up to 2x speed improvements for many string and array operations through the new Zend Engine 3.0. PHP 7 introduced significant performance enhancements to the language's core, making previously expensive operations like array merging and string concatenation considerably more efficient.

PHP 8.0, released in November 2020, and subsequent 8.x versions have continued this trajectory by introducing features that make string and array operations more expressive and type-safe. Named arguments, union types, match expressions, and the nullsafe operator have fundamentally changed how developers approach data manipulation. The introduction of the spread operator for arrays and improvements to JSON handling reflect PHP's maturation into a language suitable for enterprise-scale applications.

### 1.2 Why String and Array Operations Matter

In modern PHP applications, strings and arrays are omnipresent. Consider a typical web request lifecycle: the superglobal arrays `$_GET`, `$_POST`, and `$_SERVER` contain request data as strings and arrays. Database query results are returned as arrays of associative arrays. API responses are encoded as JSON strings derived from array structures. Template rendering involves string interpolation and concatenation. Input validation requires string pattern matching and array filtering.

The quality of string and array manipulation code directly impacts:

**Performance**: Inefficient array operations or excessive string concatenation can create performance bottlenecks. A single misused loop that repeatedly concatenates strings can slow page generation by orders of magnitude. Understanding when to use native functions versus manual iteration, or when array copying occurs, is essential for maintaining responsive applications.

**Security**: String operations are frequent vectors for injection attacks. SQL injection, cross-site scripting (XSS), and command injection vulnerabilities often stem from improper string sanitization. Injection flaws occur when untrusted data is sent to an interpreter as part of a command or query. Array operations also present security concerns, particularly when handling user input that might exploit type juggling behaviors or when serializing data for external systems.

**Maintainability**: Code that clearly expresses intent through appropriate string and array operations is easier to understand, test, and modify. Using semantic function names like `array_filter()` versus manual loops, or leveraging string functions like `str_contains()` over regex patterns, improves code readability dramatically.

**Internationalization**: Modern applications serve global audiences requiring proper UTF-8 and multibyte character handling. Multibyte string functions are essential for correctly handling UTF-8 encoded text. Failure to use appropriate multibyte functions can lead to data corruption, incorrect string lengths, and broken substring operations.

### 1.3 Research Objectives

This research paper aims to accomplish the following objectives:

- **Comprehensive Coverage**: Document all essential string and array operations in PHP, from basic declaration to advanced functional programming patterns
- **Modern Focus**: Emphasize PHP 8.0+ features while maintaining backward compatibility awareness for legacy codebases
- **Security Integration**: Identify security implications for each operation category and provide concrete mitigation strategies
- **Performance Analysis**: Present benchmarking data and optimization techniques for common operations
- **Practical Application**: Provide real-world code examples demonstrating both correct and incorrect approaches
- **Best Practices Synthesis**: Distill actionable guidelines that developers can immediately apply to improve code quality
- **Framework Context**: Show how string and array operations integrate with popular frameworks like Laravel and Symfony
- **Common Pitfalls Documentation**: Catalog frequent mistakes and misunderstandings that lead to bugs

### 1.4 Significance for Backend Development

Backend PHP development requires mastery of data manipulation. Unlike frontend JavaScript where data structures are often simple and localized, backend systems must process complex nested arrays from databases, transform data between formats (JSON, XML, CSV), validate and sanitize user input from multiple sources, construct dynamic SQL queries safely, generate formatted output for various consumers (APIs, templates, logs), and maintain session and cache data structures.

String and array proficiency directly correlates with a developer's ability to:

- Build efficient database abstraction layers that transform query results into domain objects
- Implement robust validation systems that handle edge cases gracefully
- Create flexible API endpoints that support filtering, sorting, and pagination
- Develop secure authentication systems that properly handle credentials and tokens
- Construct logging and debugging systems that provide useful diagnostic information
- Optimize data processing pipelines that handle thousands of records efficiently

The patterns and techniques covered in this paper form the foundation upon which more advanced backend concepts—like ORM usage, caching strategies, and microservice communication—are built. A developer who struggles with array transformations will find it difficult to effectively use Eloquent queries or implement complex business logic. Similarly, inadequate string handling knowledge leads to security vulnerabilities and internationalization failures.

## 2. String Declaration and Fundamental Concepts

### 2.1 String Declaration Syntax in PHP

PHP provides four primary methods for declaring strings, each with distinct characteristics regarding interpolation, escaping, and performance implications. Understanding these differences is crucial for writing efficient and maintainable code.

#### 2.1.1 Single-Quoted Strings

Single-quoted strings represent the simplest form of string declaration in PHP. They treat nearly all characters literally, with only two escape sequences: `\'` for a literal single quote and `\\` for a literal backslash.

```php
<?php
$literal = 'Hello, World!';
$withQuote = 'It\'s a beautiful day';
$withBackslash = 'C:\\Users\\Documents';
$noInterpolation = 'The value is $variable'; // Outputs: The value is $variable
```

**Performance Characteristics**: Single-quoted strings are marginally faster than double-quoted strings because the parser does not need to scan for variable interpolation. For constant strings or templates where no variables need embedding, single quotes are the optimal choice.

**Use Cases**:

- SQL query templates before parameter binding
- Regular expression patterns
- HTML templates without dynamic content
- Configuration keys and constant values

#### 2.1.2 Double-Quoted Strings

Double-quoted strings enable variable interpolation and support a comprehensive set of escape sequences including `\n` (newline), `\t` (tab), `\r` (carriage return), `\$` (literal dollar sign), and `\"` (double quote).

```php
<?php
$name = 'Alice';
$greeting = "Hello, $name!"; // Hello, Alice!
$formatted = "Line 1\nLine 2\tTabbed";
$price = 19.99;
$message = "The price is \$$price"; // The price is $19.99
```

**Complex Variable Interpolation**: For array elements or object properties, use curly braces to delineate the variable expression:

```php
<?php
$user = ['name' => 'Bob', 'age' => 30];
$info = "User: {$user['name']}, Age: {$user['age']}";

$object = new stdClass();
$object->title = 'Developer';
$text = "Position: {$object->title}";
```

**Security Consideration**: While interpolation is convenient, never interpolate unvalidated user input directly into strings used for system commands, SQL queries, or HTML output, as this creates injection vulnerabilities.

#### 2.1.3 Heredoc Syntax

Heredoc provides a way to declare multi-line strings with interpolation, useful for templates, formatted text, or embedded code blocks:

```php
<?php
$title = "Welcome";
$content = <<<EOT
<html>
<head>
    <title>$title</title>
</head>
<body>
    <h1>$title</h1>
    <p>This is a multi-line template.</p>
</body>
</html>
EOT;
```

**Rules**:

- The opening identifier (EOT in this example) can be any valid label
- The closing identifier must appear on its own line with no leading whitespace (prior to PHP 7.3)
- PHP 7.3+ allows indented closing identifiers, with indentation stripped from all lines

**PHP 7.3 Flexible Heredoc**:

```php
<?php
function generateHtml() {
    $title = "Dashboard";
    return <<<HTML
        <div class="container">
            <h1>$title</h1>
            <p>Content here</p>
        </div>
    HTML; // Closing tag indented to match function body
}
```

#### 2.1.4 Nowdoc Syntax

Nowdoc is to heredoc what single-quoted strings are to double-quoted strings—no interpolation or escape sequence processing occurs:

```php
<?php
$regex = <<<'PATTERN'
/^[a-zA-Z0-9._%+-]+@[a-zA-Z0-9.-]+\.[a-zA-Z]{2,}$/
PATTERN;

$sqlTemplate = <<<'SQL'
SELECT * FROM users 
WHERE status = 'active' 
AND created_at > $placeholder
SQL;
// $placeholder is literal text, not a variable
```

**Use Cases**:

- Regular expressions containing complex patterns
- SQL templates with literal placeholders
- Code samples in documentation
- Configuration templates

### 2.2 String Type Juggling and Conversion

PHP's dynamic type system performs automatic conversions between strings and other types, which can be both convenient and dangerous.

#### 2.2.1 Implicit Conversions

```php
<?php
$num = 42;
$str = "The answer is " . $num; // int to string conversion

$numStr = "123";
$result = $numStr + 100; // string "123" converts to int 123, result is 223

$float = "3.14";
$calculated = $float * 2; // string "3.14" converts to float 3.14

// Dangerous behavior
$weird = "10 apples" + 5; // Result: 15 (string parsed as 10)
```

**Comparison Pitfalls**:

```php
<?php
// Loose comparison coerces types
var_dump("0" == 0);       // true
var_dump("0" == false);   // true
var_dump("0e0" == "0");   // true (scientific notation)
var_dump("123" == "  123"); // true (whitespace ignored)

// Strict comparison checks type and value
var_dump("0" === 0);      // false
var_dump("123" === 123);  // false
```

**Security Impact**: Type juggling in comparisons can lead to authentication bypasses. Always use strict comparison (`===`, `!==`) when comparing passwords, tokens, or security-sensitive values.

#### 2.2.2 Explicit Conversions

```php
<?php
// Casting
$str = (string) 123;
$int = (int) "456";
$float = (float) "3.14159";

// Using functions (preferred for clarity)
$str = strval(123);
$int = intval("789");
$float = floatval("2.718");

// Type declarations in functions (PHP 7+)
function processId(string $id): int {
    return (int) $id;
}

// Strict types mode (PHP 7+)
declare(strict_types=1);

function strictFunc(string $value): void {
    echo $value;
}

// strictFunc(123); // TypeError: must be string, int given
```

### 2.3 String Encoding Fundamentals

Modern PHP applications must handle UTF-8 encoding correctly to support international users and prevent data corruption.

#### 2.3.1 ASCII vs UTF-8

ASCII uses single bytes (0-127) to represent characters, sufficient for English but inadequate for global languages. UTF-8 uses 1-4 bytes per character, supporting over 1 million code points including emojis, accented characters, and scripts like Arabic, Chinese, and Cyrillic.

```php
<?php
// ASCII string
$ascii = "Hello";
echo strlen($ascii); // 5 bytes

// UTF-8 string
$utf8 = "Héllo 世界 🌍";
echo strlen($utf8);    // 18 bytes (incorrect character count!)
echo mb_strlen($utf8); // 9 characters (correct)
```

**Critical Rule**: Always use `mb_*` functions for strings containing non-ASCII characters:

```php
<?php
$text = "Café";

// Wrong: byte-based operations
$wrong = substr($text, 0, 3);  // "Caf" - cuts in middle of UTF-8 byte sequence
$wrongLen = strlen($text);      // 5 bytes, not 4 characters

// Correct: character-based operations
$correct = mb_substr($text, 0, 3);  // "Caf"
$correctLen = mb_strlen($text);     // 4 characters
```

#### 2.3.2 Setting Default Encoding

Configure PHP to use UTF-8 throughout your application:

```php
<?php
// In php.ini
// default_charset = "UTF-8"
// mbstring.internal_encoding = UTF-8

// Or at runtime
mb_internal_encoding('UTF-8');
mb_http_output('UTF-8');
mb_regex_encoding('UTF-8');

// Database connection
$pdo = new PDO(
    'mysql:host=localhost;dbname=test;charset=utf8mb4',
    $user,
    $pass
);
```

**HTML Output**: Always specify UTF-8 encoding in your HTML:

```php
<?php
header('Content-Type: text/html; charset=UTF-8');
?>
<!DOCTYPE html>
<html>
<head>
    <meta charset="UTF-8">
    <title>Page Title</title>
</head>
```

## 3. String Interpolation and Concatenation

### 3.1 Variable Interpolation Techniques

String interpolation embeds variable values directly within double-quoted strings or heredoc blocks, offering cleaner syntax than concatenation for simple cases.

#### 3.1.1 Simple Variable Interpolation

```php
<?php
$username = "alice";
$count = 42;

// Basic interpolation
$message = "Welcome, $username!";
$status = "You have $count new messages.";

// Adjacent text requires delimitation
$filename = "{$username}_profile.jpg"; // Correct
// $filename = "$username_profile.jpg"; // Wrong: looks for $username_profile
```

#### 3.1.2 Complex Expression Interpolation

For array access, object properties, or method calls, use curly brace syntax:

```php
<?php
$user = [
    'name' => 'Bob',
    'email' => 'bob@example.com',
    'preferences' => ['theme' => 'dark']
];

$greeting = "Hello, {$user['name']}!";
$contact = "Email: {$user['email']}";
$theme = "Theme: {$user['preferences']['theme']}";

// Object property access
$obj = new stdClass();
$obj->title = 'Manager';
$obj->department = 'Sales';

$info = "Title: {$obj->title}, Department: {$obj->department}";

// Method calls require workarounds
class User {
    private $name = "Charlie";
    public function getName() {
        return $this->name;
    }
}

$user = new User();
// $text = "User: {$user->getName()}"; // Syntax error
$name = $user->getName();
$text = "User: $name"; // Assign to variable first
```

#### 3.1.3 Performance Considerations

Interpolation is generally faster than concatenation for simple cases because it requires a single parser pass:

```php
<?php
// Benchmarking interpolation vs concatenation
$iterations = 1000000;
$name = "test";
$value = 123;

// Interpolation
$start = microtime(true);
for ($i = 0; $i < $iterations; $i++) {
    $result = "Name: $name, Value: $value";
}
$interpolationTime = microtime(true) - $start;

// Concatenation
$start = microtime(true);
for ($i = 0; $i < $iterations; $i++) {
    $result = "Name: " . $name . ", Value: " . $value;
}
$concatenationTime = microtime(true) - $start;

// Typically interpolation is 5-15% faster for simple cases
```

However, for complex string building with many variables or conditional logic, concatenation or array joining may be clearer and potentially faster.

### 3.2 String Concatenation Patterns

The concatenation operator (`.`) and concatenation assignment operator (`.=`) join strings together. Proper usage patterns affect both performance and readability.

#### 3.2.1 Basic Concatenation

```php
<?php
$first = "Hello";
$last = "World";
$greeting = $first . " " . $last; // "Hello World"

$path = "/var/www" . "/" . "html" . "/" . "index.php";

// Concatenation assignment
$message = "Processing";
$message .= "...";
$message .= " Done!"; // "Processing... Done!"
```

#### 3.2.2 Building Large Strings Efficiently

**Anti-Pattern: Repeated Concatenation in Loops**

```php
<?php
// WRONG: Creates new string object on each iteration
$html = "";
foreach ($items as $item) {
    $html .= "<li>" . htmlspecialchars($item) . "</li>\n";
}
// For 1000 items, creates 1000 intermediate string copies
```

**Best Practice: Array Join Pattern**

```php
<?php
// CORRECT: Build array, then join once
$parts = [];
foreach ($items as $item) {
    $parts[] = "<li>" . htmlspecialchars($item) . "</li>";
}
$html = implode("\n", $parts);
// Single concatenation operation at the end
```

**Performance Comparison**: For generating a string from 10,000 elements, the array join pattern is typically 3-5x faster than repeated concatenation.

#### 3.2.3 Concatenation with Different Types

```php
<?php
$base = "Total: ";
$amount = 49.99;
$currency = "USD";

// Automatic type conversion
$result = $base . $amount . " " . $currency; // "Total: 49.99 USD"

// Explicit conversion for clarity
$result = $base . strval($amount) . " " . $currency;

// Boolean concatenation
$isActive = true;
$status = "Active: " . ($isActive ? "yes" : "no");
// Better than: "Active: " . $isActive // outputs "Active: 1"
```

### 3.3 Modern PHP String Syntax Features

PHP 8.x introduces features that improve string handling ergonomics and type safety.

#### 3.3.1 Nullsafe Operator in String Context

```php
<?php
class User {
    public ?Profile $profile = null;
}

class Profile {
    public string $bio = "";
}

$user = new User();

// PHP 7 approach
$bio = $user->profile !== null ? $user->profile->bio : "No bio";

// PHP 8+ nullsafe operator
$bio = $user->profile?->bio ?? "No bio";

// In string interpolation
$display = "Bio: " . ($user->profile?->bio ?? "Not provided");
```

#### 3.3.2 Named Arguments and String Functions

Named arguments (PHP 8.0+) improve readability for functions with many parameters:

```php
<?php
// Before PHP 8
$padded = str_pad("test", 10, "0", STR_PAD_LEFT);

// PHP 8+ with named arguments
$padded = str_pad(
    string: "test",
    length: 10,
    pad_string: "0",
    pad_type: STR_PAD_LEFT
);

// Especially useful for substr
$part = substr(
    string: $longText,
    offset: 0,
    length: 100
);
```

#### 3.3.3 Match Expressions for String Processing

The `match` expression (PHP 8.0+) provides a cleaner alternative to switch statements for string-based logic:

```php
<?php
$contentType = 'application/json';

// Old switch approach
switch ($contentType) {
    case 'application/json':
        $handler = new JsonHandler();
        break;
    case 'application/xml':
        $handler = new XmlHandler();
        break;
    case 'text/csv':
        $handler = new CsvHandler();
        break;
    default:
        throw new Exception("Unsupported type");
}

// New match expression
$handler = match ($contentType) {
    'application/json' => new JsonHandler(),
    'application/xml' => new XmlHandler(),
    'text/csv' => new CsvHandler(),
    default => throw new Exception("Unsupported type")
};

// Match with multiple conditions
$category = match (true) {
    str_starts_with($url, '/admin') => 'admin',
    str_starts_with($url, '/api') => 'api',
    str_contains($url, '/public') => 'public',
    default => 'unknown'
};
```

## 4. String Length, Inspection, and Analysis

### 4.1 Measuring String Length

Accurately determining string length requires understanding the difference between byte length and character length, especially critical for UTF-8 text.

#### 4.1.1 Byte Length vs Character Length

```php
<?php
$ascii = "Hello";
$utf8 = "Héllo";
$emoji = "Hello 👋🌍";

// strlen() returns byte count
echo strlen($ascii);  // 5 bytes
echo strlen($utf8);   // 6 bytes (é is 2 bytes in UTF-8)
echo strlen($emoji);  // 13 bytes (emoji use 4 bytes each)

// mb_strlen() returns character count
echo mb_strlen($ascii);  // 5 characters
echo mb_strlen($utf8);   // 5 characters
echo mb_strlen($emoji);  // 9 characters (space + 2 emoji)
```

**Rule of Thumb**: Use `mb_strlen()` for any user-generated content, international text, or when you need accurate character counts for display or validation purposes.

#### 4.1.2 Validation Use Cases

```php
<?php
function validateUsername(string $username): bool {
    $length = mb_strlen($username);
    
    // Username must be 3-20 characters (not bytes)
    if ($length < 3 || $length > 20) {
        return false;
    }
    
    // Check for valid characters
    if (!preg_match('/^[a-zA-Z0-9_]+$/u', $username)) {
        return false;
    }
    
    return true;
}

function truncateForDisplay(string $text, int $maxLength = 100): string {
    if (mb_strlen($text) <= $maxLength) {
        return $text;
    }
    
    // Truncate to max length and add ellipsis
    return mb_substr($text, 0, $maxLength - 3) . '...';
}

// Database field length validation
function validateField(string $value, int $dbFieldLength): bool {
    // Database stores bytes, so check byte length
    return strlen($value) <= $dbFieldLength;
}
```

### 4.2 String Position and Search Operations

Locating substrings within strings is fundamental for parsing, validation, and text processing.

#### 4.2.1 Position Finding Functions

```php
<?php
$text = "The quick brown fox jumps over the lazy dog";

// strpos() - find first occurrence (byte position)
$pos = strpos($text, "quick");  // 4
$notFound = strpos($text, "cat"); // false (not 0!)

// Correct way to check for substring
if (strpos($text, "fox") !== false) {
    echo "Found fox";
}

// strrpos() - find last occurrence
$lastThe = strrpos($text, "the");  // 31

// Case-insensitive searches
$pos = stripos($text, "BROWN");  // 10

// Multibyte-safe position finding
$utf8Text = "Café résumé";
$pos = mb_strpos($utf8Text, "résumé");  // 5 (character position)
```

**Common Pitfall**: Never use loose comparison with `strpos()`:

```php
<?php
$text = "0123456789";

// WRONG: This looks for "0" which is at position 0
if (strpos($text, "0")) {
    // Never executes because 0 == false
}

// CORRECT: Strict comparison
if (strpos($text, "0") !== false) {
    // Executes correctly
}
```

#### 4.2.2 Modern PHP String Contains Functions

PHP 8.0 introduced cleaner functions for checking substring presence:

```php
<?php
$email = "user@example.com";

// PHP 8.0+ str_contains()
if (str_contains($email, "@")) {
    echo "Valid email format";
}

// Replaces older pattern
if (strpos($email, "@") !== false) { /* ... */ }

// str_starts_with() and str_ends_with()
$filename = "document.pdf";

if (str_starts_with($filename, "doc")) {
    echo "Document file";
}

if (str_ends_with($filename, ".pdf")) {
    echo "PDF format";
}

// Case-insensitive variants don't exist, use lowercase comparison
$lower = strtolower($filename);
if (str_starts_with($lower, "doc")) { /* ... */ }
```

**URL Routing Example**:

```php
<?php
function routeRequest(string $path): string {
    return match (true) {
        str_starts_with($path, '/api/') => 'api_handler',
        str_starts_with($path, '/admin/') => 'admin_handler',
        str_starts_with($path, '/user/') => 'user_handler',
        $path === '/' => 'home_handler',
        default => 'not_found_handler'
    };
}
```

### 4.3 Substring Extraction

Extracting portions of strings is essential for parsing, formatting, and data manipulation.

#### 4.3.1 Basic Substring Operations

```php
<?php
$text = "Hello, World!";

// substr(string, offset, length)
$hello = substr($text, 0, 5);      // "Hello"
$world = substr($text, 7, 5);      // "World"
$fromEnd = substr($text, -6);      // "World!"
$middle = substr($text, 7, -1);    // "World"

// Negative offset starts from end
$lastThree = substr($text, -3);    // "ld!"

// mb_substr() for UTF-8
$utf8 = "Héllo, Wörld!";
$part = mb_substr($utf8, 0, 5);    // "Héllo" (5 characters, not bytes)
```

#### 4.3.2 Practical Substring Applications

```php
<?php
// Extract domain from email
function getDomain(string $email): string {
    $atPos = strpos($email, '@');
    if ($atPos === false) {
        return '';
    }
    return substr($email, $atPos + 1);
}

// Parse ISO date
function parseIsoDate(string $date): array {
    // Format: 2024-03-15
    return [
        'year' => substr($date, 0, 4),
        'month' => substr($date, 5, 2),
        'day' => substr($date, 8, 2)
    ];
}

// Mask credit card number
function maskCreditCard(string $number): string {
    $length = strlen($number);
    if ($length < 4) {
        return $number;
    }
    
    $lastFour = substr($number, -4);
    $masked = str_repeat('*', $length - 4);
    return $masked . $lastFour;
}

echo maskCreditCard("1234567890123456"); // ************3456
```

### 4.4 String Comparison Operations

Comparing strings correctly is crucial for sorting, searching, and security-sensitive operations like password verification.

#### 4.4.1 Comparison Operators and Functions

```php
<?php
// Lexicographic comparison with operators
$result = "apple" < "banana";  // true
$result = "10" < "2";          // true (string comparison, not numeric)

// strcmp() returns -1, 0, or 1
$cmp = strcmp("apple", "banana");  // -1 (apple < banana)
$cmp = strcmp("test", "test");     // 0 (equal)
$cmp = strcmp("zebra", "apple");   // 1 (zebra > apple)

// Case-insensitive comparison
$cmp = strcasecmp("Apple", "apple");  // 0

// Natural order comparison (numbers within strings)
$files = ["file1.txt", "file10.txt", "file2.txt"];
usort($files, "strnatcmp");
// Result: ["file1.txt", "file2.txt", "file10.txt"]
```

#### 4.4.2 Security-Critical Comparisons

For password hashes, tokens, and other security-sensitive values, use timing-attack-safe comparison:

```php
<?php
// WRONG: Timing attack vulnerable
function verifyTokenWrong(string $provided, string $actual): bool {
    return $provided === $actual;
    // Comparison fails fast on first mismatch
    // Attacker can measure timing to guess characters
}

// CORRECT: Constant-time comparison
function verifyToken(string $provided, string $actual): bool {
    return hash_equals($actual, $provided);
    // Always compares all characters
    // Timing doesn't reveal information
}

// Password verification (always use password_verify)
$hash = password_hash("secret", PASSWORD_DEFAULT);
if (password_verify("secret", $hash)) {
    echo "Password correct";
}
```

#### 4.4.3 Multibyte String Comparison

```php
<?php
// Locale-aware comparison for internationalization
$names = ["Émilie", "Zoë", "André", "Ève"];

// Standard sort treats accented characters by byte value
sort($names);
// Result depends on character encoding

// Locale-aware collation
setlocale(LC_COLLATE, 'fr_FR.UTF-8');
usort($names, "strcoll");
// Proper alphabetical order for French

// Or use Intl extension
$collator = new Collator('fr_FR');
$collator->sort($names);
```

## 5. String Manipulation and Transformation

### 5.1 Case Conversion Operations

Converting string case is common for normalization, comparison, and formatting purposes.

#### 5.1.1 Basic Case Conversion

```php
<?php
$text = "Hello World";

// Convert entire string
$lower = strtolower($text);      // "hello world"
$upper = strtoupper($text);      // "HELLO WORLD"

// First character only
$ucfirst = ucfirst($text);       // "Hello World" (already capitalize
$lcfirst = lcfirst($text); // "hello World"

// Each word $title = ucwords($text); // "Hello World" $title = ucwords("hello-world"); // "Hello-world" (only spaces by default)

// Custom delimiters for ucwords $title = ucwords("hello-world-example", "-"); // "Hello-World-Example"

````

#### 5.1.2 Multibyte Case Conversion

For UTF-8 text with accented characters or non-Latin scripts, use multibyte functions:

```php
<?php
$text = "Café Résumé";

// Wrong: Loses accents or corrupts characters
$lower = strtolower($text);  // "café résumé" (might work, but unreliable)

// Correct: Multibyte-safe
$lower = mb_strtolower($text);  // "café résumé"
$upper = mb_strtoupper($text);  // "CAFÉ RÉSUMÉ"

// Turkish locale example (special case)
$turkish = "İstanbul";
$lower = mb_strtolower($turkish, 'tr_TR');  // "istanbul" with Turkish rules
````

#### 5.1.3 Use Cases and Best Practices

```php
<?php
// Email normalization
function normalizeEmail(string $email): string {
    $email = trim($email);
    return mb_strtolower($email);
}

// Case-insensitive array key lookup
function findValue(array $data, string $key): mixed {
    $lowerKeys = array_change_key_case($data, CASE_LOWER);
    return $lowerKeys[strtolower($key)] ?? null;
}

// Slug generation for URLs
function generateSlug(string $title): string {
    $title = mb_strtolower($title);
    $title = preg_replace('/[^a-z0-9]+/', '-', $title);
    return trim($title, '-');
}

echo generateSlug("Hello World! This is a Test."); // "hello-world-this-is-a-test"
```

### 5.2 Trimming and Whitespace Management

Removing unwanted whitespace is essential for data cleaning and validation.

#### 5.2.1 Basic Trimming Functions

```php
<?php
$text = "  Hello World  \n\t";

// Remove whitespace from both ends
$trimmed = trim($text);      // "Hello World"

// Remove from left only
$ltrimmed = ltrim($text);    // "Hello World  \n\t"

// Remove from right only
$rtrimmed = rtrim($text);    // "  Hello World"

// Custom character mask
$data = "---Hello---";
$trimmed = trim($data, '-'); // "Hello"

// Multiple characters in mask
$path = "//path/to/file//";
$clean = trim($path, '/');   // "path/to/file"
```

#### 5.2.2 Advanced Whitespace Handling

```php
<?php
// Remove all whitespace
$text = "Hello World  Test";
$noSpaces = preg_replace('/\s+/', '', $text);  // "HelloWorldTest"

// Normalize multiple spaces to single space
$text = "Hello    World   Test";
$normalized = preg_replace('/\s+/', ' ', $text); // "Hello World Test"
$normalized = trim($normalized);

// Remove specific Unicode whitespace characters
$text = "Hello\xC2\xA0World"; // Contains non-breaking space
$clean = preg_replace('/\s+/u', ' ', $text);
```

#### 5.2.3 Input Sanitization Patterns

```php
<?php
function sanitizeInput(string $input): string {
    // Trim whitespace
    $input = trim($input);
    
    // Remove null bytes (security)
    $input = str_replace("\0", '', $input);
    
    // Normalize line endings
    $input = str_replace("\r\n", "\n", $input);
    $input = str_replace("\r", "\n", $input);
    
    // Limit consecutive newlines
    $input = preg_replace("/\n{3,}/", "\n\n", $input);
    
    return $input;
}

// Form data cleaning
function cleanFormData(array $data): array {
    return array_map(function($value) {
        if (is_string($value)) {
            return trim($value);
        }
        if (is_array($value)) {
            return cleanFormData($value);
        }
        return $value;
    }, $data);
}

$cleanData = cleanFormData($_POST);
```

### 5.3 String Replacement Operations

Replacing substrings is fundamental for text processing, template rendering, and data transformation.

#### 5.3.1 Simple String Replacement

```php
<?php
$text = "Hello World";

// str_replace(search, replace, subject)
$new = str_replace("World", "PHP", $text);  // "Hello PHP"

// Multiple replacements
$text = "apples and oranges";
$new = str_replace(
    ["apples", "oranges"],
    ["oranges", "apples"],
    $text
); // "oranges and apples"

// Case-insensitive replacement
$text = "Hello WORLD";
$new = str_ireplace("world", "PHP", $text);  // "Hello PHP"

// Count replacements
$text = "test test test";
$new = str_replace("test", "demo", $text, $count);
echo $count;  // 3
```

#### 5.3.2 Performance Characteristics

```php
<?php
// str_replace() is very fast for simple replacements
$text = str_repeat("test ", 10000);

// Fast: Direct string replacement
$start = microtime(true);
$result = str_replace("test", "demo", $text);
$time1 = microtime(true) - $start;

// Slower: Regex replacement (unnecessary for literal strings)
$start = microtime(true);
$result = preg_replace("/test/", "demo", $text);
$time2 = microtime(true) - $start;

// str_replace() is typically 3-10x faster than preg_replace() for literals
```

#### 5.3.3 Template Replacement Patterns

```php
<?php
// Simple template engine
function renderTemplate(string $template, array $data): string {
    $search = [];
    $replace = [];
    
    foreach ($data as $key => $value) {
        $search[] = "{{" . $key . "}}";
        $replace[] = htmlspecialchars($value);
    }
    
    return str_replace($search, $replace, $template);
}

$template = "<h1>{{title}}</h1><p>Welcome, {{username}}!</p>";
$html = renderTemplate($template, [
    'title' => 'Dashboard',
    'username' => 'Alice'
]);

// URL query parameter replacement
function buildUrl(string $template, array $params): string {
    $url = $template;
    foreach ($params as $key => $value) {
        $url = str_replace(
            '{' . $key . '}',
            urlencode($value),
            $url
        );
    }
    return $url;
}

$url = buildUrl(
    'https://api.example.com/users/{id}/posts/{slug}',
    ['id' => 123, 'slug' => 'hello-world']
);
```

### 5.4 String Padding and Formatting

Padding strings to fixed widths is useful for formatting output, generating fixed-width files, and alignment.

#### 5.4.1 Padding Operations

```php
<?php
$text = "Hello";

// str_pad(string, length, pad_string, type)
$right = str_pad($text, 10);                    // "Hello     " (default right pad)
$left = str_pad($text, 10, "0", STR_PAD_LEFT);  // "00000Hello"
$both = str_pad($text, 10, "-", STR_PAD_BOTH);  // "--Hello---"

// Custom padding character
$binary = str_pad(decbin(5), 8, "0", STR_PAD_LEFT);  // "00000101"

// Zero-padding numbers
$id = str_pad("42", 6, "0", STR_PAD_LEFT);  // "000042"
```

#### 5.4.2 Practical Formatting Examples

```php
<?php
// Generate fixed-width report
function generateReport(array $data): string {
    $lines = [];
    $lines[] = str_pad("Name", 20) . str_pad("Value", 10, " ", STR_PAD_LEFT);
    $lines[] = str_repeat("-", 30);
    
    foreach ($data as $name => $value) {
        $lines[] = str_pad($name, 20) . str_pad($value, 10, " ", STR_PAD_LEFT);
    }
    
    return implode("\n", $lines);
}

$report = generateReport([
    'Total Sales' => '1,234.56',
    'Total Orders' => '45',
    'Avg Order' => '27.43'
]);

/*
Output:
Name                      Value
------------------------------
Total Sales            1,234.56
Total Orders                 45
Avg Order                 27.43
*/

// Invoice number generation
function generateInvoiceNumber(int $id): string {
    return 'INV-' . date('Y') . '-' . str_pad($id, 6, '0', STR_PAD_LEFT);
}

echo generateInvoiceNumber(42);  // "INV-2024-000042"
```

### 5.5 String Reversal and Character Operations

While less common, string reversal and character-level operations have specific use cases.

```php
<?php
// Reverse entire string
$text = "Hello";
$reversed = strrev($text);  // "olleH"

// Character-at-position access
$text = "Hello";
$first = $text[0];   // "H"
$last = $text[-1];   // "o" (PHP 7.1+)

// Multibyte character access
$utf8 = "Héllo";
$char = mb_substr($utf8, 1, 1);  // "é"

// Check if string contains only certain characters
function isAlphanumeric(string $text): bool {
    return ctype_alnum($text);
}

function isNumeric(string $text): bool {
    return ctype_digit($text);
}

// Character frequency analysis
function charFrequency(string $text): array {
    $freq = [];
    $length = mb_strlen($text);
    
    for ($i = 0; $i < $length; $i++) {
        $char = mb_substr($text, $i, 1);
        $freq[$char] = ($freq[$char] ?? 0) + 1;
    }
    
    return $freq;
}
```

## 6. Regular Expressions in PHP

Regular expressions provide powerful pattern matching capabilities for complex string operations, validation, and extraction.

### 6.1 PCRE Functions Overview

PHP uses Perl-Compatible Regular Expressions (PCRE) through the `preg_*` family of functions.

#### 6.1.1 Core Pattern Matching Functions

```php
<?php
// preg_match() - Find first match
$text = "The price is $49.99";
if (preg_match('/\$(\d+\.\d{2})/', $text, $matches)) {
    echo $matches[0];  // "$49.99" (full match)
    echo $matches[1];  // "49.99" (captured group)
}

// preg_match_all() - Find all matches
$text = "Call 555-1234 or 555-5678";
preg_match_all('/\d{3}-\d{4}/', $text, $matches);
print_r($matches[0]);  // ["555-1234", "555-5678"]

// preg_replace() - Replace matches
$text = "Hello World";
$new = preg_replace('/World/', 'PHP', $text);  // "Hello PHP"

// preg_split() - Split by pattern
$text = "apple,banana;cherry:date";
$fruits = preg_split('/[,;:]/', $text);
// ["apple", "banana", "cherry", "date"]

// preg_grep() - Filter array by pattern
$words = ["apple", "banana", "apricot", "cherry"];
$aWords = preg_grep('/^a/', $words);
// ["apple", "apricot"]
```

#### 6.1.2 Return Values and Error Handling

```php
<?php
// preg_match returns 1 (match), 0 (no match), or false (error)
$result = preg_match('/pattern/', $text);

if ($result === false) {
    // Pattern syntax error
    $error = preg_last_error_msg();  // PHP 8.0+
    throw new Exception("Regex error: $error");
}

if ($result === 1) {
    // Match found
}

// Check for PREG errors
function safeRegex(string $pattern, string $subject): bool {
    $result = @preg_match($pattern, $subject);
    
    if ($result === false) {
        $errorMsg = preg_last_error_msg();
        throw new RuntimeException("Regex failed: $errorMsg");
    }
    
    return $result === 1;
}
```

### 6.2 Pattern Syntax and Common Patterns

Understanding regex syntax is essential for writing effective patterns.

#### 6.2.1 Basic Pattern Elements

```php
<?php
// Literal characters
preg_match('/hello/', 'hello world');  // Match

// Character classes
preg_match('/[aeiou]/', 'test');      // Match vowel
preg_match('/[^aeiou]/', 'test');     // Match non-vowel
preg_match('/[0-9]/', 'test123');     // Match digit
preg_match('/[a-z]/', 'Test');        // Match lowercase

// Shorthand character classes
preg_match('/\d/', '123');     // Digit [0-9]
preg_match('/\w/', 'test_123'); // Word char [a-zA-Z0-9_]
preg_match('/\s/', 'hello world'); // Whitespace

// Quantifiers
preg_match('/\d+/', '123');      // One or more digits
preg_match('/\d*/', 'abc');      // Zero or more digits
preg_match('/\d?/', 'a1');       // Zero or one digit
preg_match('/\d{3}/', '123');    // Exactly 3 digits
preg_match('/\d{2,4}/', '12');   // 2 to 4 digits
preg_match('/\d{3,}/', '1234');  // 3 or more digits

// Anchors
preg_match('/^start/', 'start here');  // Beginning of string
preg_match('/end$/', 'the end');       // End of string
preg_match('/^\d+$/', '123');          // Entire string is digits
```

#### 6.2.2 Common Validation Patterns

```php
<?php
class Validator {
    // Email validation (simple)
    public static function isEmail(string $email): bool {
        return (bool) preg_match(
            '/^[a-zA-Z0-9._%+-]+@[a-zA-Z0-9.-]+\.[a-zA-Z]{2,}$/',
            $email
        );
    }
    
    // Phone number (US format)
    public static function isPhone(string $phone): bool {
        return (bool) preg_match(
            '/^(\+1)?[-.\s]?\(?(\d{3})\)?[-.\s]?(\d{3})[-.\s]?(\d{4})$/',
            $phone
        );
    }
    
    // URL validation
    public static function isUrl(string $url): bool {
        return (bool) preg_match(
            '/^https?:\/\/(www\.)?[-a-zA-Z0-9@:%._\+~#=]{1,256}\.[a-zA-Z0-9()]{1,6}\b/',
            $url
        );
    }
    
    // Strong password (8+ chars, upper, lower, digit, special)
    public static function isStrongPassword(string $password): bool {
        return (bool) preg_match(
            '/^(?=.*[a-z])(?=.*[A-Z])(?=.*\d)(?=.*[@$!%*?&])[A-Za-z\d@$!%*?&]{8,}$/',
            $password
        );
    }
    
    // IP address (IPv4)
    public static function isIPv4(string $ip): bool {
        return (bool) preg_match(
            '/^(?:(?:25[0-5]|2[0-4][0-9]|[01]?[0-9][0-9]?)\.){3}(?:25[0-5]|2[0-4][0-9]|[01]?[0-9][0-9]?)$/',
            $ip
        );
    }
    
    // Credit card number (basic structure check)
    public static function isCreditCard(string $number): bool {
        $number = preg_replace('/\s+/', '', $number);
        return (bool) preg_match('/^[0-9]{13,19}$/', $number);
    }
}
```

### 6.3 Capture Groups and Extraction

Capture groups extract specific portions of matched patterns.

#### 6.3.1 Basic Capture Groups

```php
<?php
// Extract date components
$date = "2024-03-15";
if (preg_match('/^(\d{4})-(\d{2})-(\d{2})$/', $date, $matches)) {
    $year = $matches[1];   // "2024"
    $month = $matches[2];  // "03"
    $day = $matches[3];    // "15"
}

// Named capture groups (more readable)
if (preg_match('/^(?P<year>\d{4})-(?P<month>\d{2})-(?P<day>\d{2})$/', $date, $matches)) {
    $year = $matches['year'];
    $month = $matches['month'];
    $day = $matches['day'];
}

// Extract all email addresses from text
$text = "Contact alice@example.com or bob@test.org";
preg_match_all('/([a-zA-Z0-9._%+-]+)@([a-zA-Z0-9.-]+\.[a-zA-Z]{2,})/', $text, $matches);
$emails = $matches[0];  // ["alice@example.com", "bob@test.org"]
$usernames = $matches[1];  // ["alice", "bob"]
$domains = $matches[2];  // ["example.com", "test.org"]
```

#### 6.3.2 Practical Extraction Examples

```php
<?php
// Parse log entries
function parseLogEntry(string $line): ?array {
    $pattern = '/^\[(?P<timestamp>[^\]]+)\] (?P<level>\w+): (?P<message>.+)$/';
    
    if (preg_match($pattern, $line, $matches)) {
        return [
            'timestamp' => $matches['timestamp'],
            'level' => $matches['level'],
            'message' => $matches['message']
        ];
    }
    
    return null;
}

$log = "[2024-03-15 10:30:45] ERROR: Database connection failed";
$parsed = parseLogEntry($log);

// Extract URL components
function parseUrl(string $url): array {
    $pattern = '/^(?P<protocol>https?):\/\/(?P<domain>[^\/]+)(?P<path>\/.*)?$/';
    preg_match($pattern, $url, $matches);
    
    return [
        'protocol' => $matches['protocol'] ?? '',
        'domain' => $matches['domain'] ?? '',
        'path' => $matches['path'] ?? '/'
    ];
}

// Extract hashtags from social media text
function extractHashtags(string $text): array {
    preg_match_all('/#(\w+)/', $text, $matches);
    return $matches[1];  // Return just the tag names without #
}

$text = "Love #PHP and #webdev! #coding";
$tags = extractHashtags($text);  // ["PHP", "webdev", "coding"]
```

### 6.4 Advanced Replacement with Callbacks

`preg_replace_callback()` enables complex transformations by processing each match with a custom function.

```php
<?php
// Convert snake_case to camelCase
function snakeToCamel(string $text): string {
    return preg_replace_callback('/_([a-z])/', function($matches) {
        return strtoupper($matches[1]);
    }, $text);
}

echo snakeToCamel('user_name_field');  // "userNameField"

// Mask credit card numbers in text
function maskCreditCards(string $text): string {
    return preg_replace_callback('/\b\d{4}[\s-]?\d{4}[\s-]?\d{4}[\s-]?\d{4}\b/', function($matches) {
        $number = preg_replace('/[\s-]/', '', $matches[0]);
        $lastFour = substr($number, -4);
        return str_repeat('*', strlen($number) - 4) . $lastFour;
    }, $text);
}

// Convert markdown-style links to HTML
function markdownToHtml(string $text): string {
    return preg_replace_callback('/\[([^\]]+)\]\(([^\)]+)\)/', function($matches) {
        $text = htmlspecialchars($matches[1]);
        $url = htmlspecialchars($matches[2]);
        return "<a href=\"$url\">$text</a>";
    }, $text);
}

$md = "Visit [our site](https://example.com)";
$html = markdownToHtml($md);  // Visit <a href="https://example.com">our site</a>
```

### 6.5 Security Considerations: ReDoS Attacks

Regular Expression Denial of Service (ReDoS) attacks exploit poorly written regex patterns that cause catastrophic backtracking.

#### 6.5.1 Vulnerable Patterns

```php
<?php
// VULNERABLE: Nested quantifiers can cause catastrophic backtracking
$bad1 = '/(a+)+$/';
$bad2 = '/(a|a)*$/';
$bad3 = '/(a|ab)*$/';

// Attack: Long string of 'a's without match at end
$attack = str_repeat('a', 30) . 'b';
// preg_match($bad1, $attack); // Can take seconds or hang!

// VULNERABLE: Overlapping alternations
$emailBad = '/^([a-zA-Z0-9._%+-]+)+@[a-zA-Z0-9.-]+\.[a-zA-Z]{2,}$/';
```

#### 6.5.2 Safe Pattern Design

```php
<?php
// SAFE: Avoid nested quantifiers
$safe1 = '/^a+$/';

// SAFE: Use possessive quantifiers (no backtracking)
$safe2 = '/^(a++)$/';

// SAFE: Use atomic groups
$safe3 = '/^(?>a+)$/';

// Set execution time limit
ini_set('pcre.backtrack_limit', 100000);
ini_set('pcre.recursion_limit', 100000);

// Timeout protection wrapper
function safeMatch(string $pattern, string $subject, float $timeout = 1.0): bool {
    $start = microtime(true);
    
    $result = @preg_match($pattern, $subject);
    
    $duration = microtime(true) - $start;
    
    if ($duration > $timeout) {
        throw new RuntimeException("Regex execution exceeded timeout");
    }
    
    return $result === 1;
}
```

#### 6.5.3 Input Validation Best Practices

```php
<?php
// Validate input length before regex
function validateEmail(string $email): bool {
    // Reject unreasonably long input
    if (strlen($email) > 254) {  // RFC max email length
        return false;
    }
    
    // Use simpler validation or built-in filter
    return filter_var($email, FILTER_VALIDATE_EMAIL) !== false;
}

// Prefer character class restrictions over complex patterns
function validateUsername(string $username): bool {
    // Length check first
    if (strlen($username) < 3 || strlen($username) > 20) {
        return false;
    }
    
    // Simple character class (fast, safe)
    return (bool) preg_match('/^[a-zA-Z0-9_]+$/', $username);
}
```

## 7. Array Declaration and Structure

Arrays are PHP's most versatile data structure, serving as both indexed lists and associative maps. Understanding array creation and structure is foundational for effective PHP programming.

### 7.1 Array Declaration Methods

PHP provides multiple syntaxes for creating arrays, with the short array syntax (`[]`) being preferred in modern code.

#### 7.1.1 Basic Declaration Syntax

```php
<?php
// Modern short syntax (PHP 5.4+)
$fruits = ['apple', 'banana', 'cherry'];

// Legacy long syntax
$colors = array('red', 'green', 'blue');

// Associative arrays
$person = [
    'name' => 'Alice',
    'age' => 30,
    'email' => 'alice@example.com'
];

// Mixed keys (not recommended)
$mixed = [
    0 => 'first',
    'key' => 'second',
    1 => 'third'
];

// Empty array
$empty = [];
```

#### 7.1.2 Multi-Dimensional Arrays

```php
<?php
// 2D array (matrix)
$matrix = [
	
    [1, 2, 3], # 0
    [4, 5, 6], # 1 -> $matrix[1] : <- this row
    [7, 8, 9]  # 2  |
    #0  1 (2) -----/
];

echo $matrix[1][2];  // 6
# []


// Nested associative arrays
$users = [
    'alice' => [
        'email' => 'alice@example.com',
        'role' => 'admin',
        'preferences' => [
            'theme' => 'dark',
            'language' => 'en'
        ]
    ],
    'bob' => [
        'email' => 'bob@example.com',
        'role' => 'user',
        'preferences' => [
            'theme' => 'light',
            'language' => 'fr'
        ]
    ]
];

echo $users['alice']['preferences']['theme'];  // "dark"

// Database result representation
$products = [
    ['id' => 1, 'name' => 'Laptop', 'price' => 999.99],
    ['id' => 2, 'name' => 'Mouse', 'price' => 29.99],
    ['id' => 3, 'name' => 'Keyboard', 'price' => 79.99]
];
```

### 7.2 Dynamic Array Construction

Arrays can be built dynamically using various assignment patterns.

#### 7.2.1 Appending Elements

```php
<?php
// Auto-incremented numeric keys
$numbers = [];
$numbers[] = 10;  // Index 0
$numbers[] = 20;  // Index 1
$numbers[] = 30;  // Index 2

// Explicit key assignment
$settings = [];
$settings['debug'] = true;
$settings['timeout'] = 30;
$settings['cache'] = false;

// Building arrays in loops
$squares = [];
for ($i = 1; $i <= 10; $i++) {
    $squares[] = $i * $i;
}

// Conditional array building
$errors = [];
if (!$isValid) {
    $errors[] = "Validation failed";
}
if ($age < 18) {
    $errors[] = "Must be 18 or older";
}
```

#### 7.2.2 Array Builder Pattern

```php
<?php
class QueryBuilder {
    private array $conditions = [];
    private array $bindings = [];
    
    public function where(string $column, mixed $value): self {
        $this->conditions[] = "$column = ?";
        $this->bindings[] = $value;
        return $this;
    }
    
    public function build(): array {
        return [
            'sql' => 'SELECT * FROM users WHERE ' . implode(' AND ', $this->conditions),
            'bindings' => $this->bindings
        ];
    }
}

$query = (new QueryBuilder())
    ->where('status', 'active')
    ->where('age', 18)
    ->build();
```

### 7.3 Array Type Juggling and Key Coercion

PHP automatically converts certain key types, which can lead to unexpected behavior.

#### 7.3.1 Key Type Conversion Rules

```php
<?php
// String numbers become integers
$arr = [];
$arr['0'] = 'zero';
$arr['1'] = 'one';
$arr[2] = 'two';

print_r($arr);
// Array ( [0] => zero [1] => one [2] => two )

// Floats are truncated to integers
$arr = [];
$arr[1.5] = 'a';
$arr[1.9] = 'b';  // Overwrites previous! Both use key 1

print_r($arr);
// Array ( [1] => b )

// Booleans convert to integers
$arr = [];
$arr[true] = 'true';   // Key becomes 1
$arr[false] = 'false'; // Key becomes 0

print_r($arr);
// Array ( [1] => true [0] => false )

// null becomes empty string
$arr = [null => 'value'];
print_r($arr);
// Array ( [] => value )
```

#### 7.3.2 Security Implications

```php
<?php
// VULNERABILITY: Type juggling can break access controls
$permissions = [
    'admin' => true,
    'user' => false
];

$role = $_GET['role'];  // Attacker passes role=1

// Dangerous: "1" becomes integer 1, which might not exist
if ($permissions[$role] ?? false) {
    // Access granted!
}

// SAFE: Validate key type and existence explicitly
if (is_string($role) && isset($permissions[$role]) && $permissions[$role] === true) {
    // Properly validated
}

// Array key collision attacks
$votes = [];
foreach ($_POST['votes'] as $item => $count) {
    // If $item is "1" and "1.0", both become key 1
    $votes[$item] = $count;  // Later value wins
}

// Protection: Validate input types
foreach ($_POST['votes'] as $item => $count) {
    if (!is_string($item) || !is_numeric($count)) {
        continue;  // Skip invalid input
    }
    $votes[$item] = (int)$count;
}
```

### 7.4 Array Spread Operator

PHP 7.4 introduced the array spread operator (`...`) for unpacking arrays.

```php
<?php
// Basic spreading
$arr1 = [1, 2, 3];
$arr2 = [4, 5, 6];
$combined = [...$arr1, ...$arr2];  // [1, 2, 3, 4, 5, 6]

// Spreading with additional elements
$extended = [0, ...$arr1, 99];  // [0, 1, 2, 3, 99]

// Function arguments
function sum(...$numbers) {
    return array_sum($numbers);
}

$values = [1, 2, 3, 4, 5];
echo sum(...$values);  // 15

// Limitation: Only integer keys are retained
$assoc = ['a' => 1, 'b'
```