### Q1. What does `declare(strict_types=1);` actually change in PHP, and why does it matter for function arguments and return types?

#### **Answer**

`declare(strict_types=1)` enables **strict scalar type checking** for function calls and return values.  
Without it, PHP performs **weak type juggling**, automatically converting values to the expected type — sometimes silently causing incorrect behavior.

With strict mode enabled, PHP requires the exact expected type and throws a **TypeError** instead of silently coercing values.

---

#### **Real-world damage example: authentication failure**

```php
// strict_types = 0 (default weak typing)
function login(int $userId) {
    echo "Logged in: $userId";
}

login("007");   // Output: "Logged in: 7"
```

😱 `"007"` becomes `7` — leading zero removed  
🔐 Wrong user record fetched → security breach

---

```php
// strict_types = 1
declare(strict_types=1);

function login(int $userId) {
    echo "Logged in: $userId";
}

login("007");   // TypeError: Argument must be of type int
```

✔ Correct failure → prevents silent security leaks  
✔ Enforces integrity at system boundaries

---

#### **Strict return type example**

```php
declare(strict_types=1);

function balance(): float {
    return "100";   // TypeError: expected float, got string
}
```

Without strict mode, conversion hides bugs:

```php
function balance(): float {
    return "100";   // returns 100.0 silently
}
```

---

#### **Quick Comparison Table**

|Behavior|Weak typing (default)|strict_types=1|
|---|---|---|
|`"10" * 2`|20|TypeError if signature demands int|
|`"true"` as bool|becomes `true`|must be real boolean|
|Runtime failures|silent bugs|explicit, catchable|
|Security risk|high|low|

---

> **“Always enable strict types. Bugs caught at compile-time are cheaper than bugs discovered in production.”**

---
### Q2. What additional check does `===` perform that `==` does not, and why is `==` considered dangerous in real systems?

#### **Answer**

`===` performs **strict comparison**, requiring both **value and type** to match.  
`==` performs **loose comparison**, allowing PHP to automatically convert values (type juggling) before comparison.

Loose comparison is dangerous because very different values can incorrectly evaluate as equal — leading to authentication bypasses, corrupted logic, and security vulnerabilities.

---

#### **Real-world damage example: login bypass due to hash jiggling**

```php
$hash = "0e462097431906509019562988736854";  // Looks like scientific notation
$input = "0e123456789000000000000000000000";

if ($input == $hash) {
    echo "Authenticated!";
}
```

Both values convert to float `0.0`  
`"0e..."` → becomes scientific notation  
Result: `0.0 == 0.0` → `true` → authentication bypass

This has caused real-world CVEs when comparing MD5 or SHA1 hashes starting with `"0e"`.

---

```php
if ($input === $hash) {
    echo "Secure comparison";
}
```

✔ Strict mode prevents unintended conversion  
✔ Proper cryptographic comparison uses `hash_equals()`

---

#### **Another real-world failure: boolean confusion**

```php
$flag = "false";

if ($flag == false) {
    echo "Feature Enabled";
}
```

Output:

```
Feature Enabled
```

Because `"false"` becomes numeric `0`, and `0 == false` is true → feature incorrectly activated.

---

#### **Quick Comparison Table**

|Expression|Result|Reason|
|---|---|---|
|`"0" == false`|true|`"0"` becomes `0`|
|`"abc" == 0`|true|non-numeric string becomes `0`|
|`"123" === 123`|false|string vs integer|
|`"0" === false`|false|different types|

---
> **“If you aren’t explicitly coercing types, never use `==`. Strict comparison avoids silent security failures.”**
---
### Q3. Give two real examples where type juggling produces unexpected or risky results in PHP.

#### **Answer**
Type juggling occurs when PHP automatically converts values between types during comparison or arithmetic.  
While convenient in small scripts, it becomes **dangerous in authentication, financial calculations, and boolean logic**, where silent conversion leads to incorrect behavior and exploitable vulnerabilities.
#### **Real-world damage example: authentication bypass via numeric-string casting**

```php
$storedHash = "0e462097431906509019562988736854";   // MD5 hash shaped like exponential notation
$inputHash  = "0e123456789000000000000000000000";

if ($inputHash == $storedHash) {
    echo "Logged in!";
}
```

😱 Both strings get converted to float `0.0`  
🔓 `0.0 == 0.0` → `true`  
🚨 User logs in without valid credentials

This vulnerability has appeared in real CVEs exploiting `"0e"` hashes from weak MD5 operations.

---

```php
if ($inputHash === $storedHash) {
    echo "Secure comparison";
}
```

✔ Strict comparison stops the attack  
✔ Proper fix: `hash_equals()` for timing-safe comparison

---

#### **Real-world damage example: boolean coercion breaking feature flags**

```php
$enabled = "false";

if ($enabled == false) {
    echo "Feature Enabled";
}
```

Output:

```
Feature Enabled
```

Because `"false"` becomes numeric `0`, and `0 == false` is true → feature enabled when it should be disabled.

Consequences:

- disabled payment gateways become active
    
- maintenance mode accidentally turns off
    
- security switches reverse behavior
    

---

```php
if ($enabled === false) {
    echo "Correct behavior";
}
```

✔ Explicit type protects logic  
✔ Prefer `filter_var($enabled, FILTER_VALIDATE_BOOLEAN)` for config flags

---

#### **Quick Comparison Table**

|Expression|Result|Reason|
|---|---|---|
|`"0" == false`|true|`"0"` becomes integer `0`|
|`"abc" == 0`|true|non-numeric string → `0`|
|`"10" == 10`|true|string cast to int|
|`"10" === 10`|false|different types|

---

> **“Type juggling is convenient until it silently breaks production — strict comparison and explicit casting prevent hidden failures.”**

---

Understood — rewriting **Q5** with **all code inside one single block**, keeping structure concise.

---

### Q4. Explain the bug caused by using `=` instead of `==` in an `if` condition. Provide a short code example.

#### **Answer**
`=` performs **assignment**, not comparison.  
Inside an `if` condition, assignment returns the assigned value, which is then evaluated as a boolean — causing logic to behave incorrectly and often allowing unauthorized execution.

This is one of the most common and dangerous bugs in real codebases.

#### **Example (single unified code block)**

```php
$isAdmin = false;

if ($isAdmin = true) {       // BUG: assignment, not comparison
    echo "Access granted!";
} else {
    echo "Access denied";
}

// Output: Access granted!
// Because $isAdmin becomes true and if(true) always runs

// Correct comparison
if ($isAdmin == true) {
    echo "Access granted";
}

// Safer strict version
if ($isAdmin === true) {
    echo "Access granted";
}
```

#### **Quick Summary Table**

|Operator|Meaning|Result in `if()`|
|---|---|---|
|`=`|assignment|returns assigned value|
|`==`|loose comparison|boolean comparison|
|`===`|strict comparison|safest and recommended|

---
> **“A single `=` in a condition is a silent logic bomb — always use `===` unless you truly intend assignment.”**
---
### Q5. List the three major contexts where the `=>` operator is used in PHP and give one code example for each.

#### **Answer**

The `=>` operator is used in **associative arrays**, **foreach key/value iteration**, and **array destructuring**.  
It defines explicit key–value mapping instead of relying on numeric indexes.

---

#### **Examples (all in one block)**

```php
// 1) Associative array
$user = [
    "id"   => 10,
    "name" => "Avijit"
];

// 2) foreach key => value
foreach ($user as $key => $value) {
    echo "$key = $value";
}

// 3) Array destructuring / short list syntax
$data = ["id" => 1, "role" => "admin"];
["id" => $id, "role" => $role] = $data;
```

---

#### **Quick Summary Table**

|Context|Purpose|
|---|---|
|Associative arrays|Define key/value pairs|
|foreach|Read key + value|
|Destructuring|Bind named variables|

---
> **“`=>` models relationships, not positions — critical for structured data.”**
---
### Q6. Show the difference between pre-increment (`++$x`) and post-increment (`$x++`) in terms of returned value.

#### **Answer**
`++$x` increments the value **first** and then returns the updated value.  
`$x++` returns the **original** value first, and increments **afterwards**.  
Both modify the variable, but the returned result differs.

#### **Example (single code block)**

```php
$x = 5;
echo ++$x;  // 6  (value increased, then returned)
echo $x;    // 6

$y = 5;
echo $y++;  // 5  (original returned, then increment happens)
echo $y;    // 6
```

---

#### **Quick Comparison Table**

|Operator|Returned Value|Final Stored Value|
|---|---|---|
|`++$x`|incremented value|incremented|
|`$x++`|original value|incremented|

---
> **“Use `++$x` when you need the incremented value immediately — use `$x++` when order matters.”**
---
### Q7. How does the null-coalescing operator `??` differ from the Elvis operator `?:` and the logical operator `||`?

#### **Answer**
`??` checks **only for null**, while `?:` and `||` check **truthiness**.  
This matters because values like `"0"`, `0`, and `false` should often be treated as valid but get rejected by `?:` and `||`, while `??` preserves them.
#### **Example (single code block)**

```php
$a = 0;
$b = null;

// Null-coalescing: returns first value that is not null
echo $a ?? "fallback";   // 0
echo $b ?? "fallback";   // fallback

// Elvis operator: returns fallback if falsey
echo $a ?: "fallback";   // fallback

// Logical OR: also uses truthiness
echo $a || "fallback";   // true  (echo prints 1)
```

#### **Quick Comparison Table**

| Operator | Checks    | Returns fallback when     |
| -------- | --------- | ------------------------- |
| `??`     | null only | value is `null`           |
| `?:`     | falsy     | `0`, `"0"`, `false`, `""` |
| `        |           | `                         |

---
> **“Use `??` for optional values — use `?:` or `||` only when falsy values are invalid.”**
---
### Q8. Predict the output of: `"5" + 3`, `"5" . 3`, `"10 pigs" + 2`, `2 . "10 pigs"` — and explain why.

#### **Answer**
`+` triggers **numeric conversion**, while `.` performs **string concatenation**.

When PHP encounters a non-numeric string in arithmetic, it converts until the first invalid character, falling back to `0` if nothing numeric exists.
#### **Example (single unified code block with outputs)**

```php
echo "5" + 3;         // 8        (string converted to number)
echo "5" . 3;         // "53"     (concatenation, no conversion)

echo "10 pigs" + 2;   // 12       ("10" extracted, rest ignored)
echo 2 . "10 pigs";   // "210 pigs" (concatenation, both treated as strings)
```

#### **Quick Summary Table**

|Expression|Output|Reason|
|---|---|---|
|`"5" + 3`|`8`|numeric conversion|
|`"5" . 3`|`"53"`|concatenation|
|`"10 pigs" + 2`|`12`|numeric prefix used|
|`"pigs" + 2`|`2`|converts to `0`|
|`2 . "10 pigs"`|`"210 pigs"`|string concatenation|

---
> **“Use `.` for strings — using `+` risks hidden numeric coercion and data corruption.”**
---
### Q9. List PHP’s four scalar types and four compound types. Which additional type is neither scalar nor compound?

#### **Answer**
PHP defines **scalar types** for primitive values and **compound types** for structured data collections.  
There is also an additional **special type** that belongs to neither category.
#### **Types Overview (single block for clarity)**

```php
// Scalar Types
int
float
string
bool

// Compound Types
array
object
callable
iterable

// Special Type (neither scalar nor compound)
null
```
#### **Explanation**

- **Scalar** = single discrete value (used for computation & comparisons)
    
- **Compound** = grouped or structured values
    
- **Null** = represents absence of value or uninitialized state
#### **Quick Summary Table**

|Category|Types|
|---|---|
|Scalar|int, float, string, bool|
|Compound|array, object, callable, iterable|
|Special|null|

---
> **“Null is not a type of data — it represents the absence of data and therefore stands alone.”**
---
### Q10. Why does `"10abc" == 10` evaluate to true while `"10abc" == 0` also evaluates to true? Explain conversion order.

#### **Answer**
In loose comparison, PHP performs **numeric conversion** on strings before comparison.  
When a string begins with digits, PHP extracts the numeric prefix.  
When a string contains **no valid numeric prefix**, it converts the entire value to `0`.

Because of this conversion order, `"10abc"` becomes `10` in numeric comparison — but also becomes `0` when compared in certain mixed contexts.
#### **Example (single code block with outputs)**

```php
echo "10abc" == 10;   // true   ("10abc" → 10)
echo "10abc" == 0;    // true   ("10abc" → 10, 10 == 0 after additional juggling)

echo "abc" == 0;      // true   (no numeric prefix → 0)
echo "0abc" == 0;     // true   ("0" extracted → 0)
```
#### **Conversion rules summary**

|Value|Converted To|Reason|
|---|---|---|
|`"10abc"`|`10`|numeric prefix `10` parsed|
|`"abc"`|`0`|no digits found|
|`"0abc"`|`0`|numeric prefix `0`|
#### **Why both comparisons become true**

1. `"10abc"` → `10`    
2. In mixed numeric context, PHP may *re-evaluate* comparison with coercion    
3. `10 == 0` is `false`, but chaining coercion changes evaluation order inside comparisons, causing multiple false positives in real configuration scenarios    

---
> **“Loose comparison rewrites values before comparing them — strict comparison (`===`) avoids dangerous type juggling.”**
---
### Q11. Which values evaluate to `false` in a boolean context in PHP? List all of them.

#### **Answer**
PHP treats several values as **falsy**, meaning they convert to `false` when evaluated in a boolean context (e.g., inside `if`, `while`, logical operators).  
Knowing the full list prevents accidental rejection of meaningful values like `"0"` or `0`.
#### **Falsy values list (single block)**

```php
false
0
0.0
"0"
""
null
[]
{}
"0.0"        // numeric string converted to 0
```

(Also resources on failure, e.g., `fopen()` returning `false`)
#### **Example of unexpected failure**

```php
if ("0") {
    echo "true";
} else {
    echo "false";  // runs
}
```

😱 `"0"` is considered falsy → often causes login, OTP, or config bugs

#### **Quick Summary Table**

|Type|Falsy Values|
|---|---|
|Boolean|`false`|
|Integer & Float|`0`, `0.0`|
|Strings|`"0"`, `""`|
|Null|`null`|
|Arrays|`[]`, `{}`|
|Resource failures|return `false`|

---
> **“Never validate user input with `if($x)` — it silently rejects `"0"` and `0`.”**
---
### Q12. Show an example of a real bug that occurs due to misunderstanding operator precedence between `&&` and `or`.

#### **Answer**
`&&` has **higher precedence** than `or`, meaning expressions containing both operators may not evaluate in the order developers expect.  
This causes assignments to bind unexpectedly, especially with return values, causing silent logical failures.

#### **Real-world bug example (single code block)**

```php
$loggedIn = false;

// Developer intends: assign result of (check && required) to $loggedIn
$loggedIn = checkUser() && verifyOTP() or die("Auth failure");
```

Expected logic:

```
($loggedIn = checkUser() && verifyOTP()) or die(...)
```

Actual logic executed by PHP:

```
($loggedIn = checkUser() && verifyOTP()) or die(...)
```

But because `&&` binds tighter than `or`, the assignment occurs **before** the `or`, so `$loggedIn` becomes `true` if `checkUser()` is true — even if `verifyOTP()` fails.
#### **Correct versions**

```php
$loggedIn = checkUser() && verifyOTP() or die("Auth failure"); // wrong
$loggedIn = checkUser() && verifyOTP() || die("Auth failure"); // still unclear
$loggedIn = (checkUser() && verifyOTP()) or die("Auth failure"); // correct


if (!(checkUser() && verifyOTP())) {
    die("Auth failure");
}
```

#### **Quick Summary Table**

|Operator|Precedence|Danger|
|---|---|---|
|`&&`|higher|evaluates first|
|`and` / `or`|lower|messes up assignments|
|Unauthorized access|likely|due to wrong grouping|

---
> **“Never mix `&&` with `and`/`or` — wrapping in parentheses avoids invisible precedence bugs.”**
---

### Q13. What is the practical difference between implicit type conversion and explicit type casting?

#### **Answer**

**Implicit type conversion** (type juggling) happens automatically during operations or comparisons, without developer intention.  
**Explicit type casting** is when the developer intentionally converts a value to a specific type, making behavior predictable and safe.

Implicit conversion is convenient but dangerous in critical logic; explicit casting ensures correctness and prevents hidden coercion bugs.

#### **Examples (single code block)**

```php
// Implicit conversion (automatic)
$result = "5" + 3;     // 8  ("5" converted to int automatically)
if ("0" == false) { }  // true unexpectedly

// Explicit type casting (intentional)
$price = (float) "5.99";     // 5.99
$count = (int) "010";        // 10
$enabled = (bool) "false";   // true, but conversion is explicit & expected
```

#### **Quick Summary Table**

|Type Conversion|Triggered By|Safe?|Control|
|---|---|---|---|
|Implicit|operations & comparisons|❌ risky|low|
|Explicit|`(int)`, `(float)`, `(bool)`, `(string)`|✅ predictable|high|

---
> **“Implicit conversion surprises you — explicit casting communicates intent and prevents silent bugs.”**
---
### Q14. What happens when you cast `null`, `false`, `""`, and `"0"` to `(int)` and to `(bool)`?

#### **Answer**
Casting changes the underlying type explicitly.  
PHP applies consistent conversion rules: **empty or null values become `0` when cast to int**, and **become `false` when cast to bool** — except `"0"`, which becomes `false` and often causes subtle validation bugs.

#### **Casting examples (single code block)**

```php
// (int) casting
(int) null   = 0
(int) false  = 0
(int) ""     = 0
(int) "0"    = 0

// (bool) casting
(bool) null  = false
(bool) false = false
(bool) ""    = false
(bool) "0"   = false   // critical edge case (OTP, config flags)
```

#### **Quick Summary Table**

|Value|(int) Result|(bool) Result|
|---|---|---|
|`null`|`0`|`false`|
|`false`|`0`|`false`|
|`""`|`0`|`false`|
|`"0"`|`0`|`false`|

---
> **“`"0"` behaves like `false` in boolean context — never rely on truthiness for input validation.”**
---
### Q15. What is the difference between `echo` and `print`, and what does `print` actually return?

#### **Answer**

Both `echo` and `print` output text to the browser, but they differ in behavior.  
`echo` is slightly faster and can output multiple arguments, while `print` always returns `1` and can be used inside expressions.

#### **Examples (single code block)**

```php
echo "Hello";            // outputs text
echo "A", "B", "C";      // multiple arguments supported

print "Hello";           // outputs text and returns 1
$result = print "Hi";    // prints "Hi" and $result becomes 1
```

#### **Quick Summary Table**

|Feature|echo|print|
|---|---|---|
|Multiple arguments|Yes|No|
|Return value|No|Yes (`1`)|
|Performance|Faster|Slightly slower|
|Use inside expressions|No|Yes|

---
> **“Use `echo` for output; use `print` only when a return value is needed in expressions.”**
---
### Q16. How does the `instanceof` operator behave when used with interfaces, parent classes, and traits?

#### **Answer**
`instanceof` checks whether an object belongs to a class, a parent class, or implements an interface.  
It **does not work with traits**, because traits are reused code blocks, not types.
#### **Examples (single unified code block)**

```php
class Animal {}
class Dog extends Animal implements JsonSerializable {
    public function jsonSerialize() { return []; }
}

$dog = new Dog();

var_dump($dog instanceof Dog);              // true
var_dump($dog instanceof Animal);           // true (parent class)
var_dump($dog instanceof JsonSerializable); // true (interface)
var_dump($dog instanceof stdClass);         // false

trait Flyer {}
// $dog instanceof Flyer;  // Fatal error: traits cannot be checked with instanceof
```

#### **Quick Summary Table**

|Type|`instanceof` Works?|Notes|
|---|---|---|
|Same class|Yes|direct instance|
|Parent class|Yes|inheritance|
|Interface|Yes|implementation|
|Trait|No|traits are not types|
|Unrelated class|No||

---
> **“Use `instanceof` for type safety across inheritance hierarchies — traits are not part of the type system.”**
---
### Q17. When comparing arrays, under what scenario does `==` return true but `===` returns false?

#### **Answer**

`==` compares arrays by **value only**, ignoring order and type of keys.  
`===` requires arrays to have **the same values, in the same order, with the same key types**.

Therefore, two arrays may appear equal under `==` but fail under strict comparison using `===`.
#### **Example (single unified code block)**

```php
$a = [1, 2, 3];
$b = [1, 2, 3];

// Associative key order difference
$c = ["id" => 1, "age" => 20];
$d = ["age" => 20, "id" => 1];

var_dump($c == $d);   // true  (same values, key order ignored)
var_dump($c === $d);  // false (order and key positions matter)

// Numeric string vs integer keys
$e = [1 => "A"];
$f = ["1" => "A"];

var_dump($e == $f);   // true
var_dump($e === $f);  // false (int vs string key)
```

#### **Quick Summary Table**

|Comparison|Order matters?|Key type matters?|Safe?|
|---|---|---|---|
|`==`|No|No|❌ risky|
|`===`|Yes|Yes|✅ recommended|

---
> **“Use `===` for arrays unless you explicitly want loose comparison ignoring structure.”**
---
### Q18. What value does PHP assign to a declared but uninitialized variable, and what happens when referencing an undeclared variable with `E_ALL` enabled?

#### **Answer**
A **declared but uninitialized variable** defaults to `null`.  
An **undeclared variable** triggers a **notice** under `E_ALL` and also evaluates as `null`, which can hide bugs if errors are suppressed.

#### **Example (single unified code block)**

```php
// Declared but uninitialized
$price;
var_dump($price);   // NULL

// Undeclared variable
var_dump($total);   // Notice: Undefined variable $total
                    // NULL returned and used in expression

// Silent bug example
if ($isAdmin) {   // $isAdmin never set
    // unintended access path due to null -> falsey coercion
}
```

#### **Quick Summary Table**

|Case|Behavior|Value|Risk|
|---|---|---|---|
|Declared but uninitialized|Allowed|`null`|logic confusion|
|Undeclared variable|Notice (E_ALL)|`null`|hidden bugs & security issues|
|Error suppressed (with `@`)|No warning|`null`|dangerous|

---
> **“Treat undefined variables as failures — strict error levels expose hidden logic bugs.”**
---

### Q19. Which operator performs string concatenation in PHP, and why should `+` not be used for strings?

#### **Answer*

The `.` operator performs **string concatenation** in PHP.  
Using `+` on strings triggers **numeric conversion**, meaning PHP attempts to treat strings as numbers and may silently strip characters or return incorrect results — causing data corruption and unpredictable behavior.

#### **Example (single unified code block)**

```php
// Correct string concatenation
echo "Hello" . " World";     // "Hello World"

// Incorrect numeric coercion
echo "10" + "20";            // 30   (converted to integers)
echo "10 apples" + 5;        // 15   ("10" extracted, rest ignored)
echo "apples" + 5;           // 5    (string becomes 0)

// Real bug example
$username = "user";
$id = "123";
$session = $username + $id;  // 123  (username lost)
```

#### **Quick Summary Table**

|Operator|Purpose|Safe?|
|---|---|---|
|`.`|concatenates strings|✅ yes|
|`+`|numeric addition / coercion|❌ never for strings|

---
> **“Always use `.` for strings — `+` quietly converts and destroys text.”**
---

### Q20. Explain the three critical differences between `and`/`or` and `&&`/`||` (precedence, return value, recommended usage). Then explain why the following behaves unexpectedly:

```php
$user = getUser() or die("No user!");
```

#### **Answer**

`&&` and `||` have **higher precedence** than `and` and `or`, meaning they bind more tightly inside expressions.  
`and` / `or` behave more like **control-flow operators**, while `&&` / `||` are intended for **logical conditions**.  
Their return values also differ: `and` / `or` return the last evaluated operand, which affects assignment results.

Because of precedence, the assignment in the example runs **before** the `or`, so `$user` receives the result of the function call even if it fails — making the `die()` never execute when expected.

#### **Bug example (single code block)**

```php
// Intended behavior
$user = getUser() or die("No user!");

// Actual interpretation (due to precedence)
($user = getUser()) or die("No user!");

// If getUser() returns null or false:
$user = null;    // assignment happened
// then `or` is evaluated afterwards
// but $user is already set → die doesn't trigger as expected
```

Correct versions:

```php
$user = getUser() || die("No user!");             // still confusing
$user = (getUser()) or die("No user!");           // grouping fix
if (!getUser()) die("No user!");                  // clear
$user = getUser() ?? abort(403);                  // modern approach
```

#### **Quick Summary Table**

|Operator|Precedence|Use Case|Risk|
|---|---|---|---|
|`&&` / `||`|higher|
|`and` / `or`|lower|control flow, readability|breaks assignments|

---
> **“Never mix assignment with `and`/`or` — always group or use `&&`/`||` for logic.”**
---
