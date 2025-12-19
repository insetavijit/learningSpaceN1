## **Switch / Match — Overview**

`switch` and `match` provide a cleaner, more structured alternative to long `elseif` chains when you are comparing **one value against multiple possible outcomes**. They improve readability, reduce cognitive load, and make decision trees easier to maintain—especially in systems where rules evolve frequently.

`match` (introduced in PHP 8) is **safer and more expressive**, returning values and enforcing exhaustive handling without fall-through side effects—far better for modern codebases.

Real-world systems like routing tables, pricing rules, notification dispatch, error mapping, and enum-based decisions rely heavily on these control structures.

##### **Topics to Learn — Switch / Match**

| **Topic**                   | **Brief Description**                                                                |
| --------------------------- | ------------------------------------------------------------------------------------ |
| **Basic Switch**            | Tests a variable against multiple case conditions to choose a single execution path. |
| **Break and Fall-through**  | Controls whether execution continues across cases or stops after a match.            |
| **Match Expression**        | Modern, strict, expression-based alternative returning a value with no fall-through. |
| **Return-based Branching**  | Cleaner inline decision assignment without large conditional blocks.                 |
| **Best Fit Use Cases**      | When multiple outcomes depend on a single evaluated value.                           |
| **Error Handling Patterns** | Mapping status codes, response objects, and standard result routing.                 |

---
#### 01 Basic Switch — Deep Exploration
##### 🧑‍🏫 **Mentor Interpretation**

> _This switch is acting as a controller router. Instead of writing 5 if-elseif blocks, you clearly map each user action to its logic. A junior developer often clutters code with long conditional ladders—switch turns this into clean, maintainable control flow._

|Concept|Professional Explanation|
|---|---|
|**Purpose**|To compare a variable against multiple predefined values and execute the matching block. More readable than long `if/elseif` chains.|
|**Best Use Case**|When checking a single variable against many fixed options, such as command handling, routing, menu choices, role checks.|
|**Flow Logic**|`switch` evaluates once → checks against case values → runs first matching block → stops unless `break` or `fallthrough`.|
|**Key Behavior**|Strict comparison (===) type sensitive in PHP 8+ for match, but not for traditional switch, allowing flexible matching.|
|**Common Mistake**|Forgetting `break`, causing unintentional fall-through execution.|
|**Real-World Analogy**|Like a **reception desk** directing you to the correct department based on your ticket number.|

---
#### 02 Break & Fall-Through in `switch`

##### **What It Means**

**Break and Fall-Through** control how execution flows inside a `switch` statement.
When a case matches, execution will **continue into the next case** unless a `break` stops it — this automatic continuation is called **fall-through**. Using `break` intentionally stops execution after the matched case, ensuring clean, isolated logic.
##### **Fall-Through Example (execution continues)**

```php
$role = 'editor';

switch ($role) {
    case 'admin':
        echo "Full access\n";
    case 'editor':
        echo "Edit access\n";   // falls through from admin
    case 'viewer':
        echo "View access\n";
}
```

**Output**

```
Edit access
View access
```

> Fall-through can cause serious logic or security problems when not intentional.
##### **Break Example (execution stops correctly)**

```php
$role = 'editor';

switch ($role) {
    case 'admin':
        echo "Full access\n";
        break;

    case 'editor':
        echo "Edit access\n";
        break;

    case 'viewer':
        echo "View access\n";
        break;
}
```

> `break` prevents unintended execution and ensures only the matched case runs.

##### **When to Use Which**

| Case                                           | Use                                                        |
| ---------------------------------------------- | ---------------------------------------------------------- |
| You want logic isolation per case              | **Use `break`**                                            |
| You want grouped cases with identical behavior | **Use intentional fall-through (commented or documented)** |
| You want guaranteed no fall-through errors     | **Use `match` instead of `switch`**                        |

##### **Modern Recommendation (2025+)**

```php
$access = match ($role) {
    'admin'  => "Full access",
    'editor' => "Edit access",
    'viewer' => "View access",
    default  => "No access",
};
```

> `match` eliminates fall-through entirely, improving safety and readability.

##### **ore Takeaway**

> **Fall-through continues execution into the next case.
> `break` stops execution after a match.
> Use fall-through intentionally, and prefer `match` for safe deterministic control.**

---
Here is the **final polished answer** for the topic:


#### 03 Match Expression

##### **What It Means**

A **match expression** is a modern, strict, expression-based alternative to `switch`.
It evaluates a value and **returns a result directly**, performs **strict comparison (`===`)**, and **never allows fall-through**. Every branch must be intentional, and missing cases can be detected early, improving correctness and safety.


##### **Example — Clean, Deterministic Value Mapping**

```php
$statusCode = 404;

$message = match ($statusCode) {
    200 => 'OK',
    201 => 'Created',
    400 => 'Bad Request',
    404 => 'Not Found',
    500 => 'Server Error',
    default => 'Unknown Status',
};

echo $message;
```

**Output**

```
Not Found
```

> `match` returns a value directly without needing break statements or external assignment.


##### **Key Features**

| Feature           | Description                                                 |
| ----------------- | ----------------------------------------------------------- |
| Strict Comparison | Uses `===`, preventing type coercion bugs                   |
| Expression-Based  | Always resolves to a value (string, object, enum, etc.)     |
| No Fall-Through   | Eliminates entire category of switch-based logic failures   |
| Exhaustive        | Can enforce handling all cases when `default` is omitted    |
| Clean & Readable  | Compact branching structure perfect for modern applications |


##### **When to Use**

| Use Case                                                 | Best Choice     |
| -------------------------------------------------------- | --------------- |
| Returning data based on a single discriminator           | `match`         |
| Compact, predictable decision tables                     | `match`         |
| Mapping values or enums to objects, messages, or results | `match`         |
| Safe branching with no accidental continuation           | `match`         |
| Legacy side-effect procedural logic                      | Prefer `switch` |


##### **Why Prefer `match` Over `switch`**

* No `break` keyword, no fall-through risk
* Safer for financial logic, permissions, routing, and enums
* Cleaner unit testing and easier reasoning
* Works well with PHPStan/Psalm for compile-time safety


##### **Core Takeaway**

> **`match` is the modern, strict, value-returning replacement for `switch`, eliminating fall-through errors and improving safety, readability, and correctness.**


Ready for the next topic?
**Options:**
A) `continue` vs `break` vs `return`
B) `match` with enums and exceptions
C) When *not* to use `match`
D) Legacy switch-to-match refactoring guide

Which one next? 🚀

Absolutely — here is the **final polished section** for:

#### 04 Return-based Branching**
##### **What It Means**
**Return-based branching** is a modern approach to decision-making where the branching structure directly **returns a value**, rather than modifying a variable first and returning later. It eliminates scattered return points, avoids large nested conditional blocks, and results in compact, readable, and predictable control flow. This pattern pairs naturally with `match` and functional-style architecture.
##### **Example — Inline Return vs Traditional Conditional**

######## **Traditional conditional (noisy, repetitive)**

```php
function responseMessage(int $code): string {
    if ($code === 200) {
        return 'OK';
    } elseif ($code === 404) {
        return 'Not Found';
    } elseif ($code === 500) {
        return 'Server Error';
    }

    return 'Unknown';
}
```

######## **Return-based branching (clean, single expression)**

```php
function responseMessage(int $code): string {
    return match ($code) {
        200 => 'OK',
        404 => 'Not Found',
        500 => 'Server Error',
        default => 'Unknown',
    };
}
```

**Output**

```
Not Found
```

> The function returns immediately from the expression, with **no variable assignment, no repetition, and no deep nesting**.
##### **Key Advantages**

| Benefit                      | Explanation                                     |
| ---------------------------- | ----------------------------------------------- |
| Cleaner & Readable           | Removes indentation and branching clutter       |
| Predictable Flow             | One entry, one exit — no hidden branching paths |
| Functional Style             | Easy to test, reason about, and refactor        |
| Reduces Bugs                 | Avoids mutation and missing return issues       |
| Works Perfectly with `match` | Expression-style inline value resolution        |
##### **When to Use**

| Scenario                                        | Why                                   |
| ----------------------------------------------- | ------------------------------------- |
| Creating API response values                    | produces clean consistent output      |
| DTO / View model selection                      | easier than nested if-else            |
| Mapping inputs to results                       | deterministic, expression-style logic |
| Avoiding mutable variables and multiple returns | reduces cognitive load                |
##### **Why Not Other Tools**

* `switch` requires assignment, break management, and is action-oriented rather than value-oriented.
* `if / elseif` spreads return statements and creates multiple exit paths that can confuse maintainers.
* `match(true)` works for range logic but is unnecessary when branching is simple and value-driven.


##### **Core Takeaway**

> **Return-based branching simplifies logic by returning values directly from decision expressions, eliminating large conditional blocks and improving clarity, testability, and safety.**

---
#### 05 Best Fit Use Cases

##### **What It Means**
**Best fit use cases** describe the ideal scenarios where a specific branching construct performs optimally. For decision logic where multiple outcomes depend on evaluating **one input value**, the most suitable tool is a **`match` expression** (or a well-structured `switch` in legacy contexts). This pattern centralizes branching around one discriminator and produces clear, deterministic results without complex boolean conditions or nested structures.
##### **Example — Best Fit Use Case for Single-Value Outcome Selection**

```php
function badgeColor(string $status): string {
    return match ($status) {
        'success' => 'green',
        'warning' => 'yellow',
        'error'   => 'red',
        'info'    => 'blue',
        default   => 'gray',
    };
}
```

**Output**

```
green
```

##### **Why This Is the Best Fit**

- The decision depends on **one evaluated value** (`$status`)
    
- There are **multiple possible outcomes**, each exclusive
    
- No relational comparisons or multiple conditions are required
    
- The logic maps cleanly into a decision table structure
    
- The branching result is a single return value, perfect for expression-based style
    
- Improves clarity and safety compared to long `if/elseif` chains
##### **When to Use This Pattern**

|Situation|Use|
|---|---|
|Single input controlling multiple outcomes|✔ Ideal|
|Mapping states, statuses, roles, categories|✔ Efficient|
|Returning result with no side effects|✔ Cleaner|
|Replacing complex and nested condition blocks|✔ Readable|
|Preparing code for enum-based architecture|✔ Modern|
##### **Why Not the Other Tools**

- `if / elseif` becomes long, repetitive, and harder to maintain when states increase.
    
- `switch` risks fall-through without careful break placement and feels verbose with value mapping.
    
- `match(true)` adds unnecessary complexity since no range or boolean logic is present.
##### **Core Takeaway**

> **Use `match` when multiple exclusive outcomes depend on evaluating one single value — the result is clean, readable, deterministic, and future-proof.**
---
#### 06. Error Handling Patterns

##### **What It Means**
**Error handling patterns** define structured ways to transform system status codes, exception states, or internal result objects into consistent responses. Instead of scattering `if/elseif` or deeply nested condition checks across the codebase, modern architectures centralize this logic using `match` expressions to route outcomes cleanly to UI messages, API responses, or error objects. This ensures predictable behavior and simplifies debugging, maintenance, and extensibility.

##### **Example — Routing Status Codes to Error Responses**

```php
function errorResponse(int $code): string {
    return match ($code) {
        200 => 'OK',
        400 => 'Bad Request — invalid input received.',
        401 => 'Unauthorized — login required.',
        403 => 'Forbidden — insufficient permissions.',
        404 => 'Resource Not Found.',
        500 => 'Internal Server Error.',
        default => 'Unhandled Error — please try again later.',
    };
}
```

**Output**

```
Bad Request — invalid input received.
```

##### **Why This Pattern Works Well**

* All error-to-message mappings live in one clean location, not spread across the app
* `match` enforces strict handling and prevents silent missing error branches
* Avoids accidental fall-through or ambiguous behavior common in `switch`
* Makes unit testing and observability simple and deterministic
* Supports functional, predictable API response pipelines
##### **Best Fit Use Cases**

| Scenario                                   | Pattern Benefit                                |
| ------------------------------------------ | ---------------------------------------------- |
| HTTP response mapping                      | Consistent error presentation and traceability |
| Exception → user message conversion        | Safe abstractions for UI layers                |
| Result objects in service layers           | Stable contract between domain and interface   |
| Routing workflows based on status          | No fragile conditional branches                |
| API standardization (success/error models) | Clear predictable handling across endpoints    |
##### **Why Not the Other Tools**
* `switch` risks missing `break` and leaking additional cases, causing incorrect messaging
* `if / elseif` quickly becomes long and difficult to maintain as response types grow
* `match(true)` is unnecessary when conditions depend on exact values rather than ranges
##### **Core Takeaway**

> **Use `match` for error handling patterns where status codes or result objects must reliably map to standardized responses — ensuring clarity, safety, and consistency across the application.**

### **Real-World Practical Example**

```php
<?php
declare(strict_types=1);

$action = $_GET['action'] ?? 'view';

switch ($action) {
    case 'view':
        echo "Displaying the page...";
        break;

    case 'edit':
        echo "Opening editor...";
        break;

    case 'delete':
        echo "Deleting record...";
        break;

    default:
        echo "Unknown action, redirecting to home.";
}
```

---
### **Interview Questions
#### **Q1. When should I use `match` instead of `switch` in PHP?**

**Answer —**  
In modern PHP development (2024–2025+), **`match` should be the default branching construct** whenever you’re selecting a result from a single input. It provides **strict type safety (`===`)**, returns values directly, and prevents fall-through bugs. It also works perfectly with enums and static analysis to guarantee exhaustive handling.  
`switch` is now largely **legacy**, suitable only for **pre-PHP-8 refactoring, simple CLI tools**, and code paths performing **side effects instead of returning values**.

**Example**

```php
// 2025 production standard using match
$phrase = match ($statusCode) {
    200 => 'OK',
    201 => 'Created',
    400 => 'Bad Request',
    401 => 'Unauthorized',
    403 => 'Forbidden',
    404 => 'Not Found',
    422 => 'Unprocessable Entity',
    429 => 'Too Many Requests',
    500 => 'Internal Server Error',
    default => throw new UnexpectedValueException("Unhandled status $statusCode"),
};
```

**Doubt Table (Yes / No)**

|Doubt|Yes|No|
|---|:-:|:-:|
|Is `match` the successor to `switch`?|✔️||
|Does `match` prevent loose type comparisons?|✔️||
|Is `switch` considered technical debt in 2025+?|✔️||
|Are modern frameworks shifting to `match`?|✔️||
|Should switches be refactored during cleanup?|✔️||

---

#### **Q2. How do I make `match` enforce exhaustive branching?**

**Answer —**  
Remove `default` and use a **backed enum** or a **closed constant set**. Missing branches will trigger `UnhandledMatchError` at runtime, and tools like **PHPStan/Psalm** catch it at CI time, giving you real compile-time validation and zero silent failures.

**Example**

```php
enum PaymentStatus: string {
    case SUCCESS; case FAILED; case PENDING; case REFUNDED;
}

function badge(PaymentStatus $status): string {
    return match ($status) {
        PaymentStatus::SUCCESS  => 'green',
        PaymentStatus::FAILED   => 'red',
        PaymentStatus::PENDING  => 'yellow',
        PaymentStatus::REFUNDED => 'blue',
        // No `default` = fully exhaustive
    };
}
```

**Doubt Table**

|Doubt|Yes|No|
|---|:-:|:-:|
|Will new enum cases break CI if missed?|✔️||
|Is `UnhandledMatchError` safer than silent bugs?|✔️||
|Is this pattern industry-standard in 2025?|✔️||

---

#### **Q3. How do I group multiple cases in `match`?**

**Answer —**  
Use **comma-separated branch grouping**, the modern, explicit, and type-safe replacement for brittle `switch` fall-through behavior.

**Example**

```php
$segment = 'student';   // incoming customer type (example input)

// Modern 2025+ preferred approach — match expression
$discount = match ($segment) {
    'student', 'senior', 'veteran', 'disabled' => 0.30,
    'employee', 'partner'                      => 0.15,
    'newsletter_subscriber'                    => 0.05,
    default                                    => 0.00,
};

echo "Customer segment: {$segment}\n";
echo "Discount: " . ($discount * 100) . "%\n";
```

**Doubt Table**

|Doubt|Yes|No|
|---|:-:|:-:|
|Is group syntax supported since PHP 8.0?|✔️||
|Is it safer than switch fall-through?|✔️||
|Should fall-through be banned in reviews?|✔️||

---

#### **Q4. Can `match` safely return different types?**

**Answer —**  
Yes — combine **union return types** with **`match(true)` conditional rules** to cleanly return multiple possible types while keeping strict type inference and validation.

**Example**

```php
function normalize(mixed $v): string|int|float|array {
    return match (true) {
        is_null($v)     => [],
        is_numeric($v)  => $v + 0,
        is_string($v)   => trim($v),
        is_array($v)    => array_values($v),
        default         => throw new InvalidArgumentException(),
    };
}
```

**Doubt Table**

|Doubt|Yes|No|
|---|:-:|:-:|
|Is `match(true)` clean and idiomatic now?|✔️||
|Should polymorphism be preferred for >5 rules?|✔️||

---

#### **Q5. How do I replace the old `switch(true)` pattern?**

**Answer —**  
Use **`match(true)`**, the officially recommended and expressive successor in PHP 8.1+, eliminating accidental fall-through and inconsistent comparisons.

**Example**

```php
$plan = match (true) {
    $u < 10      => 'starter',
    $u < 100     => 'pro',
    $u < 1000    => 'business',
    $hasCustom() => 'enterprise',
    default      => 'free',
};
```

**Doubt Table**

|Doubt|Yes|No|
|---|:-:|:-:|
|Is `switch(true)` obsolete?||✔️|
|Is `match(true)` the modern standard?|✔️||
|Should switch(true) be refactored in 2025?|✔️||

Perfect — continuing **Q6–Q10** with the same premium 2025 architect-level standard, structure, tone, and formatting.

---

#### **Q6. Where is `match` _not_ a good fit, and when should I still use `switch`?**

**Answer —**  
`match` should not be used when the intention of the code is **side effects** rather than returning a value — such as printing output, writing logs, modifying external state, or performing procedural actions. `switch` remains acceptable for **simple, linear CLI scripts**, integration glue code, and maintenance of **legacy codebases** where rewriting would introduce risk without benefit.  
In high-quality modern systems, `switch` is treated as **an imperative control mechanism**, while `match` is **a pure expression for deterministic output**.

**Example**

```php
// CLI maintenance script — switch is acceptable
switch ($command) {
    case 'clear-cache': shell_exec('php artisan cache:clear'); break;
    case 'restart-worker': shell_exec('supervisorctl restart queue'); break;
    default: echo "Unknown command\n";
}
```

**Doubt Table**

|Doubt|Yes|No|
|---|:-:|:-:|
|Is switch still useful for procedural side effects?|✔️||
|Is switch appropriate in throwaway CLI tools?|✔️||
|Should switch ever appear in core domain logic?||✔️|

---

#### **Q7. Can `match` return objects, enums, DTOs, or exceptions?**

**Answer —**  
Yes — `match` excels at **domain-driven design use cases** by returning strongly-typed objects, backed enums, or throwing exceptions directly inside cases. This allows eliminating long procedural branching and replacing it with clean, immutable decision logic.

**Example**

```php
return match ($role) {
    'admin'   => new AdminDashboardView(),
    'editor'  => new EditorDashboardView(),
    'viewer'  => new ViewerDashboardView(),
    default   => throw new DomainException("Invalid role: $role"),
};
```

**Doubt Table**

|Doubt|Yes|No|
|---|:-:|:-:|
|Can match return objects and value-objects?|✔️||
|Is match useful for DDD state transitions?|✔️||
|Should match be used in domain services instead of switch?|✔️||

---

#### **Q8. Can `match` replace complex nested if/elseif chains?**

**Answer —**  
Yes — replacing nested branching with `match(true)` dramatically improves readability, testability, and maintainability. It forces rule clarity and eliminates duplicated conditions and inconsistent control paths typical in deeply nested `if` structures.

**Example**

```php
$result = match (true) {
    $age < 13                 => 'child',
    $age < 18                 => 'teen',
    $age < 60                 => 'adult',
    $age >= 60                => 'senior',
    default                   => throw new RangeException(),
};
```

**Doubt Table**

|Doubt|Yes|No|
|---|:-:|:-:|
|Is match(true) cleaner than deeply nested if/elseif?|✔️||
|Does match improve testability and intent clarity?|✔️||
|Should complex branching be moved to match?|✔️||

---

#### **Q9. Is `match` faster than `switch` in real-world performance?**

**Answer —**  
Yes — `match` is compiled more efficiently and optimized for expression evaluation. It typically outperforms `switch` because it avoids loose comparison overhead and short-circuit complexity. In production benchmark suites (Symfony, Laravel Octane, RoadRunner, Swoole), evaluating returning `match` blocks consistently beats equivalent `switch` implementations.

**Example**

```php
// performance-oriented branching
$priority = match ($event->type) {
    'alert' => 1,
    'warning' => 2,
    'info' => 3,
    default => 4,
};
```

**Doubt Table**

|Doubt|Yes|No|
|---|:-:|:-:|
|Is match generally faster or equal in speed to switch?|✔️||
|Is match optimized for expression evaluation?|✔️||
|Does switch have any performance advantage in 2025?||✔️|

---

#### **Q10. Can `match` simplify routing, workflow orchestration, and state machines?**

**Answer —**  
Yes — `match` is widely used in modern routing kernels, event dispatchers, workflow engines, and finite state machines because it guarantees that **all states are mapped** and missing states cannot silently slip through. This makes it an ideal core mechanism for workflow-heavy platforms and reactive architectures.

**Example**

```php
$nextStep = match ($order->status) {
    'new'       => 'process-payment',
    'paid'      => 'prepare-shipment',
    'shipped'   => 'notify-delivery',
    'delivered' => 'request-feedback',
    default     => throw new LogicException('Unknown order state'),
};
```

**Doubt Table**

|Doubt|Yes|No|
|---|:-:|:-:|
|Is match ideal for workflow and FSM design?|✔️||
|Does match enforce complete state mapping?|✔️||
|Should routing/state engines avoid switch in 2025+?|✔️||

---
### **Why Not Others**
---
#### **Mapping one value to one outcome — `match`**

**Problem ( > 25 words )**  
In an e-commerce platform, the backend returns order status codes like `new`, `paid`, or `shipped`, but the customer-facing dashboard must display friendly readable labels. Each code must map to exactly one output without ambiguity. The logic needs to remain simple, predictable, and easy to update as more statuses are introduced, avoiding large conditional blocks and reducing the risk of subtle branching errors.

```php
$label = match ($order->status) {
    'new'       => 'Awaiting Payment',
    'paid'      => 'Processing',
    'shipped'   => 'On the Way',
    'delivered' => 'Completed',
    default     => 'Unknown',
};
```

**Why not the other tools (expanded)**  
`switch` relies on `break` statements and can accidentally fall through, making the mapping unsafe and harder to maintain in growing codebases.  
`if / elseif` becomes repetitive, verbose, and harder to modify as additional statuses are introduced.  
`match(true)` adds unnecessary complexity, since this scenario is purely equality-based, not condition or range driven.

---
#### **Returning a single value cleanly — `match`**

**Problem ( > 25 words )**  
A REST API must return different MIME type headers depending on the format requested by the client application. The response must always resolve to exactly one valid content-type string, without scattered return statements or temporary variables that increase complexity. The logic must remain clean, predictable, and simple to extend when new formats are added, ensuring consistent behavior across environments and easier unit testing.

```php
$responseType = match ($request->format()) {
    'json' => 'application/json',
    'xml'  => 'application/xml',
    'html' => 'text/html',
    default => 'text/plain',
};
```

**Why not the other tools (expanded)**  
`switch` requires extra boilerplate like variable assignment and multiple `break` calls, which makes the control flow harder to follow.  
`if / elseif` spreads return statements across branches, damaging readability and complicating testing.  
`match(true)` offers no benefit because no conditional evaluation or range logic is involved.

---

#### **Multiple grouped cases without fall-through — `match`**

**Problem ( > 25 words )**  
A pricing system must apply the same discount level to several different customer categories, such as student, senior, or veteran. Previously, the implementation using `switch` caused incorrect billing when a developer forgot a break statement, leading to silent logical errors. The discount determination must be safe, compact, and grouped intentionally without relying on fragile break control or repetitive code patterns.

```php
$discount = match ($customer->group) {
    'student', 'senior', 'veteran', 'disabled' => 0.30,
    'employee', 'partner'                      => 0.15,
    default                                    => 0.00,
};
```

**Why not the other tools (expanded)**  
`switch` grouping depends on the developer remembering where to place breaks, which can easily introduce subtle and dangerous billing bugs.  
`if / elseif` produces repetitive comparison blocks that become harder to maintain as groups grow or merge.  
`match(true)` adds unnecessary complexity because no relational conditions are required.

---
Absolutely — continuing with the **remaining scenarios** (Q4–Q6), in the **same format, tone, and structure** as the previous ones.

---

#### **Executing commands or side-effects — `switch`**

**Problem ( > 25 words )**  
A server maintenance CLI tool needs to perform operational tasks such as clearing cache, restarting worker processes, or checking service status. These operations do not return a computed value; instead they trigger external commands and print output to the terminal. The behavior is procedural and action-oriented, so clarity of execution order is more important than expression-style value handling or compact return logic.

```php
switch ($command) {
    case 'clear-cache': system('php artisan cache:clear'); break;
    case 'restart-workers': system('supervisorctl restart queue'); break;
    case 'status': system('supervisorctl status'); break;
    default: echo "Unknown command\n";
}
```

**Why not the other tools (expanded)**  
`match` is designed to return values rather than perform side-effect operations, making intent harder to read when executing commands.  
`if / elseif` becomes unwieldy and harder to scale when many command types are added.  
`match(true)` provides no benefit because no range or conditional evaluation exists — only action triggering.

---

#### **Complex conditional rules — `if / elseif`**

**Problem ( > 25 words )**  
A subscription management system determines access rights based on multiple variables including user role, subscription plan, and account age. The decision logic relies on relational comparisons and combined boolean conditions, not a single selector value. The result must be easy to understand, trace, and debug, especially when rules change frequently based on evolving business policies.

```php
if ($user->role === 'admin') {
    $access = 'full';
} elseif ($plan === 'premium' && $user->days_active > 30) {
    $access = 'extended';
} elseif ($plan === 'basic') {
    $access = 'limited';
} else {
    $access = 'none';
}
```

**Why not the other tools (expanded)**  
`match` supports only one compared input, making it unsuitable for logic requiring multiple interdependent conditions.  
`switch` cannot handle relational operators like `<`, `>`, or `&&`, so intent becomes unclear and forced.  
`match(true)` may work, but becomes less readable and harder to maintain if multiple combined rules are required.

---

#### **Range / threshold / scoring tiers — `match(true)`**

**Problem ( > 25 words )**  
A financial decision engine assigns a risk tier based on credit score ranges. Each score band maps to a specific risk level, and ranges may evolve over time. The logic must remain linear, readable, and easy to modify as business policy adjusts. Using nested `if` statements creates deep indentation and increases risk of mistakes, whereas structured range mapping keeps the model clean and explicit.

```php
$risk = match (true) {
    $score < 500        => 'high',
    $score < 650        => 'medium',
    $score < 750        => 'low',
    $score >= 750       => 'very low',
    default             => 'unknown',
};
```

**Why not the other tools (expanded)**  
`match` only performs exact comparisons, so it cannot express numeric ranges or progression.  
`switch` cannot evaluate relational operators, making it impossible to represent intervals cleanly.  
`if / elseif` works but becomes bulky and difficult to refactor as more scoring tiers are added.

---
#### **One-Line Selection Rule (Expanded)**

> **Use `match` for deterministic value mapping, `switch` for executing actions, `if` for multi-factor logic, and `match(true)` for clean range-based tiering.**

---
