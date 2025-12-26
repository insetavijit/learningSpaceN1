### **Overview**

Mastering variables, data types, and operators is the essential foundation for writing reliable PHP. These concepts control how data is stored, transformed, and interpreted throughout a program. Understanding how types behave, how operators interact, and how scope and memory work prevents subtle bugs and ensures accurate calculations, stable application state, and confident debugging—critical skills for real-world development and scalable backend systems.

### **Topics to Learn — Variables, Types & Operators**

| **Topic**                      | **Brief Description**                                                        |
| ------------------------------ | ---------------------------------------------------------------------------- |
| **Variable Declaration**       | Creating named storage locations for values using `$variableName`.           |
| **Naming Conventions**         | Clear, meaningful names using camelCase for readability and maintainability. |
| **Data Types**                 | Understanding int, float, string, bool, array, object, null, resource.       |
| **Type Juggling**              | Automatic type conversion that PHP performs silently during operations.      |
| **Strict Typing**              | `declare(strict_types=1)` to enforce safer and predictable type behavior.    |
| **Constants**                  | Using `const` or `define()` for values that must never change.               |
| **Operators**                  | Arithmetic, assignment, comparison, logical, and string concatenation tools. |
| **Operator Precedence**        | Execution order that determines calculation results (`*` before `+`).        |
| **Scope**                      | Where a variable exists—global, local, static, and closure context.          |
| **Copy-on-Write**              | Optimization where PHP copies values only when modified.                     |
| **Casting & Conversion**       | `(int)`, `(float)`, `(string)` type transformations.                         |
| **Debugging Tools**            | `var_dump`, `print_r`, `debug_zval_dump` to inspect values and types.        |
| **[[Garbage Collection.spl]]** | Memory cleanup when variables go out of scope or are `unset()`.              |

> [!quote] Rasmus Lerdorf (Creator of PHP) "PHP is about getting shit done — but only the people who truly master variables, types and operators get to do it elegantly, fast, and without ever being surprised."
> 
## life of a ver
```cs
Variable Flow Path:
└── [Declaration / Allocation Stage]
      • Variable name created and registered in symbol table
      • Memory allocated to hold initial value (zval structure)
      • Example: $count = 10;
      ↓
└── [Value Assignment & Typing]
      • Assigned value determines its data type (int, float, string, object..)
      • Weak typing may trigger implicit type conversion (type juggling)
      • strict_types enables explicit type enforcement
      ↓
└── [Usage & Read Operations]
      • Value accessed or used in expressions, conditions, or calculations
      • Example: $tax = $count * 0.18;
      • Access may trigger copy-on-write behavior internally
      ↓
└── [Mutation / Update Stage]
      • Operators modify stored value (arithmetic, string, comparison, logical)
      • Reassignment may change type or replace underlying memory reference
      • Example: $count += 5;
      ↓
└── [Scope Boundary Evaluation]
      • Variable visibility depends on scope (global, local, function, closure)
      • Leaves scope when block/function ends unless captured or globalized
      • Example: function() { $x = 10; } → removed after execution
      ↓
└── [Garbage Collection & Memory Release]
      • Reference counter reaches zero → considered unused
      • Cyclic references handled by GC root scanning
      • unset($var) forces early cleanup (optional manual release)
      ↓
System Output: **Final Value Used / Output Generated**
      • Returned, printed, processed, stored or passed to another function
      • Eventually removed at script termination (end of request lifecycle)
      
```



---

## 01. Variable Declaration

**Core Concept:** Creating named storage locations for values using `$variableName` syntax.
### The Reality Behind Declaration

When you write `$price = 99.99;`, PHP doesn't just "store a number." Here's what actually happens:

```
PHP Engine Flow:
1. Symbol table lookup → register "price" as identifier
2. Allocate zval structure in heap memory
3. Set type flag (IS_DOUBLE/float)
4. Store value or pointer to value
5. Initialize reference counter to 1
6. Set is_reference flag to false
```

The **zval (Zend Value)** is PHP's universal container:

```c
struct _zval_struct {
    zend_value value;        // The actual data
    uint32_t type_info;      // Type + flags
    uint32_t u2;             // Cache slot / extra info
};
```

### Why Variable Naming Is Your Highest-Leverage Skill
**The 60% Rule**: Studies of production PHP codebases show that **60% of debugging time** is spent on confusion caused by poor variable names. Not algorithms. Not architecture. Names.

### 2025 Best Practices + Real Cost of Violation

| Rule                     | Why It Matters        | ❌ Violation                      | ✅ Correct                                    |
| ------------------------ | --------------------- | -------------------------------- | -------------------------------------------- |
| **Domain-rich names**    | Code documents itself | `$x`, `$temp`, `$data`           | `$orderSubtotal`, `$userEmail`               |
| **PSR-12 camelCase**     | Tool compatibility    | `$user_name`, `$order_total`     | `$userName`, `$orderTotal`                   |
| **Boolean prefixes**     | Semantic clarity      | `$active`, `$valid`, `$ready`    | `$isActive`, `$hasPermission`, `$canEdit`    |
| **Counter prefixes**     | Intent signaling      | `$retries`, `$users`             | `$retryCount`, `$totalUsers`, `$userQty`     |
| **Numeric separators**   | Visual parsing        | `1000000`, `250000`              | `1_000_000`, `250_000`                       |
| **Narrowest scope**      | Memory + isolation    | Global `$temp` reused 5 times    | Function-scoped, single-purpose              |
| **One job per variable** | Predictability        | `$result` morphs through 4 types | `$userRecord`, `$userName`, `$formattedName` |

### Before → After Refactor (Identical Logic, 10x Clarity)

```php
// ❌ Before – passes all tests, fails all humans
$a = 10847; $g = false; $t = 2499.00; $d = $t*0.2; $tx=$t*($g?0.12:0.18); $f=$t-$d+$tx;

// ✅ After – instantly readable by any developer
declare(strict_types=1);

$customerId         = 10847;
$isGuestCheckout    = false;
$orderSubtotal      = 2_499.00;

$discountAmount     = $orderSubtotal * 0.20;
$taxRate            = $isGuestCheckout ? 0.12 : 0.18;
$taxAmount          = $orderSubtotal * $taxRate;

$finalAmount        = $orderSubtotal - $discountAmount + $taxAmount;
$orderReference     = sprintf("ORD-%s-%d", date('Ymd'), $customerId);
```

### Anti-Patterns That Silently Destroy Codebases

```php
// ❌ Mystery abbreviations (cognitive overhead)
$ord_sub_tot = 100;  // "tot" = total? tooltip? What?
$usr_addr_ln1 = ""; // Unreadable even to author after 2 weeks

// ❌ Hungarian notation (obsolete since PHP 7 type hints)
$strName = "John";   // Redundant: function getName(): string
$intCount = 5;       // Redundant: $count is clearly int from context

// ❌ Variable reuse (the silent production killer)
$result = fetchUser($id);        // object{id, name, email}
$result = $result['email'];      // string
$result = strtoupper($result);   // UPPERCASE STRING
// Same variable, 3 different types → xdebug shows wrong data

// ✅ Correct: Each transformation gets its own name
$userRecord     = fetchUser($id);
$userEmail      = $userRecord['email'];
$displayEmail   = strtoupper($userEmail);
```

---

## 02. Naming Conventions

**Brief:** Consistent naming patterns that reduce cognitive load and enable tooling.

### The 5-Second Decision Tree

Use this every single time you type `$`:

```
1. Can a junior dev understand this in < 5 seconds?
   ├─ NO → Rename immediately
   └─ YES → Continue

2. Is it a boolean?
   ├─ YES → Prefix: is/has/can/should/will
   │        Examples: $isVerified, $hasAccess, $canEdit
   └─ NO → Continue

3. Is it a collection?
   ├─ YES → Use plural noun
   │        $users (not $user), $orderItems (not $item)
   └─ NO → Continue

4. Does it represent money, time, or measurement?
   ├─ YES → Include unit in name
   │        $priceInCents, $durationSeconds, $distanceKm
   └─ NO → Continue

5. Still unsure?
   └─ Spell it out completely (abbreviations age like milk)
      $temperature (not $temp)
      $configuration (not $config)
```

### The Unit Suffix Pattern (Financial/Scientific Code)

```php
// ❌ Ambiguous – what currency? what precision?
$price = 99.99;
$amount = 1500;

// ✅ Explicit – no assumptions needed
$priceInCents = 9999;        // int, no float precision issues
$amountInDollars = 15.00;    // clearly USD, 2 decimal places
$durationInSeconds = 3600;   // int, not "1 hour"
$distanceInMeters = 1_500;   // SI units explicit

// ✅ Best: Use value objects (PHP 8.1+)
readonly class Money {
    public function __construct(
        public int $amountInCents,
        public string $currency
    ) {}
}
```

### Boolean Naming That Reads Like English

```php
// ❌ Unclear intent
if ($user) { }           // Exists? Active? Logged in?
if ($verified) { }       // Email? Phone? ID? All three?
if ($complete) { }       // Payment? Shipment? Profile?

// ✅ Crystal clear
if ($userExists) { }
if ($hasVerifiedEmail) { }
if ($isPaymentComplete) { }
if ($canEditPost) { }
if ($shouldSendNotification) { }
if ($willExpireSoon) { }
```

---

## 03. Data Types

**Brief:** PHP's 8 fundamental types that determine how values are stored and manipulated.

### The Type Hierarchy (Memory Layout Perspective)

```
PHP Types (by memory storage)
│
├── Scalar (stored directly in zval)
│   ├── int      → 8 bytes (64-bit), range: ±9.2 quintillion
│   ├── float    → 8 bytes (IEEE 754 double precision)
│   ├── string   → pointer + length (stored in heap)
│   └── bool     → 1 byte (internally, optimized to bit flag)
│
├── Compound (stored as pointers)
│   ├── array    → HashTable structure (ordered hashmap)
│   └── object   → zend_object structure + properties table
│
└── Special
    ├── null     → special sentinel value (no memory allocated)
    └── resource → integer handle to external resource (file, DB, socket)
```

### Modern Type Declaration (2025 Standard)

|Type|Example|Modern Declaration (PHPStan/Psalm)|Why Use It|
|---|---|---|---|
|**int**|`$id = 10847;`|`/** @var positive-int */`|Catches negative IDs at analysis time|
|**float**|`$price = 2_499.99;`|`/** @var float<0, max> */`|Enforces non-negative prices|
|**string**|`$email = 'hi@x.com';`|`/** @var non-empty-string */`|Prevents empty string bugs|
|**bool**|`$isActive = true;`|`/** @var true */` when always true|Narrows type for better inference|
|**array**|`$items = ['a', 'b'];`|`/** @var list<string> */`|List vs assoc array distinction|
|**array**|`$config = ['key' => 'val'];`|`/** @var array<string, mixed> */`|Key-value type safety|
|**object**|`$user = new User();`|`User $user`|Native PHP type hint (best)|
|**null**|`$middle = null;`|`?string` or `string\|null`|Null safety in type system|
|**resource**|`$file = fopen(...);`|❌ Avoid → Use `SplFileObject`|Resources are legacy, objects are modern|

### Integer: Platform Limits & Overflow Behavior

```php
// Platform-dependent ranges (always check!)
echo PHP_INT_MAX;  // 64-bit: 9_223_372_036_854_775_807
echo PHP_INT_MIN;  // 64-bit: -9_223_372_036_854_775_808

// Overflow converts to float (SILENTLY!)
$overflow = PHP_INT_MAX + 1;
var_dump($overflow);  // float(9.2233720368548E+18)

// Different number bases (all compile to same int)
$decimal = 42;
$hex = 0x2A;        // Hexadecimal
$octal = 052;       // Octal (deprecated: use 0o52 in PHP 8.1+)
$binary = 0b101010; // Binary

var_dump($decimal === $hex);  // true
```

### Float: Precision Traps & Solutions

```php
// ❌ The infamous precision problem
var_dump(0.1 + 0.2 === 0.3);  // false (binary representation issue)

// ❌ Never compare floats with ===
$a = 0.1 + 0.2;
$b = 0.3;
if ($a === $b) { }  // Unreliable!

// ✅ Epsilon comparison
$epsilon = 0.00001;
if (abs($a - $b) < $epsilon) {
    // Considered equal
}

// ✅ Best: Use integers for money
$priceInCents = 999;  // $9.99
$taxInCents = 180;    // $1.80
$totalInCents = $priceInCents + $taxInCents;  // Perfect precision!

// ✅ Or use BCMath for arbitrary precision
$price = '99.99';
$tax = bcmul($price, '0.18', 2);  // String math, exact
```

### String: Interpolation & Performance

```php
// Single quotes (literal, faster)
$literal = 'Hello $name';  // Output: Hello $name

// Double quotes (variables parsed, slightly slower)
$name = "Priya";
$greeting = "Hello $name";  // Output: Hello Priya

// Complex expressions (use curly braces)
$user = ['name' => 'Priya', 'role' => 'admin'];
echo "User {$user['name']} is an {$user['role']}";

// Heredoc (multiline with parsing)
$html = <<<HTML
<div class="user">
    <h1>Welcome $name</h1>
</div>
HTML;

// Nowdoc (multiline literal, like single quotes)
$code = <<<'CODE'
function test() {
    return $var;  // $var NOT parsed
}
CODE;

// Performance tip: Concatenation vs interpolation
// For simple cases, they're similar
// For loops, concatenation is faster:
$result = '';
foreach ($items as $item) {
    $result .= $item;  // Faster than "$result$item"
}
```

### Array: The Swiss Army Knife (Ordered HashMap)

```php
// PHP arrays are actually ordered hashmaps (since PHP 7.0)
// Insertion order is preserved!

// Indexed array (list)
$fruits = ["apple", "banana", "orange"];
/** @var list<string> $fruits */  // PHPStan: sequential keys 0,1,2...

// Associative array (dictionary)
$user = [
    "id" => 10847,
    "name" => "Priya",
    "role" => "admin"
];
/** @var array<string, string|int> $user */

// Mixed keys (discouraged but possible)
$mixed = [
    0 => "first",
    "key" => "second",
    1 => "third"
];

// Array unpacking (PHP 7.4+)
$defaults = ["theme" => "dark", "lang" => "en"];
$userPrefs = ["lang" => "fr"];
$config = [...$defaults, ...$userPrefs];  // ["theme"=>"dark", "lang"=>"fr"]

// Destructuring
[$first, $second] = $fruits;  // $first="apple", $second="banana"
["name" => $userName, "role" => $userRole] = $user;
```

---

## 04. Type Juggling

**Brief:** PHP's automatic type conversion system – the source of 70% of beginner bugs.

### The Silent Type Conversion Engine

PHP tries to "help" by converting types automatically. This is **almost never what you want**:

```php
// String to int (numeric string extraction)
"5" + 3;              // 8 (string → int)
"10 apples" + 5;      // 15 (extracts 10, ignores "apples")
"apples" + 5;         // 5 (no number found → 0)

// Boolean conversions in arithmetic
true + 1;             // 2
false + 1;            // 1
true * 5;             // 5

// The loose comparison nightmare
0 == "apple";         // true ("apple" → 0)
0 == "0";             // true
0 == false;           // true
"" == false;          // true
null == false;        // true
[] == false;          // true

// Even worse: loose comparison with arrays
[] == 0;              // true
[1] == 1;             // true (!!)
["1"] == 1;           // true (!!!)

// String to bool (everything truthy except...)
(bool) "false";       // true (non-empty string!)
(bool) "0";           // false (special case)
(bool) "";            // false
```

### Why Type Juggling Exists

PHP was designed in 1994 for form processing. Back then:

```php
// $_GET parameters are always strings
$page = $_GET['page'];  // "5" (string)
$items = $page * 10;    // 50 (automatic conversion)
```

This was convenient for quick scripts. For modern applications, it's a liability.

### Falsy Values (The Complete List)

Only **7 values** are considered false:

```php
// The 7 falsy values
false         // boolean false
0             // integer zero
0.0           // float zero
""            // empty string
"0"           // string containing single zero
[]            // empty array
null          // null value

// EVERYTHING else is truthy, including:
"false"       // non-empty string!
"0.0"         // string (not numeric zero)
-1            // negative numbers
[[]]          // array containing empty array
new stdClass  // any object, even empty
```

---

## 05. Strict Typing

**Brief:** `declare(strict_types=1);` eliminates type juggling for function arguments.

### The Modern PHP Type System

```php
<?php
declare(strict_types=1);  // FILE-SCOPE directive (must be first line)

// Without strict types (legacy behavior)
function calculate(float $price): float {
    return $price * 1.18;
}
calculate("100");  // ✅ Works (string "100" → float 100.0)

// With strict types (modern, safe)
declare(strict_types=1);
function calculate(float $price): float {
    return $price * 1.18;
}
calculate("100");  // ❌ TypeError: must be of type float, string given
calculate(100.0);  // ✅ Works
```

### Strict Types: Scope & Limitations

**Critical: `strict_types` is FILE-scoped, not function-scoped**

```php
// file1.php
<?php
declare(strict_types=1);

function add(int $a, int $b): int {
    return $a + $b;
}

// file2.php (no strict_types)
<?php
require 'file1.php';

add("5", "3");  // ✅ Works! (file2.php doesn't have strict_types)
                // Caller's file determines strictness, not callee's
```

### Full Type Declaration Syntax (PHP 8.2)

```php
<?php
declare(strict_types=1);

class Order {
    // Property types (PHP 7.4+)
    public int $id;
    public float $total;
    private bool $isPaid = false;
    
    // Constructor property promotion (PHP 8.0+)
    public function __construct(
        public readonly string $orderNumber,  // Readonly (PHP 8.1+)
        public int|float $amount,              // Union types (PHP 8.0+)
        public ?string $couponCode = null      // Nullable
    ) {}
    
    // Return type + nullable
    public function getCoupon(): ?string {
        return $this->couponCode;
    }
    
    // Never return type (PHP 8.1+)
    public function terminate(): never {
        throw new Exception("Order cancelled");
    }
    
    // Void return
    public function logPayment(): void {
        error_log("Payment processed");
    }
    
    // Mixed type (accepts anything)
    public function processData(mixed $data): mixed {
        return $data;
    }
}

// Intersection types (PHP 8.1+)
interface Loggable {}
interface Cacheable {}

function process(Loggable&Cacheable $object): void {
    // $object must implement BOTH interfaces
}

// Disjunctive normal form (PHP 8.2+)
function handle((Loggable&Cacheable)|null $obj): void {
    // $obj is (Loggable AND Cacheable) OR null
}
```

---

## 06. Constants & Immutable Values

**Brief:** Values that must never change during execution.

### The 4 Ways to Define Constants in Modern PHP

|Method|Scope|Type Safety|Runtime Definition|Best For|
|---|---|---|---|---|
|**`const`**|Class/namespace|✅ Yes|❌ No|Class constants, enum cases|
|**`define()`**|Global|❌ No|✅ Yes|Legacy code, dynamic constants|
|**Enum** (8.1+)|Type-safe set|✅✅ Best|❌ No|Related constants (status, role, etc.)|
|**`readonly`** (8.1+)|Object property|✅ Yes|✅ Yes|Immutable value objects|

### Class Constants (Preferred for Modern Code)

```php
class PaymentProcessor {
    // Typed constants (PHP 8.3+)
    public const string API_VERSION = 'v2';
    public const int TIMEOUT_SECONDS = 30;
    public const float VAT_RATE = 0.18;
    
    // Visibility modifiers (PHP 7.1+)
    private const string SECRET_KEY = 'xyz123';
    protected const int MAX_RETRIES = 3;
    
    public function process(): void {
        echo self::API_VERSION;  // Access within class
    }
}

echo PaymentProcessor::API_VERSION;  // Access from outside
// echo PaymentProcessor::SECRET_KEY;  // Fatal error: private
```

### Enums: Type-Safe Constant Sets (PHP 8.1+)

```php
// Backed enum (has underlying value)
enum OrderStatus: string {
    case Pending = 'pending';
    case Processing = 'processing';
    case Completed = 'completed';
    case Cancelled = 'cancelled';
    
    // Methods allowed!
    public function isFinalized(): bool {
        return match($this) {
            self::Completed, self::Cancelled => true,
            default => false,
        };
    }
    
    public function color(): string {
        return match($this) {
            self::Pending => 'yellow',
            self::Processing => 'blue',
            self::Completed => 'green',
            self::Cancelled => 'red',
        };
    }
}

// Usage
function updateOrder(int $id, OrderStatus $status): void {
    // Type safety: can't pass invalid status
    if ($status->isFinalized()) {
        // ...
    }
}

updateOrder(123, OrderStatus::Completed);  // ✅
// updateOrder(123, 'completed');  // ❌ TypeError
// updateOrder(123, OrderStatus::Invalid);  // ❌ Compile error

// Get all cases
$allStatuses = OrderStatus::cases();

// Get from value
$status = OrderStatus::from('pending');  // Throws if invalid
$status = OrderStatus::tryFrom('invalid');  // Returns null if invalid
```

### Readonly Classes: Immutable Value Objects (PHP 8.2+)

```php
// Entire class is readonly
readonly class Money {
    // All properties automatically readonly
    public function __construct(
        public int $amountInCents,
        public string $currency
    ) {}
    
    // Can have methods, just can't modify properties
    public function add(Money $other): Money {
        if ($this->currency !== $other->currency) {
            throw new InvalidArgumentException('Currency mismatch');
        }
        
        return new Money(
            $this->amountInCents + $other->amountInCents,
            $this->currency
        );
    }
    
    public function format(): string {
        $dollars = $this->amountInCents / 100;
        return sprintf('%s %.2f', $this->currency, $dollars);
    }
}

$price = new Money(9999, 'USD');
$tax = new Money(1800, 'USD');
$total = $price->add($tax);

echo $total->format();  // USD 117.99
// $total->amountInCents = 5000;  // ❌ Error: readonly property
```

### Global Constants: Legacy, But Sometimes Necessary

```php
// define() - runtime definition
define('APP_VERSION', '2.1.0');
define('MAX_UPLOAD_SIZE', 1024 * 1024 * 5);  // 5MB

// Can be defined conditionally
if (!defined('DEBUG')) {
    define('DEBUG', false);
}

// Case-insensitive (deprecated, don't use)
define('API_KEY', 'secret', true);  // 3rd param = case-insensitive
```

---

## 07. Operators Cheat Sheet  
(Every single operator you will ever need in modern PHP, with exact behavior, gotchas, and best-practice usage)

### Arithmetic Operators
| Operator | Name              | Example                | Result       | Critical Notes |
|----------|-------------------|------------------------|--------------|----------------|
| `+`      | Addition          | `5 + 3`                | `8`          | Also array union `+` (left-preference) |
| `-`      | Subtraction       | `5 - 3`                | `2`          | |
| `*`      | Multiplication    | `5 * 3`                | `15`         | |
| `/`      | Division          | `10 / 3`               | `3.333…`     | **Always returns float** (even `10 / 2 → float(5)`) |
| `%`      | Modulus           | `10 % 3`               | `1`          | Sign follows dividend: `-10 % 3 → -1` |
| `**`     | Exponentiation    | `2 ** 8`               | `256`        | Right-associative: `2 ** 3 ** 2 → 2 ** (3 ** 2) = 512` |
| `-`      | Unary negation    | `-$x`                  | negative     | |
| `+`      | Unary plus        | `+$x`                  | (int/float)  | Forces numeric context |

```php
intdiv(17, 5);     // int(3)  → integer division (PHP 7+)
2 ** 3 ** 2;       // 512, not 64
```

### Assignment Operators
| Operator | Example            | Equivalent                | Best Use Case |
|----------|--------------------|---------------------------|---------------|
| `=`      | `$x = 10`          | –                         | Initial assignment |
| `+=`     | `$x += 5`          | `$x = $x + 5`             | Accumulate |
| `-=`     | `$x -= 5`          | `$x = $x - 5`             | Decrease |
| `*=`     | `$x *= 4`          | `$x = $x * 4`             | Scale |
| `/=`     | `$x /= 2`          | `$x = $x / 2`             | Halving |
| `%=`     | `$x %= 3`          | `$x = $x % 3`             | Cycling |
| `**=`    | `$x **= 3`         | `$x = $x ** 3`            | Power growth |
| `.=`     | `$x .= "!"`        | `$x = $x . "!"`           | String building |
| `??=`    | `$x ??= 100`       | `$x = $x ?? 100`          | Lazy init (PHP 7.4+) |
| `&= |= ^= <<= >>=` | Bitwise compound | Rarely needed outside low-level code |

```php
$config['theme'] ??= 'dark';     // Only sets if null or unset
$cache ??= Cache::expensiveLoad(); // Runs once
```

### Comparison Operators – **Never use loose (`==`) in 2025**
| Operator | Name               | Type Juggling? | Example          | Result | Verdict |
|----------|--------------------|----------------|------------------|--------|--------|
| `==`     | Equal              | Yes            | `5 == "5"`       | `true` | Never use |
| `===`    | Identical          | No             | `5 === "5"`      | `false`| Always use |
| `!=`     | Not equal          | Yes            | `5 != "5"`       | `false`| Never use |
| `!==`    | Not identical      | No             | `5 !== "5"`      | `true` | Always use |
| `<>`     | Not equal (old)    | Yes            | –                | –      | Deprecated style |
| `<` `>` `<=` `>=` | Standard comparisons | No (after juggling) | –      | –      | Safe with strict types |
| `<=>`    | Spaceship          | No             | `5 <=> 3`        | `1`    | Perfect for sorting |

```php
// Spaceship in action (PHP 7+)
usort($users, fn($a, $b) => $a->score <=> $b->score);
// Or descending:
usort($users, fn($a, $b) => $b->createdAt <=> $a->createdAt);
```

### Logical Operators
| Operator | Name     | Short-circuits? | Precedence | Example                       | Typical Use |
|----------|----------|----------------|------------|-------------------------------|-------------|
| `!`      | NOT      | –              | Highest    | `!$active`                    | Inversion |
| `&&`     | AND      | Yes            | High       | `$user && $user->isAdmin()`   | Guard clauses |
| `||`     | OR       | Yes            | High       | `$input || $default`          | Fallback |
| `and`    | AND (low precedence) | Yes      | Low        | Rarely used |
| `or`     | OR (low precedence)  | Yes      | Low        | Rarely used |
| `xor`    | XOR      | No             | Low        | Exactly one true |

```php
// Classic guard pattern
if (!($user instanceof User)) {
    return;
}
// Short-circuit saves calls
$isReady = $dbConnected && $cacheWarmed && $queueRunning;
```

### String Operators
| Operator | Name                  | Example                         | Performance Note |
|----------|-----------------------|---------------------------------|------------------|
| `.`      | Concatenation         | `"Hello " . $name`              | Fastest in loops |
| `.=`     | Concat assignment     | `$log .= $line . PHP_EOL;`      | Avoid in hot loops with interpolation |
| `{}`     | Interpolation         | `"User {$id} logged in"`        | Slightly slower than `.` |

```php
// Best practices 2025
// 1. Simple → interpolation
echo "Welcome back, {$user->name}!";

// 2. Complex or loops → concatenation or sprintf
$result = '';
foreach ($items as $i) {
    $result .= $i->id . ':' . $i->name . "\n";
}

// 3. Formatting → sprintf or strtr
echo sprintf('Price: ₹%.2f (tax %.0f%%)', $price, $tax * 100);
```

### Null Coalescing Operator Chain (PHP 7+ – your new best friend)
| Operator | Name                        | Example                                    | Equivalent |
|----------|-----------------------------|--------------------------------------------|------------|
| `??`     | Null coalescing             | `$name = $input['name'] ?? $user->name ?? 'Guest';` | Nested isset |
| `??=`    | Null coalescing assignment  | `$settings['timeout'] ??= 30;`             | Lazy default |

```php
// Real-world magic
$country = $input['country'] 
    ?? $user->profile->country 
    ?? $request->ipGeo()->country 
    ?? 'US';
```

### Bitwise Operators (rare but still appear in flags, permissions, UUIDs)
| Operator | Name           | Example                     | Common Use |
|----------|----------------|-----------------------------|----------|
| `&`      | AND            | `$permissions & PERM_READ`  | Check flag |
| `|`      | OR             | `$flags |= FLAG_ACTIVE`     | Set flag |
| `^`      | XOR            | `$a ^ $b`                   | Toggle |
| `~`      | NOT            | `~$mask`                    | Invert |
| `<<`     | Left shift     | `1 << 3 → 8`                | Fast ×2^n |
| `>>`     | Right shift    | `16 >> 2 → 4`               | Fast ÷2^n |

```php
// Example: permission system
define('PERM_READ', 1 << 0);  // 1
define('PERM_WRITE', 1 << 1); // 2
define('PERM_DELETE', 1 << 2); // 4

$role = PERM_READ | PERM_WRITE;
if ($role & PERM_WRITE) { /* can write */ }
```

## 08. Operator Precedence
### **Overview**

Operator precedence determines **which part of an expression PHP evaluates first** when multiple operators appear together. Most unexpected bugs in conditions, math expressions, string concatenations, and nested ternaries come from assuming the wrong evaluation order.  
This cheat sheet ranks every operator from highest to lowest priority, explains associativity, and highlights real-world traps that hurt even experienced developers.  
Master this once — and write expressions that behave exactly the way you intend them to.

|Level|Operators (highest → lowest)|Associativity|Rule-of-Thumb|Dangerous Trap Example|
|---|---|---|---|---|
|1|`!` `~` `+` `-` `(int)` `(float)` `(string)` `(array)` `(object)` `(bool)` `@`|Right|Unary operators|`!$active` → NOT has highest precedence|
|2|`**`|Right|Exponentiation|`2 ** 3 ** 2` → 512 (not 64)|
|3|`*` `/` `%`|Left|Multiply/Divide/Mod|`10 + 5 * 2` → 20|
|4|`+` `-` `.`|Left|Add/Subtract/Concat|`"a" . "b" . "c"` → "abc"|
|5|`<<` `>>`|Left|Bitshift|Rare in business logic|
|6|`<` `<=` `>` `>=`|Non-associative|Comparisons|`5 < 10 < 3` → **syntax error**|
|7|`==` `!=` `===` `!==` `<>` `<=>`|Non-associative|Equality|`5 == 5 == true` → **syntax error**|
|8|`&`|Left|Bitwise AND||
|9|`^`|Left|Bitwise XOR||
|10|`|`|Left|Bitwise OR|
|11|`&&`|Left|Logical AND|`true && false && true` → false|
|12|`||`|Left|
|13|`??`|Right|Null coalescing|`$a ?? $b ?? $c` → checks left-to-right|
|14|`? :` (ternary)|Right|Ternary|`true ? 'a' : false ? 'b' : 'c'` → "a"|
|15|`= += -= *= /= .= ??= **=` etc.|Right|Assignment|`$a = $b = 5` → both become 5|
|16|`and`|Left|Low-precedence AND|Rarely used|
|17|`xor`|Left|Low-precedence XOR|Rarely used|
|18|`or`|Left|Low-precedence OR|Classic trap below|
### **The 5 Most Common Precedence Bugs (and how to kill them forever)**

```php
// Trap 1 – Logical vs bitwise
$a || $b && $c     →  $a || ($b && $c)    // && wins
$a or $b = false   →  ($a or $b) = false  // assignment wins → syntax error!

// Trap 2 – Concatenation vs addition
$x = 5 . 3 + 2;    →  "53" + 2 → 55        // . wins over +
$x = 5 + 3 . 2;    →  5 + "32" → 37

// Trap 3 – Ternary nesting
$isAdmin ? 'admin' : $isEditor ? 'editor' : 'guest'
→ $isAdmin ? 'admin' : ($isEditor ? 'editor' : 'guest')   // correct

// Trap 4 – Null coalescing + logical
$a ?? $b || $c     →  ($a ?? $b) || $c     // ?? wins

// Trap 5 – The infamous "or die()" pattern (still seen in legacy code)
mysqli_connect(...) or die("Failed");   // die() runs if connect FALSE
// Correct modern way:
$conn = mysqli_connect(...) or throw new DatabaseException("Failed");
```

### **2025 Golden Rule – Never Trust Precedence**

### **Always add parentheses when mixing ≥2 operator types.**

```php
// Crystal-clear (zero mental overhead)
$total = ($price + $tax) * $quantity;
$isReady = ($user && $user->isVerified()) || $isGuest;
$theme = $input['theme'] ?? $user->preference ?? 'dark';

$result = $a ** $b + $c * $d;           // still ambiguous → add parentheses!
$result = ($a ** $b) + ($c * $d);       // perfect
```

---
## 09. Scope in PHP

#### **Overview**
Scope determines **where a variable can be accessed** within a program and **how long it remains in memory**.  
It’s a protective system that avoids naming conflicts, controls accessibility, and keeps code structured, safe, and easy to maintain.
#### **Scope Types — Quick Reference Table**

| **Scope Type** | **Where It Exists**               | **Lifetime**                                 | **Used For**                                        |
| -------------- | --------------------------------- | -------------------------------------------- | --------------------------------------------------- |
| **Local**      | Inside a function                 | Exists only during that function’s execution | Short-term calculations and internal logic          |
| **Global**     | Outside any function              | Available throughout the entire script       | Shared configuration data & program-wide values     |
| **Static**     | Inside a function                 | Persists across multiple calls               | Counters, caching, remembering previous state       |
| **Closure**    | Inside nested/anonymous functions | Controlled by parent context                 | Safe variable sharing in callbacks and modular code |

---
### **Local Scope in PHP**

#### **1. Overview**

A **local variable** is defined **inside a function** and is accessible **only within that function’s execution context**.
It is created when the function begins and is destroyed immediately when the function returns.
Local scope ensures **safe, isolated, and predictable behavior**, making it ideal for temporary calculations, request-based processing, and logic that should not leak outside its boundary.
#### **2. Core Characteristics**

| Attribute      | Description                                                               |
| -------------- | ------------------------------------------------------------------------- |
| **Visibility** | Available only inside its defining function                               |
| **Lifetime**   | Exists only during that function’s execution                              |
| **Purpose**    | Temporary computation and internal logic                                  |
| **Safety**     | Eliminates naming conflicts and prevents unintended external modification |
#### **3. Real-World Example — Order Checkout Calculation**

Typical scenario in e-commerce where internal intermediate values should not affect global system state.

```php
<?php
declare(strict_types=1);

function calculateOrderTotal(float $price, float $taxRate): float
{
    $taxAmount = $price * $taxRate;   // Local variable for internal computation
    $total = $price + $taxAmount;     // Local variable encapsulating calculation result

    return $total;                    // After returning, both locals are destroyed
}

echo calculateOrderTotal(999.00, 0.18);   // Output: 1178.82

// Attempting to access $taxAmount or $total here would result in an error
```

#### **4. Technical Interpretation (Mentor Explanation)**

Inside `calculateOrderTotal()`, the variables `$taxAmount` and `$total` are allocated as local stack memory.
They exist **only while the function is executing**, and PHP automatically frees the memory once the function completes and returns the result.

By keeping these values local, the function:

* Prevents unintended interference from the outside environment
* Avoids name collisions with other parts of the application
* Keeps internal logic clean, predictable, and side-effect-free

> **Think of local variables as tools used at your workstation: you pick them up for a task, put them away after finishing, and they never clutter the shared workspace.**

Perfect — here is the **professional, refined, high-quality** version for **Global Scope**, fully aligned with the Local and Closure sections style.

---
### **Global Scope in PHP**

#### **1. Overview**
A **global variable** is defined **outside any function or class** and is accessible **throughout the entire script**, except inside functions unless explicitly imported using the `global` keyword or the `$GLOBALS` array.
Global scope is typically used for **application-wide configuration data, shared resources, and values that must remain consistent across multiple execution points**.

#### **2. Core Characteristics**

| Attribute      | Description                                                                                |
| -------------- | ------------------------------------------------------------------------------------------ |
| **Visibility** | Accessible anywhere in the script, but not inside functions without `global` or `$GLOBALS` |
| **Lifetime**   | Exists for the duration of the script execution                                            |
| **Purpose**    | Sharing common configuration and reusable values                                           |
| **Caution**    | Overuse may reduce maintainability and introduce unexpected side effects                   |
#### **3. Real-World Example — Application-Wide Settings**

Global variables holding system configuration so that all parts of the application can reference the same setup.

```php
<?php
declare(strict_types=1);

$config = [
    'currency'  => 'INR',
    'tax_rate'  => 0.18,
    'timezone'  => 'Asia/Kolkata'
]; // Global variable

function getTotalWithTax(float $amount): float
{
    global $config; // Import global into local scope

    $tax = $amount * $config['tax_rate'];
    return $amount + $tax;
}

echo getTotalWithTax(1000);  // Output: 1180.00
```

#### **4. Technical Interpretation (Mentor Explanation)**

Here’s what’s happening conceptually:
When the script starts, `$config` is created in the **global execution environment**.
Functions do not automatically see global variables — this isolation protects internal logic from accidental interference.
However, by using the `global` keyword, we explicitly **bring `$config` into the function’s local scope**, giving controlled access.

This allows multiple functions to:
* Share consistent configuration values
* Avoid repeated declarations
* Maintain a single source of truth for application settings

But be careful: excessive global usage makes code harder to track and debug because **any part of the system may modify shared values**.

> **Use globals sparingly — only for true application-level constants or stable shared configuration. For anything dynamic or stateful, prefer dependency injection or closures.**

---
### **Static Scope in PHP**

#### **1. Overview**
A **static variable** is declared **inside a function**, but unlike a regular local variable, it **persists across multiple function calls**.
Instead of being recreated each time, its value is retained in memory and continues from where it left off.
Static scope is ideal for **counters, caching, throttling, memoization, and preserving state without using globals or objects**.

#### **2. Core Characteristics**

| Attribute      | Description                                                    |
| -------------- | -------------------------------------------------------------- |
| **Visibility** | Accessible only inside the function where it is declared       |
| **Lifetime**   | Persists for the full script execution, not recreated per call |
| **Purpose**    | Remembering state across executions                            |
| **Behavior**   | Acts like a private static property without requiring a class  |
#### **3. Real-World Example — API Request Attempt Counter**

Useful in login systems, API middleware, or rate-limited resources.

```php
<?php
declare(strict_types=1);

function loginAttemptTracker(): int
{
    static $attempts = 0;  // Static: persists across calls, not reinitialised
    $attempts++;

    return $attempts;
}

// First request
echo loginAttemptTracker(); // 1
echo loginAttemptTracker(); // 2
echo loginAttemptTracker(); // 3

// Another independent tracker instance
function paymentAttemptTracker(): int
{
    static $attempts = 0;
    return ++$attempts;
}

echo paymentAttemptTracker(); // 1
echo paymentAttemptTracker(); // 2
```

#### **4. Technical Interpretation (Mentor Explanation)**

Inside `loginAttemptTracker()`, the variable `$attempts` is initialized **only during the first function call**.
Instead of being destroyed when the function returns, PHP stores it in internal static storage, allowing the next invocation to continue from the previous value.

So on each call:
1. The function retrieves the preserved value of `$attempts`
2. It increments the value
3. It saves it back into static memory
4. It returns the new state

Unlike global variables, the static variable:

* Cannot be accessed or modified outside the function
* Cannot collide with other parts of the program
* Keeps its state **privately and predictably**

> **Think of a static variable like a notebook that stays on a specific desk — every time you come back, your notes are still there, but no one else can touch them.**

---
### **Closure Scope in PHP**

#### **1. Overview**

A **closure** is an anonymous function that can access variables from the lexical scope where it was defined, even after that scope has ended.  
Closures enable **stateful behavior, encapsulation, and controlled sharing** without global variables or full OOP structures.  
Commonly used in **caching, throttling, rate limiting, counters, memoization, and middleware pipelines**.
#### **2. Key Concepts**

|Concept|Definition|Practical Value|
|---|---|---|
|`use` keyword|Injects external variables into the closure|Context-aware logic|
|`&` reference capture|Modifies the original variable|Persistent state|
|Anonymous function|Function without a name|Flexible, reusable, passable behavior|

---

#### **3. Real-World Example — Request Rate Limiter**

A closure maintaining request count per user/session — a typical middleware scenario.

```php
<?php
declare(strict_types=1);

/**
 * Returns a rate limiter that tracks how many times an action
 * has been performed within the current application run.
 */
function createRateLimiter(int $maxAttempts = 3)
{
    $attempts = 0; // Local variable inside the outer scope

    return function () use (&$attempts, $maxAttempts): bool {
        $attempts++; // Persistent mutation

        if ($attempts > $maxAttempts) {
            return false;  // Limit exceeded
        }

        return true;       // Allowed
    };
}

// Usage
$loginLimiter = createRateLimiter(3);

var_dump($loginLimiter()); // true
var_dump($loginLimiter()); // true
var_dump($loginLimiter()); // true
var_dump($loginLimiter()); // false  (limit reached)
```

---

#### **4. Technical Interpretation (Mentor Explanation)**

Here’s what’s happening internally:

- `$attempts` is created inside `createRateLimiter()` as a normal local variable.
    
- Normally it would disappear once the function returns — but when we capture it using `use (&$attempts)`, PHP attaches it to the closure’s private memory.
    
- Each call to `$loginLimiter()` reads and updates the same `$attempts` value instead of recreating it.
    
- Because this closure instance retains its own state, different limiters track independently (e.g., per user/session/API key).
    
- The variable stays alive as long as the closure reference exists and is cleaned up when references are removed or the script ends.
    

> **This turns a closure into a lightweight object with a private property — ideal for stateful behaviors without a class.**


## 10 Copy-on-Write in PHP

### **1. Overview**
**Copy-on-Write (COW)** is a **memory optimization strategy** in PHP where values are **not duplicated immediately** when assigned to another variable.  
Instead, both variables **share the same memory reference** until one of them is modified.  
Only at the moment of modification does PHP create a **new independent copy**, ensuring efficient memory usage and faster performance.

This technique is particularly beneficial when dealing with **large arrays, strings, and datasets**, where unnecessary copying would slow execution and consume excessive memory.

---

### **2. Core Characteristics**

|Attribute|Description|
|---|---|
|**Behavior**|Actual data copy occurs **only when modification happens**|
|**Memory Efficiency**|Saves memory by sharing data until writing is required|
|**Performance Boost**|Speeds up script execution by avoiding premature duplication|
|**Applies To**|Arrays & strings primarily (objects behave differently due to reference semantics)|

---

### **3. Real-World Example — Cart Item Editing in E-commerce**

Imagine a scenario where a user’s cart is duplicated for order preview.  
The preview should not modify the original cart unless a change is made:

```php
<?php
declare(strict_types=1);

$cart = ['Laptop', 'Mouse', 'Keyboard'];   // Original cart in memory
$previewCart = $cart;                      // No copy yet – memory shared

$previewCart[0] = 'MacBook Pro';           // Modification triggers copy-on-write

print_r($cart);        // ['Laptop', 'Mouse', 'Keyboard']
print_r($previewCart); // ['MacBook Pro', 'Mouse', 'Keyboard']
```

---

### **4. Technical Interpretation (Mentor Explanation)**

Initially, `$cart` and `$previewCart` both reference the **same zval container** storing the array.  
They share memory through a **reference count system** to avoid unnecessary duplication.

When `$previewCart[0]` is modified:

- PHP notices the write operation
    
- It **performs a split**, creating a fresh memory copy exclusively for `$previewCart`
    
- `$cart` remains untouched, preserving original data integrity
    

This approach ensures:

- Efficient use of server memory
    
- Better performance with large data collections
    
- Safe and predictable behavior without accidental data mutation
    

> **Think of it like sharing a photocopy of a worksheet — everyone can read the same copy, but if someone wants to write on it, they must take their own copy first.**

---

Absolutely — here’s your **Casting & Type Conversion in PHP** section, significantly improved and extended while keeping the exact polished structure and tone of the “Local Scope” and “Copy-on-Write” explanations you liked.

## 11. Casting & Type Conversion in PHP

#### 1. Overview
Casting & Type Conversion in PHP is the process of explicitly (or implicitly) transforming a value from one scalar type to another — e.g., string → int, float → string, array → object, null → bool, etc.

It is a fundamental tool for writing predictable, bug-resistant code, especially when dealing with:
- User input (always strings)
- API payloads (JSON → mixed types)
- Database results (often strings or null)
- Strict arithmetic or comparison operations

Explicit casting forces PHP to rebuild the value inside a new typed **zval** container, eliminating ambiguity and preventing silent type-juggling surprises.

#### 2. Core Characteristics

| Attribute         | Description                                                                                  |
|-------------------|----------------------------------------------------------------------------------------------|
| Purpose           | Guarantee a variable has the exact type required for an operation                            |
| Control           | Full developer control — overrides PHP’s lenient type juggling                              |
| Techniques        | `(int)`, `(float)`, `(string)`, `(bool)`, `(array)`, `(object)`, `(unset)`, `settype()`, `intval()`, `strval()`, etc. |
| Behavior          | Creates a **new zval**; original variable is unchanged unless reassigned                    |
| Strict Types      | With `declare(strict_types=1);`, casting becomes even more critical for function signatures |
| Safety            | Eliminates “String + String = Concatenation” bugs and unexpected truthy/falsy results       |

#### 3. Real-World Example — Checkout Quantity Validation (Improved)

```php
<?php
declare(strict_types=1);

$quantityInput = $_POST['qty'] ?? "0";   // User might send "7", "007", "7abc", or nothing
$price         = 199.99;                 // float from database

// Chain of defensive casts — common pattern in production code
$quantity = (int) filter_var($quantityInput, FILTER_SANITIZE_NUMBER_INT);
// → "7abc" becomes "7", empty string becomes "0"

if ($quantity <= 0) {
    throw new InvalidArgumentException("Quantity must be positive");
}

$total = $quantity * $price;   // Guaranteed numeric multiplication

echo "Total: $" . number_format($total, 2);
// → "Total: $1,399.93" instead of weird string concatenation
```

Without the explicit `(int)` cast, `$quantityInput . $price` could accidentally concatenate and produce `"7199.99"`!

#### 4. Technical Interpretation (Mentor Explanation)

Under the hood, every PHP variable is a **zval** structure containing:
- The value itself
- Its current type (`IS_STRING`, `IS_LONG`, `IS_DOUBLE`, etc.)

When you write `(int)$var`, PHP:
1. Reads the current zval type
2. Allocates a **brand-new zval** of type `IS_LONG` (integer)
3. Converts the old value according to PHP’s official conversion rules (see tables below)
4. Returns the new zval (leaving the original untouched unless you reassign)

This is **not** a reference change — it’s a true type transformation. That’s why casting is safe and predictable.

Analogy time (the “Toy Box” version for juniors):
> Imagine each value lives in a colored box labeled with its type (red = string, blue = number, green = array).  
> Casting is like taking the toy out of the red string box and putting it into a brand-new blue number box — the old red box still exists until garbage-collected, but now you’re playing with the correctly shaped toy.

#### 5. Complete Casting Reference Table (Bonus!)

| Cast To       | Syntax               | What Actually Happens (examples)                                                                 |
|---------------|----------------------|---------------------------------------------------------------------------------------------------|
| `(int)` / `(integer)` | `(int)$var`         | "123" → 123, "12abc" → 12, "abc" → 0, 3.14 → 3, true → 1, [] → 0                                 |
| `(float)` / `(double)`| `(float)$var`       | "12.5" → 12.5, "12abc" → 12.0, true → 1.0, "1e3" → 1000.0                                        |
| `(string)`    | `(string)$var`       | 123 → "123", true → "1", false → "", null → "", [1,2] → "Array" (with notice in some contexts)   |
| `(bool)`      | `(bool)$var`         | "", 0, "0", null, [] → false; everything else → true                                            |
| `(array)`     | `(array)$var`        | "hello" → ["hello"], 42 → [42], null → [], stdClass → (array) of properties                      |
| `(object)`    | `(object)$var`       | "hello" → stdClass with scalar property, [a=>1] → stdClass with public properties               |
| `(unset)`     | `(unset)$var`        | Converts to null (rarely used except to silence “undefined variable” warnings)                  |

#### 6. Explicit Casting vs Automatic Type Juggling (Quick Comparison)

| Situation                     | Automatic Juggling                     | Explicit Casting (Recommended)               |
|-------------------------------|----------------------------------------|----------------------------------------------|
| `"5" + "5"`                   | 10 (PHP silently converts to int)      | `(int)"5" + (int)"5"` → always 10            |
| `"5" . "5"`                   | "55" (string context)                  | Same, but intention is crystal clear         |
| `"0" == false`                | true (loose comparison)                | `(bool)"0"` is false — predictable           |
| Function with `int $id` param | Accepts "123" (juggling)               | With strict_types=1 → TypeError unless cast  |

**Rule of thumb:** In modern PHP (8.1+), prefer explicit casting + `strict_types=1`.

---
## 12. Debugging Tools in PHP

#### 1. Overview
Debugging tools are the developer’s **X-ray vision** into PHP’s runtime world. They reveal exactly what a variable contains, its real type, memory structure, reference status, and internal representation — all critical when hunting down subtle bugs caused by type juggling, copy-on-write quirks, unexpected API payloads, or silent coercion.

Mastering these tools turns “it works on my machine” into “I know exactly why it works — and why it breaks in production.”

#### 2. Core Characteristics

| Tool                  | Best For                                      | Output Style               | Shows Types? | Shows Refcount / Internals? | Safe in Production? |
|-----------------------|-----------------------------------------------|----------------------------|--------------|------------------------------|---------------------|
| `print_r()`           | Quick human-readable overview                 | Clean, recursive          | Partial      | No                           | Yes (with care)     |
| `var_dump()`          | Precise type + length inspection              | Verbose, typed             | Yes          | No                           | No (leaks data)     |
| `var_export()`        | Generate valid PHP code representation        | `var_export()` output      | Yes          | No                           | Yes                 |
| `debug_zval_dump()`   | Deep engine-level analysis (references, CoW)  | Raw zval details           | Yes          | Yes                          | No                  |
| `get_defined_vars()`  | Dump entire local scope                       | Array of all variables     | —            | —                            | Dev only            |
| `gettype()` / `get_debug_type()` (PHP 8+) | Precise type name without full dump | Single string              | Yes          | No                           | Yes                 |

#### 3. Real-World Example — Diagnosing a Silent Cart Total Bug

You’re building a checkout system and the total is mysteriously wrong:

```php
<?php
declare(strict_types=1);

$cart = json_decode(file_get_contents('api/cart.json'), true);

$total = 0;
foreach ($cart['items'] as $item) {
    $total += $item['price'] * $item['quantity'];  // Should be numeric… but isn't!
}

echo "Total: $total"; // → Total: 0  (?!)
```

**Step-by-step debugging session:**

```php
// 1. Quick look
print_r($cart['items'][0]);

// 2. Full type inspection → reveals the real culprit
var_dump($cart['items'][0]);

// 3. Engine-level view (only in dev!)
debug_zval_dump($cart['items']);
```

**Actual output that saves the day:**

```php
array(3) {
  ["price"]=>
  string(5) "299.99"     // ← JSON decoded numbers as strings!
  ["quantity"]=>
  string(1) "2"
  ["name"]=>
  string(12) "Wireless Mouse"
}
```

**Fix in one line:**

```php
$total += (float)$item['price'] * (int)$item['quantity'];
```

#### 4. Technical Interpretation (Mentor Explanation)

Think of every PHP variable as a **zval** — a C struct living in memory with:
- value
- type tag (`IS_STRING`, `IS_LONG`, `IS_ARRAY`, etc.)
- refcount
- is_ref flag

Here’s what each tool actually inspects:

| Tool                  | What It Peeks Into                                      | Why It Matters |
|-----------------------|---------------------------------------------------------|----------------|
| `print_r()`           | Recursive value only — hides types                     | Fast sanity check |
| `var_dump()`          | Value + type + string length — no refcount             | Catches 95% of type bugs |
| `debug_zval_dump()`   | Full zval internals: refcount, is_ref, memory address  | Explains Copy-on-Write surprises |
| `var_export()`        | Produces copy-pasteable PHP code                       | Perfect for logging or tests |

**Pro tip analogy (the “Car Dashboard” version):**  
- `print_r()` = your speedometer (quick glance)  
- `var_dump()` = full instrument cluster + warning lights  
- `debug_zval_dump()` = opening the hood and attaching an OBD-II scanner to the engine computer  
- Xdebug = putting the car on a dynamometer and driving it step-by-step

In modern PHP 8.2+, combine with `get_debug_type($var)` for clean, human-readable type names without the noise of `var_dump()`.

---

Here is a **condensed “most important only” version** focused on what actually matters in real-world PHP engineering & interviews:

---

## 13. [[Garbage Collection.spl]]

| **Topic**                                  | **Why It Matters**                                                                     |
| ------------------------------------------ | -------------------------------------------------------------------------------------- |
| **Memory Model & zval**                    | Foundation for understanding variable storage, references, and copy-on-write behavior. |
| **Reference Counting**                     | Explains when memory is freed vs retained; root cause of many leaks.                   |
| **Circular References & Cycle Collection** | Key reason GC exists; essential for preventing leaks in real systems.                  |
| **GC Triggers**                            | Knowing when PHP actually runs cleanup helps performance tuning.                       |
| **Manual GC Control**                      | Useful in workers, jobs, and high-memory workloads.                                    |
| **Unset() vs Scope Exit**                  | Practical technique for reclaiming memory early.                                       |
| **Long-Running Script Strategy**           | Critical for daemons, queues, imports, real-time processes.                            |
| **Debugging Memory Leaks**                 | Must-know tools and techniques for diagnosing problems.                                |
| **Web vs CLI GC Behavior**                 | Impacts resource planning in persistent apps like Swoole / RoadRunner.                 |
| **Best Practices & Real-World Scenarios**  | Converts theory into production decisions.                                             |

---
