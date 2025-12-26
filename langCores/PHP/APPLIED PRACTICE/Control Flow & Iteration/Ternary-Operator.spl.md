# 🧠 **Ternary Operator — Overview**

The **ternary operator (`?:`)** is a compact conditional expression used to return one of two values based on a boolean test. It reduces verbose `if-else` blocks and enables clean inline assignments, improving readability in expressions where short conditional evaluation is needed.

In modern PHP, the **null-coalescing (`??`)** and **null-coalescing assignment (`??=`)** variations enhance safety and streamline common patterns for default values and optional data—especially useful in API structures, form handling, and configuration systems where fallback values are essential.

Real-world usage includes: **input sanitization, permission checks, display formatting, status labels, nullable default handling, inline validation, and conditional rendering in templates**.


### **Topics to Learn — Ternary Operator**

|**Topic**|**Brief Description**|
|---|---|
|**Basic Ternary Usage**|Returns one of two values based on a conditional expression.|
|**Nested Ternaries**|Multi-layer decision paths in compact form—use sparingly to avoid complexity.|
|**Short Ternary (`?:`)**|Returns the left operand if truthy, otherwise the right—lightweight fallback.|
|**Null-Coalescing (`??`)**|Provides default values when a variable is undefined or null.|
|**Null-Coalescing Assign (`??=`)**|Assigns a fallback to the variable only when its current value is null.|
|**Inline Return / Assignment**|Enables single-line decision logic without full `if-else` blocks.|
|**Modern Best Practices**|Prioritize clarity, avoid deep nesting, and use specialized variants where useful.|

---
## 01 Basic Ternary Usage

##### **What It Means**
The **ternary operator (`?:`)** evaluates a condition and returns **one of two values** depending on whether the expression is true or false. It is a concise alternative to short `if-else` statements and is ideal for inline decision assignments where only two simple outcomes exist.
##### **Example — Inline True/False Selection**

```php
$score = 78;

$status = ($score >= 50)
    ? 'Passed'
    : 'Failed';

echo $status;
```

**Output**

```
Passed
```

> The ternary operator keeps logic compact and readable, but overusing nested ternaries can make code harder to understand.

---
## 02 Nested Ternaries

##### **What It Means**
**Nested ternaries** allow multiple conditional decisions to be handled in a single expression. They are useful when more than two outcomes must be selected, but must be formatted carefully to avoid confusing, unreadable logic.  
A good practice is to **indent or break across lines** so the decision flow remains clear.
##### **Example — Multi-Level Grading Decision**

```php
$score = 86;

$grade = ($score >= 90) ? 'A'
       : ($score >= 80) ? 'B'
       : ($score >= 70) ? 'C'
       : 'D';

echo $grade;
```

**Output**

```
B
```

> Nested ternaries are powerful for compact multi-outcome decisions, but should be avoided if readability suffers—consider `match` or `if-else` for complex logic.
---
## 03 Short Ternary (`?:`)

##### **What It Means**
The **short ternary operator (`?:`)** is a simplified version of the ternary that returns the left operand if it is **truthy**, otherwise returns the right operand.  
It removes the need to repeat the variable or expression being tested, making fallback handling compact and expressive for common default-value scenarios.

Useful when checking for non-empty strings, arrays, or other truthy values.
##### **Example — Fallback Value if Empty**

```php
$username = '';

$name = $username ?: 'Guest';

echo $name;
```

**Output**

```
Guest
```

> Use short ternary for quick fallback handling, especially in views or formatting, but avoid it when explicit null handling is needed — use `??` instead.

---
## 04 Null-Coalescing (`??`)

##### **What It Means**
The **null-coalescing operator (`??`)** returns the **first value that is not null**.  
It is commonly used when dealing with **optional data** such as request inputs, API responses, database fields, or configuration settings.  
Unlike the short ternary, it only checks for **null**, not truthiness—meaning empty strings or zero values are preserved rather than replaced.
##### **Example — Safe Default When Null**

```php
$city = null;

$location = $city ?? 'Unknown';

echo $location;
```

**Output**

```
Unknown
```

> Prefer `??` when handling nullable data sources where empty values must remain valid, unlike `?:` which treats empty as false.

---
## 05 Null-Coalescing Assign (`??=`)

##### **What It Means**
The **null-coalescing assignment operator (`??=`)** assigns a value to a variable **only if the variable is currently null**.  
It is a concise alternative to checking and assigning default values manually and is especially useful for configuration loading, input handling, and initializing optional parameters.

It effectively means:

```
if ($var is null) { $var = value; }
```

But in one clean expression.


##### **Example — Auto-Assign Default Only When Null**

```php
$limit = null;

$limit ??= 10;

echo $limit;
```

**Output**

```
10
```

> `??=` prevents overwriting meaningful values such as 0, empty strings, or false—making it safer than reassigning unconditionally.

---
## 06 Inline Return / Assignment

##### **What It Means**
Ternary expressions are often used for **inline returns or variable assignments** when a function or statement must choose between two outcomes without requiring a full `if-else` block.  
This keeps logic compact, improves readability for simple decisions, and is widely used in controllers, formatting helpers, and API responses.

Inline ternaries should be clear and short—if logic becomes complex, switch to a normal conditional structure.


##### **Example — Inline Return in Function**

```php
function statusLabel($active) {
    return $active
        ? 'Active'
        : 'Inactive';
}

echo statusLabel(true);
```

**Output**

```
Active
```

> Inline ternaries are excellent for small decision branches, but avoid embedding multiple calculations or nested expressions that reduce clarity.

---
## 07 Best Practical Use Cases

##### **What It Means**
The ternary operator shines in scenarios where **simple conditional value selection** improves clarity and reduces boilerplate code.  
It is ideal for **lightweight decision logic**, UI output formatting, and safe fallback handling—without the weight of multiple `if-else` blocks.  
Used wisely, it increases readability; used excessively, it can obscure intent.
##### **Example — Status Label Formatting**

```php
$statusCode = 1;

$label = ($statusCode === 1)
    ? 'Approved'
    : 'Pending';

echo $label;
```

**Output**

```
Approved
```

> Ternary is most effective for **display formatting, default values, permission checks, and simple return selection**—but avoid overuse when more complex branching exists.

Here's an improved, cleaner, and more professional version of your PHP ternary operators cheat sheet with better formatting, clearer explanations, modern PHP 8+ enhancements, and practical real-world context:

| Topic                                  | Purpose / When to Use                                              | Fast Example                                                                            | Output (example)        |
| -------------------------------------- | ------------------------------------------------------------------ | --------------------------------------------------------------------------------------- | ----------------------- |
| **Basic Ternary**                      | Choose between two values inline instead of if-else                | `$result = $score > 70 ? 'Pass' : 'Fail';`                                              | `Pass` or `Fail`        |
| **Nested Ternaries**                   | Multiple conditions in a single expression (use sparingly!)        | `$grade = $score >= 90 ? 'A' : ($score >= 80 ? 'B' : ($score >= 70 ? 'C' : 'F'));`      | `A` / `B` / `C` / `F`   |
| **Elvis Operator** (`?:`)              | Return value if truthy, otherwise default (deprecated in PHP 8.0+) | `$name = $user ?: 'Anonymous';`                                                         | `Anonymous`             |
| **Null Coalescing** (`??`)             | Return first **non-null** value (ignores false, "", 0, [])         | `$city = $data['city'] ?? 'New York';`                                                  | `New York`              |
| **Null Coalescing Chain**              | Check multiple fallbacks in order                                  | `$username = $input['name'] ?? $user->name ?? $_SESSION['user'] ?? 'Guest';`            | `Guest`                 |
| **Null Coalescing Assignment** (`??=`) | Assign value only if variable is null                              | `$settings['timeout'] ??= 30;`                                                          | Assigns `30` if not set |
| **Ternary in Templates/Views**         | Clean inline display logic in Blade, Twig, or plain PHP views      | `<span class="badge <?= $active ? 'bg-success' : 'bg-secondary' ?>">Status</span>`      | Applies correct class   |
| **With Functions / Returns**           | Shorten simple conditional returns                                 | `return $user->isAdmin() ? true : false;`                                               | `true` / `false`        |
| **Modern Replacement Pattern**         | Prefer null coalescing over old elvis/ternary in most cases        | Old: `$name = $user ? $user->name : 'Guest';`<br>New: `$name = $user->name ?? 'Guest';` | Same result             |
| **Real-World Best Practice**           | Use for simple conditions only. Avoid complex logic.               | Good: `$label = $isActive ? 'Online' : 'Offline';`<br>Bad: deeply nested ternaries      | Clear & maintainable    |

### Recommended Modern PHP (8.0+) Style
```php
// ✅ Best practices
$username  = $input['username'] ?? $user->getName() ?? 'Guest';
$status    = $isActive ? 'active' : 'inactive';
$role      = $user->role->value ?? 'user';
$limit    ??= 100;  // set default only if null
$color     = match(true) {
    $score >= 90 => 'success',
    $score >= 70 => 'warning',
    default      => 'danger',
}; // Consider match() for complex cases!
```

**Rule of Thumb**:  
Use ternary/`??` only when the expression stays readable on one line.  
For anything more complex → use a proper `if` or `match` expression.

Save this updated version — it's cleaner, future-proof, and reflects current PHP best practices! 🚀

---
# Interview questions 
#### **Q1. What is the ternary operator in PHP? Explain its purpose with a simple example.**

**Answer —**  
The ternary operator is a **compact inline conditional expression** used to choose between two values based on a boolean condition. It allows developers to replace small `if-else` blocks with a cleaner, more expressive one-line assignment, improving readability in templates, value selection, and functional-style return logic.

**Example**

```php
$age = 21;
$status = $age >= 18 ? 'Adult' : 'Minor';

echo $status; // Adult
```

**Doubt Table**

|Doubt|Yes|No|
|---|:-:|:-:|
|Is ternary mainly for short conditional value selection?|✔️||
|Does ternary replace large multi-branch decision trees?||✔️|
|Is ternary faster than if-else in real performance terms?||✔️ (difference negligible)|
#### **Q3. Convert this if-else to a ternary**

```php
if ($age >= 18) {
    $status = 'Adult';
} else {
    $status = 'Minor';
}
```

**Answer —**  
The equivalent ternary expression condenses the same logic into a single, clean assignment without changing behavior or readability.

**Converted Example**

```php
$status = $age >= 18 ? 'Adult' : 'Minor';
```

**Doubt Table**

|Doubt|Yes|No|
|---|:-:|:-:|
|Is this logically identical to the original if-else?|✔️||
|Does this reduce unnecessary lines of code?|✔️||
|Is ternary ideal for complex multi-branch logic?||✔️|


#### **Q4. Can a ternary operator be used directly in an echo statement? Show an example.**

**Answer —**  
Yes — ternary can be used inline inside `echo` expressions, making it useful for templates, UI rendering, and quick dynamic output.

**Example**

```php
echo $isLoggedIn ? 'Welcome back!' : 'Please log in';
```

**Doubt Table**

|Doubt|Yes|No|
|---|:-:|:-:|
|Is inline echo a common real-world use case?|✔️||
|Does inline ternary improve readability in templates?|✔️||
|Should inline ternaries return long multi-line blocks?||✔️|
#### **Q5. Is the ternary operator just syntactic sugar, or does it have real advantages over if-else?**

**Answer —**  
The ternary operator is not just syntactic sugar — it offers **real practical benefits** such as reducing verbosity, improving inline value assignment, and enabling expression-based logic where `if-else` would break flow (especially inside returns and view templates). While functionally equivalent, it leads to **cleaner and more maintainable code** when used appropriately.

**Example**

```php
return $user->active ? 'Enabled' : 'Disabled';
```

**Doubt Table**

|Doubt|Yes|No|
|---|:-:|:-:|
|Does ternary improve short-form conditional readability?|✔️||
|Does ternary replace complex branching logic?||✔️|
|Is ternary safer or faster than if-else inherently?||✔️|


#### **Q6. What is the null coalescing operator (`??`)? How does it differ from the ternary operator?**

**Answer —**  
The null coalescing operator (`??`) returns the **first operand if it exists and is not null**, otherwise returns the second operand. Unlike ternary, it **checks only for null / undefined values**, and does not evaluate truthiness or condition expressions — making it safer and cleaner for array access, request input handling, and default value assignment.

**Example**

```php
$username = $data['username'] ?? 'Guest';
```

**Key Difference**

- Ternary: requires a **condition**
    
- `??`: checks **null or undefined only** (no condition required)
    

**Doubt Table**

|Doubt|Yes|No|
|---|:-:|:-:|
|Does `??` avoid undefined index notices?|✔️||
|Does `??` evaluate complex conditions or truthiness like ternary?||✔️|
|Is `??` ideal for defaults & optional values?|✔️||


Here you go — **Q7 and Q8** in the same structured pattern:


#### **Q7. Give a practical example where using `??` is safer or cleaner than a ternary with `isset()`.**

**Answer —**  
`??` is cleaner and more reliable when accessing array keys, query params, or request payloads that might not exist. It prevents PHP notices like _“Undefined index”_ without needing verbose `isset()` checks or repetitive ternaries.

**Example**

```php
// Instead of:
$username = isset($_GET['user']) ? $_GET['user'] : 'Guest';

// Cleaner and safer:
$username = $_GET['user'] ?? 'Guest';
```

**Doubt Table**

|Doubt|Yes|No|
|---|:-:|:-:|
|Does `??` remove the need for `isset()`?|✔️||
|Does `??` prevent undefined index warnings?|✔️||
|Can `??` evaluate complex logical conditions?||✔️|


#### **Q8. What does the null coalescing assignment operator (`??=`) do? Provide a real-world use case.**

**Answer —**  
The `??=` operator assigns a value **only if** the variable is currently null or undefined. It is commonly used in configuration loading, defaults assignment, and request hydration without overwriting existing values.

**Example**

```php
$config['timezone'] ??= 'UTC';
```

If `$config['timezone']` is null or not set, it becomes `'UTC'`; otherwise, its original value is preserved.

**Real use case**

```php
$userSettings['theme'] ??= 'light';
```

**Doubt Table**

|Doubt|Yes|No|
|---|:-:|:-:|
|Does `??=` assign only when value is null/unset?|✔️||
|Does `??=` overwrite existing valid values?||✔️|
|Is `??=` useful for application defaults & configs?|✔️||


Got it — here are **Q9 and Q10** in the same formatted pattern:


#### **Q9. When should you avoid nested ternaries? Why are they considered a code smell?**

**Answer —**  
Nested ternaries should be avoided when they reduce clarity and make code harder to understand or maintain. Deeply nested ternaries are considered a **code smell** because they hide intent, complicate debugging, and break readability — especially in collaborative or production codebases.

**Example (Bad Practice)**

```php
$result = $x > 0 ? ($x > 10 ? 'big' : 'small') : 'negative';
```

**Better Alternative**

```php
if ($x > 0) {
    $result = $x > 10 ? 'big' : 'small';
} else {
    $result = 'negative';
}
```

**Doubt Table**

|Doubt|Yes|No|
|---|:-:|:-:|
|Are nested ternaries hard to read and maintain?|✔️||
|Are nested ternaries acceptable for complex decisions?||✔️|
|Should nested ternaries be replaced by clearer structures?|✔️||


#### **Q10. Rewrite this safely using null coalescing (avoid notices)**

```php
$name = isset($user['name']) ? $user['name'] : 'Guest';
```

**Answer —**  
This can be simplified using the null coalescing operator `??`, which safely returns a fallback value without causing undefined index warnings.

**Correct Replacement**

```php
$name = $user['name'] ?? 'Guest';
```

**Doubt Table**

|Doubt|Yes|No|
|---|:-:|:-:|
|Does `??` prevent undefined index notices?|✔️||
|Is this more concise than using `isset()` with ternary?|✔️||
|Does `??` evaluate complex conditions like ternary?||✔️|


Here you go — **Q11 and Q12** answered in the same structured format:


#### **Q11. Explain operator precedence issues with ternary and other operators. When must you use parentheses?**

**Answer —**  
Ternary operators have lower precedence than most arithmetic and comparison operators, which means expressions inside ternaries may not evaluate in the order developers expect. Using parentheses is essential when mixing ternary logic with operations like addition, concatenation, or function calls to avoid ambiguity and unintended results.

**Example — Without parentheses (wrong / confusing)**

```php
echo $x + 5 > 10 ? 'High' : 'Low'; // ambiguous interpretation
```

**Correct & explicit with parentheses**

```php
echo ($x + 5 > 10) ? 'High' : 'Low';
```

**Doubt Table**

|Doubt|Yes|No|
|---|:-:|:-:|
|Can precedence cause unexpected output in ternaries?|✔️||
|Should parentheses be used in compound expressions?|✔️||
|Is ternary always evaluated from left to right safely?||✔️|


#### **Q12. Compare ternary, null coalescing (`??`), and the `match()` expression. When should you prefer `match()`?**

**Answer —**  
While ternary is ideal for short binary decisions and `??` is perfect for null-safe default handling, `match()` should be used when you need **multiple deterministic outcomes**, **strict comparison**, and **exhaustive mapping** without fall-through or silent failures.

**Quick Comparison**

|Feature|Ternary|`??`|`match()`|
|---|:-:|:-:|:-:|
|Decision Type|True/False|Null/default|Multiple choices|
|Null Safety|❌|✔️|✔️|
|Strict Comparison|❌|❌|✔️|
|Enforced completeness|❌|❌|✔️|

**Example – When `match()` is better**

```php
$role = match ($user->role) {
    'admin'   => 'Dashboard',
    'editor'  => 'Editor Panel',
    'member'  => 'Home',
    default   => 'Guest Area',
};
```

**Doubt Table**

|Doubt|Yes|No|
|---|:-:|:-:|
|Is `match()` ideal for multi-state routing or role decisions?|✔️||
|Does `match()` guarantee exhaustive mapping?|✔️||
|Should `match()` replace ternary for simple two-way decisions?||✔️|


Here you go — **Q13 and Q14** in the same structured answer pattern:


#### **Q13. Is there any performance difference between ternary and if-else in PHP? Should performance guide your choice?**

**Answer —**  
No significant performance difference exists between ternary and `if-else` in modern PHP (8+). The opcode execution cost is nearly identical, so performance should **not** be the deciding factor. Readability, maintainability, and clarity of intent are far more important when choosing between them.

**Example Insight**

```php
// Both compile into very similar opcodes
$result = $flag ? 'A' : 'B';
```

**Doubt Table**

|Doubt|Yes|No|
|---|:-:|:-:|
|Is ternary meaningfully faster than if-else?||✔️|
|Should readability guide the decision instead of performance?|✔️||
|Is optimization by rewriting ternaries worthwhile?||✔️|


#### **Q14. In template files (e.g., Blade, Twig, or plain PHP views), is heavy use of ternaries acceptable? Defend your position.**

**Answer —**  
Yes — ternaries are widely accepted and recommended in templates **as long as they remain simple and readable**. They improve layout clarity, prevent scattering logic across blocks, and keep views concise. However, complex nested ternaries are discouraged, as they harm readability and violate separation of concerns.

**Example — Ideal use in a Blade template**

```php
<span class="{{ $active ? 'highlight' : 'muted' }}">
```

**Doubt Table**

|Doubt|Yes|No|
|---|:-:|:-:|
|Are simple ternaries acceptable and common in templates?|✔️||
|Are nested or multi-branch ternaries a readability issue?|✔️||
|Should complex logic stay inside controllers/services instead?|✔️||

Here you go — **Q15 and Q16** in the same structured answer style:


#### **Q15. Can function calls in a ternary cause unexpected side effects? Demonstrate with an example.**

**Answer —**  
Yes — placing function calls inside ternary branches can trigger unintended side effects, especially when the functions modify state, perform database operations, or execute expensive logic. Because ternary expressions evaluate one of the two branches only after the condition is checked, they can hide side effects that would be clearer in full `if-else` blocks.

**Example — Problematic use**

```php
// Both functions write to a log file or database
$result = $isVIP ? updateVipStatus() : updateRegularStatus();
```

**Better alternative**

```php
if ($isVIP) {
    $result = updateVipStatus();
} else {
    $result = updateRegularStatus();
}
```

**Doubt Table**

|Doubt|Yes|No|
|---|:-:|:-:|
|Can hidden side effects reduce code transparency?|✔️||
|Should ternary be avoided for calling state-modifying functions?|✔️||
|Is ternary safe for simple value returns?|✔️||


#### **Q16. How would you format a complex multi-level condition for maximum readability (ternary vs alternatives)?**

**Answer —**  
For multi-level or multi-branch logic, avoid nested ternaries and use clearer structures like `if-else`, `match()`, or dedicated strategy functions. Readability and maintainability should take priority over compactness — especially in team environments and long-term codebases.

**Example — Poor readability via nested ternary**

```php
$result = $score >= 90 ? 'A+' : ($score >= 80 ? 'A' : ($score >= 70 ? 'B' : 'C'));
```

**Better alternatives**

```php
// Using match()
$result = match (true) {
    $score >= 90 => 'A+',
    $score >= 80 => 'A',
    $score >= 70 => 'B',
    $score >= 60 => 'C',
    default      => 'F',
};
```

or

```php
// Using classic structure
if ($score >= 90) $result = 'A+';
elseif ($score >= 80) $result = 'A';
elseif ($score >= 70) $result = 'B';
else $result = 'C';
```

**Doubt Table**

|Doubt|Yes|No|
|---|:-:|:-:|
|Are nested ternaries a readability problem?|✔️||
|Is `match()` a better choice for multi-condition grading logic?|✔️||
|Should ternary be reserved for simple binary decisions?|✔️||


Here you go — **Q17 and Q18** in the same structured format:


#### **Q17. Real-world bug scenario**

```php
$discount = $user ? $user->isVIP() ? 20 : 10 : 0;
```

**What’s wrong here? How would you fix and improve it?**

**Answer —**  
The problem is **ambiguity caused by nested ternaries without parentheses**, making the evaluation order unclear and potentially returning the wrong discount if `$user` is null or if object access fails. It also becomes very hard to read and debug.

**Improved Version — using parentheses for correctness**

```php
$discount = $user
    ? ($user->isVIP() ? 20 : 10)
    : 0;
```

**Best Version — using `match()` for clarity**

```php
$discount = match (true) {
    !$user             => 0,
    $user->isVIP()     => 20,
    default            => 10,
};
```

**Doubt Table**

|Doubt|Yes|No|
|---|:-:|:-:|
|Is the original expression ambiguous and risky?|✔️||
|Is readability significantly improved with parentheses or match()?|✔️||
|Should complex pricing logic be written in nested ternaries?||✔️|


#### **Q18. Why was the short ternary (`?:` / elvis operator) deprecated and removed in PHP 8.0+? Was it a good decision?**

**Answer —**  
The short ternary created confusion and inconsistent behavior because it returned the **left-hand operand when true**, rather than explicitly returning a second operand. It led to subtle bugs in truthiness checks and was commonly misunderstood, making it error-prone in real-world code. Removing it improved clarity and reduced accidental misuse.

**Example — deprecated/removed style**

```php
$value = $input ?: 'default'; // confusing and inconsistent
```

**Safer replacement using null coalescing**

```php
$value = $input ?? 'default';
```

**Doubt Table**

|Doubt|Yes|No|
|---|:-:|:-:|
|Was `?:` responsible for confusing truthiness behavior?|✔️||
|Did its removal improve clarity and safety?|✔️||
|Is `??` the recommended modern replacement?|✔️||


Absolutely — here are the final two responses completing the full set:


#### **Q19. How does type juggling affect ternary expressions when comparing values like `0`, `''`, `'0'`, `false`, or `null`?**

**Answer —**  
Type juggling can cause ternary conditions to behave unexpectedly because PHP treats several values as **falsy**, even though they are not identical. This can result in incorrect outcomes if strict comparison isn’t used. Values such as `0`, `'0'`, `''`, `null`, and `false` can unintentionally produce the same branch outcome.

**Example — Problem caused by type juggling**

```php
$value = '0';
$result = $value ? 'True' : 'False'; // returns "False" (unexpected)
```

**Correct approach — using strict comparison**

```php
$result = ($value === '0') ? 'Literal zero' : 'Other';
```

**Doubt Table**

|Doubt|Yes|No|
|---|:-:|:-:|
|Can falsy values cause misleading ternary results?|✔️||
|Should strict operators be used to avoid confusion?|✔️||
|Does ternary automatically protect against type juggling?||✔️|


#### **Q20. Bonus Challenge: Convert this to the cleanest, most readable single-line version (without nested ternaries)**

```php
if ($score >= 90) $grade = 'A+';
elseif ($score >= 80) $grade = 'A';
elseif ($score >= 70) $grade = 'B';
elseif ($score >= 60) $grade = 'C';
else $grade = 'F';
```

**Answer —**  
The cleanest approach is to use `match(true)` which avoids nested ternaries and clearly maps each state.

**Best Single-Line Version**

```php
$grade = match (true) {
    $score >= 90 => 'A+',
    $score >= 80 => 'A',
    $score >= 70 => 'B',
    $score >= 60 => 'C',
    default      => 'F',
};
```

**Doubt Table**

|Doubt|Yes|No|
|---|:-:|:-:|
|Is `match(true)` more readable than nested ternaries?|✔️||
|Does match prevent ambiguous evaluation order?|✔️||
|Is match recommended for grading and tier logic?|✔️||
