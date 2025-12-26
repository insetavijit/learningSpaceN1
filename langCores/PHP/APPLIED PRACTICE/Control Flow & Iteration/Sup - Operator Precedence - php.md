## **8. Operator Precedence – 2025 Definitive Cheat Sheet**

_(Memorize this once and never get bitten again)_

### **Overview**

Operator precedence determines **which part of an expression PHP evaluates first** when multiple operators appear together. Most unexpected bugs in conditions, math expressions, string concatenations, and nested ternaries come from assuming the wrong evaluation order.  
This cheat sheet ranks every operator from highest to lowest priority, explains associativity, and highlights real-world traps that hurt even experienced developers.  
Master this once — and write expressions that behave exactly the way you intend them to.

---

| Level | Operators (highest → lowest)                                                   | Associativity   |
| ----- | ------------------------------------------------------------------------------ | --------------- |
| 1     | `!` `~` `+` `-` `(int)` `(float)` `(string)` `(array)` `(object)` `(bool)` `@` | Right           |
| 2     | `**`                                                                           | Right           |
| 3     | `*` `/` `%`                                                                    | Left            |
| 4     | `+` `-` `.`                                                                    | Left            |
| 5     | `<<` `>>`                                                                      | Left            |
| 6     | `<` `<=` `>` `>=`                                                              | Non-associative |
| 7     | `==` `!=` `===` `!==` `<>` `<=>`                                               | Non-associative |
| 8     | `&`                                                                            | Left            |
| 9     | `^`                                                                            | Left            |
| 10    | `                                                                              | `               |
| 11    | `&&`                                                                           | Left            |
| 12    | `                                                                              |                 |
| 13    | `??`                                                                           | Right           |
| 14    | `? :` (ternary)                                                                | Right           |
| 15    | `= += -= *= /= .= ??= **=` etc.                                                | Right           |
| 16    | `and`                                                                          | Left            |
| 17    | `xor`                                                                          | Left            |
| 18    | `or`                                                                           | Left            |

---
## **Level 1 — Unary Operators (Top Priority!)**

Unary operators work on **just one value** and always execute **before** any arithmetic, comparison, or logical expression.
Think of them as **warm-up moves before the main game** — they prepare values for real calculations.

#### **Quick Reference Table**

| **Operator**                             | **Purpose / What It Does**                  | **Example & Result**      |
| ---------------------------------------- | ------------------------------------------- | ------------------------- |
| `!`                                      | Turns **true → false** and **false → true** | `!true` → `false`         |
| `~`                                      | Flips all **binary bits**                   | `~0` → `-1`               |
| `+`                                      | Converts a value to **number form**         | `+"5"` → `5`              |
| `-`                                      | **Negates** a number                        | `-5` → `-5`               |
| `(int)`                                  | Converts to an **integer**                  | `(int)"50"` → `50`        |
| `(float)`                                | Converts to a **decimal**                   | `(float)"3.1"` → `3.1`    |
| `(string)` `(array)` `(object)` `(bool)` | **Force type changes**                      | `(bool)""` → `false`      |
| `@`                                      | **Hides errors** (avoid using)              | `@file_get_contents(...)` |

#### **Kid-Friendly Example (One Simple Block)**

```php
$lightOn = false; // Light is OFF
$chairs  = "4";   // Number stored as text

// Unary operators happen BEFORE the math
$result = !$lightOn + (int)$chairs - -1;

/*
Step-by-step:
!$lightOn     => true => 1   (NOT flips OFF to ON)
(int)$chairs  => 4           (text becomes number)
- -1          => +1          (double negative = plus)

Final total: 1 + 4 + 1 = 6
*/

echo $result; // 6 chairs ready when the lights turn ON
```

#### **Remember This**

> Unary operators **run first** and **change values upfront**.
> When an expression looks confusing — **use parentheses to stay clear**.

Would you like a **visual flow chart** version or a **real-life analogy comic style** version next? 🎨📦

Understood. I will rewrite **each level in the exact same structure** as Level 1 — same headings, same flow, same kid-friendly block, same reminder section.
Here is **Level 2** rewritten in that format. Confirm, and I’ll continue Levels 3–18 the same way.

## **Level 2 — Exponentiation (`**`)**

Exponentiation is used to **raise a number to a power** (multiply it by itself repeatedly).
It runs **before multiplication, division, addition, and most other math**.
It is **right-associative**, meaning it calculates **from right → left**.

#### **Quick Reference Table**

| **Operator** | **Purpose / What It Does**         | **Example & Result**  |
| ------------ | ---------------------------------- | --------------------- |
| `**`         | Raises a number to a **power**     | `2 ** 3` → `8`        |
|              | Works **right-to-left (not left)** | `2 ** 3 ** 2` → `512` |

#### **Kid-Friendly Example (One Simple Block)**

```php
$cookies = 2;

// Exponentiation happens right-to-left
$result = $cookies ** 3 ** 2;

/*
Step-by-step:
3 ** 2  = 9   (power first on the right)
2 ** 9  = 512 (two becomes super-big!)

Final result = 512 cookies!
*/

echo $result; // A mountain of cookies!
```

#### **Remember This**

> Exponentiation is **right-associative**, not left.
> `2 ** 3 ** 2` means `2 ** (3 ** 2)` → **512**, not **64**.

Would you like me to continue with **Level 3 — Multiplication, Division & Mod** in the same format now? ✨

## **Level 3 — Multiply / Divide / Mod**

These operators handle the **core math operations** and run **before addition and subtraction**.  
They work **left-to-right**, solving as they move across the expression.

#### **Quick Reference Table**

|**Operator**|**Purpose / What It Does**|**Example & Result**|
|---|---|---|
|`*`|Multiply numbers|`5 * 2` → `10`|
|`/`|Divide numbers|`10 / 2` → `5`|
|`%`|Remainder after division|`10 % 3` → `1`|

#### **Kid-Friendly Example (One Simple Block)**

```php
$apples = 10;
$friends = 5;

// multiplication/division happens BEFORE plus/minus
$result = 2 + $apples / $friends * 3;

/*
Step-by-step:
$apples / $friends = 10 / 5 = 2
2 * 3 = 6
2 + 6 = 8
*/

echo $result; // We share and still multiply happiness: 8!
```

#### **Remember This**

> Multiply, divide, and mod happen **before any + or -**.  
> If mixing operations, **use parentheses for safety**.

## **Level 4 — Add/Subtract/Concatenate**

These operators handle basic math and join text.  
They work **left-to-right**, like reading a sentence.

#### **Quick Reference Table**

|**Operator**|**Purpose / What It Does**|**Example & Result**|
|---|---|---|
|`+`|Add numbers|`3 + 4` → `7`|
|`-`|Subtract numbers|`10 - 6` → `4`|
|`.`|Join strings (concatenate)|`"A" . "B"` → `"AB"`|

#### **Kid-Friendly Example (One Simple Block)**

```php
$first = "Ice";
$second = "Cream";

// Strings join with a dot
$flavor = $first . " " . $second;

echo $flavor; // Ice Cream 🍦
```

#### **Remember This**

> `.` is **string joining**, not decimal or dot.  
> `"a" . "b"` becomes `"ab"` — not math!

## **Level 5 — Bitshift (`<<` `>>`)**

Bitshift operators move binary bits **left or right**, like shifting numbers on a conveyor belt.  
Mostly used in **low-level logic**, rarely in normal apps.

#### **Quick Reference Table**

|**Operator**|**Purpose / What It Does**|**Example & Result**|
|---|---|---|
|`<<`|Shift bits left (×2 each shift)|`8 << 1` → `16`|
|`>>`|Shift bits right (÷2 each shift)|`8 >> 1` → `4`|

#### **Kid-Friendly Example (One Simple Block)**

```php
$number = 4;

$result1 = $number << 1; // 4 * 2 = 8
$result2 = $number >> 1; // 4 / 2 = 2

echo $result1 . ", " . $result2; // 8, 2
```

#### **Remember This**

> Bitshifts are rare unless dealing with **binary data or hardware**.  
> If unsure — **you probably don’t need them**.

## **Level 6 — Comparisons (`<` `<=` `>` `>=`)**

These operators compare values and return **true or false**.  
They have **no associativity**, so **never chain comparisons**.

#### **Quick Reference Table**

|**Operator**|**Purpose / What It Does**|**Example & Result**|
|---|---|---|
|`<` `<=` `>` `>=`|Compare values|`10 > 3` → `true`|

#### **Kid-Friendly Example (One Simple Block)**

```php
$score = 85;

$passed = $score >= 60; // 85 >= 60 = true

echo $passed; // true (You passed the exam!)
```

#### **Remember This**

> **Do NOT chain comparisons.**  
> `5 < 10 < 3` → ❌ **invalid**

## **Level 7 — Equality **

These operators check if values are **equal or different**.  
Some are **loose** (allow type conversion) and others are **strict**.

#### **Quick Reference Table**

|**Operator**|**Purpose / What It Does**|**Example & Result**|
|---|---|---|
|`==` `!=`|Loose comparison (type-flexible)|`"5" == 5` → `true`|
|`===` `!==`|Strict comparison (no type change)|`"5" === 5` → `false`|
|`<>`|Not equal alternative|`5 <> 6` → `true`|
|`<=>`|Spaceship: returns -1 / 0 / 1|`5 <=> 5` → `0`|

#### **Kid-Friendly Example (One Simple Block)**

```php
$ageText = "10";
$ageNumber = 10;

echo $ageText == $ageNumber;  // true (values match)
echo $ageText === $ageNumber; // false (types differ)
```

#### **Remember This**

> **Use `===` almost always** — it avoids confusing type juggling.  
> `==` can give weird and risky results.

Great — here are the **next 5 levels (Level 8 → Level 12)** in **the exact same structure** as before.

## **Level 8 — Bitwise AND (`&`)**

Bitwise AND compares two numbers bit-by-bit and keeps a **1 only when both bits are 1**.  
Used mainly in **low-level programming, flags, and permissions**.

#### **Quick Reference Table**

|**Operator**|**Purpose / What It Does**|**Example & Result**|
|---|---|---|
|`&`|Bit-by-bit AND operation|`5 & 3` → `1`|

#### **Kid-Friendly Example (One Simple Block)**

```php
$a = 5; // 101 in binary
$b = 3; // 011 in binary

$result = $a & $b;

/*
Binary compare:
101
011
----
001  = 1
*/

echo $result; // 1 (only one light is ON at the same time)
```

#### **Remember This**

> Bitwise AND keeps a bit only if **both sides have it**.  
> Mostly used in **permission flags or hardware** work.

## **Level 9 — Bitwise XOR (`^`)**

Bitwise XOR returns **1 only when bits are different**.  
Useful when toggling values or cryptography.

#### **Quick Reference Table**

|**Operator**|**Purpose / What It Does**|**Example & Result**|
|---|---|---|
|`^`|Bitwise XOR (exclusive OR)|`5 ^ 3` → `6`|

#### **Kid-Friendly Example (One Simple Block)**

```php
$a = 5; // 101
$b = 3; // 011

$result = $a ^ $b;

/*
Binary compare:
101
011
----
110 = 6
*/

echo $result; // 6 (different switches light up)
```

#### **Remember This**

> XOR is **true only when values differ**.  
> If both bits match → result becomes `0`.

## **Level 10 — Bitwise OR (`|`)**

Bitwise OR keeps a **1 when either bit is 1**.  
Think: **if any switch is ON, light turns ON**.

#### **Quick Reference Table**

|**Operator**|**Purpose / What It Does**|**Example & Result**|
|---|---|---|
|`|`|Bitwise OR|

#### **Kid-Friendly Example (One Simple Block)**

```php
$a = 5; // 101
$b = 3; // 011

$result = $a | $b;

/*
101
011
----
111 = 7
*/

echo $result; // 7 lights power together!
```

#### **Remember This**

> **Either side true = result true** (bitwise version).  
> Useful in **access flags & permissions**.

## **Level 11 — Logical AND (`&&`)**

Logical AND checks **true/false conditions**, stopping early if false is found (**short-circuiting**).

#### **Quick Reference Table**

|**Operator**|**Purpose / What It Does**|**Example & Result**|
|---|---|---|
|`&&`|All conditions must be true|`true && false && true` → `false`|

#### **Kid-Friendly Example (One Simple Block)**

```php
$hasTicket = true;
$hasID = false;

$canEnter = $hasTicket && $hasID;

/*
Both must be true to enter
true && false = false
*/

echo $canEnter; // false (You forgot your ID!)
```

#### **Remember This**

> **Stops immediately** when it finds `false`.  
> Used for **strict rule checking**.

## **Level 12 — Logical OR (`||`)**

Logical OR needs **only one true** to return true.  
Also **short-circuits**, stopping early if true appears.

#### **Quick Reference Table**

|**Operator**|**Purpose / What It Does**|**Example & Result**|
|---|---|---|
|`||`|

#### **Kid-Friendly Example (One Simple Block)**

```php
$hasUmbrella = false;
$hasRaincoat = true;

$stayDry = $hasUmbrella || $hasRaincoat;

/*
Only one needs to be true to stay dry
false || true = true
*/

echo $stayDry; // true – still dry in the rain!
```

#### **Remember This**

> Logical OR **stops when it finds true**.  
> Great for **fallback or default options**.

Absolutely — here are the **remaining levels (13 → 18)** in the **exact same structure**.

## **Level 13 — Null Coalescing (`??`)**

The null coalescing operator returns the **first value that is NOT null**.  
It is perfect for **default values** when variables might be empty or missing.  
It is **right-associative**, checking values from **left → right**.

#### **Quick Reference Table**

|**Operator**|**Purpose / What It Does**|**Example & Result**|
|---|---|---|
|`??`|Use value if set, else fallback|`$name = $user ?? "Guest"` → `"Guest"`|

#### **Kid-Friendly Example (One Simple Block)**

```php
$userName = null;

// null coalescing checks left to right
$finalName = $userName ?? "Player";

/*
$userName is null
So "Player" is selected
*/

echo $finalName; // Player joins the game!
```

#### **Remember This**

> Use `??` for **defaults** when something might be missing.  
> Great for user input, optional configs, and safety values.

## **Level 14 — Ternary (`?:`)**

The ternary operator is a **shortcut for if/else** and reads like a sentence:  
**condition ? value_if_true : value_if_false**  
It is **right-associative**, so nested ternaries evaluate from right to left.

#### **Quick Reference Table**

|**Operator**|**Purpose / What It Does**|**Example & Result**|
|---|---|---|
|`?:`|fast decision / choose value|`true ? "yes" : "no"` → `"yes"`|

#### **Kid-Friendly Example (One Simple Block)**

```php
$score = 70;

$message = $score >= 60 ? "Passed" : "Try Again";

/*
$score >= 60 = true
So choose "Passed"
*/

echo $message; // Passed 🎉
```

#### **Remember This**

> Ternary is great for **short choices**, but avoid deep nesting.  
> If confusing, replace with **if/else**.

## **Level 15 — Assignment Operators**

Assignment gives a value to a variable.  
Chained assignment copies values across multiple variables.

#### **Quick Reference Table**

|**Operator**|**Purpose / What It Does**|**Example & Result**|
|---|---|---|
|`=`|assign value|`$a = 10`|
|`+= -= *=`|math + assign at the same time|`$a += 5` → `$a = $a + 5`|
|`.=`|join strings & save|`$msg .= "!"`|
|`??=`|assign only if null|`$name ??= "Guest"`|

#### **Kid-Friendly Example (One Simple Block)**

```php
$stars = 3;

$stars += 2; // add 2 more stars
$stars .= "⭐"; // attach a star symbol

echo $stars; // 5⭐
```

#### **Remember This**

> `$a = $b = 5` means **both become 5**.  
> `??=` is great for filling missing values.

## **Level 16 — Low-Precedence AND (`and`)**

Works like `&&` but with **lower priority**, meaning it runs **after assignments and other operations**.  
Can create unexpected bugs — avoid in real code.

#### **Quick Reference Table**

|**Operator**|**Purpose / What It Does**|**Example & Result**|
|---|---|---|
|`and`|low-priority logical AND|Rarely used|

#### **Kid-Friendly Example (One Simple Block)**

```php
$success = false;

$result = $success and true;

/*
Assignment happens first:
$result = false
Then and is applied
*/

echo $result; // false
```

#### **Remember This**

> `and` behaves differently from `&&` — **dangerous for beginners**.  
> Always use `&&` instead.

## **Level 17 — Low-Precedence XOR (`xor`)**

Returns true **only when one side is true**, not both.  
Also low priority like `and`.

#### **Quick Reference Table**

|**Operator**|**Purpose / What It Does**|**Example & Result**|
|---|---|---|
|`xor`|exactly-one must be true|`true xor false` → `true`|

#### **Kid-Friendly Example (One Simple Block)**

```php
$blue = true;
$red  = false;

$winner = $blue xor $red;

/*
exactly one is true => winner = true
*/

echo $winner; // true — only one team scores!
```

#### **Remember This**

> XOR is like **only one can win**.  
> If both true or both false → result false.

## **Level 18 — Low-Precedence OR (`or`)**

Works like `||`, but **very low precedence**, commonly causing bugs.  
Runs **after assignment**, so parentheses are extremely important.

#### **Quick Reference Table**

|**Operator**|**Purpose / What It Does**|**Example & Result**|
|---|---|---|
|`or`|low-priority logical OR|Dangerous in assignment|

#### **Kid-Friendly Example (One Simple Block)**

```php
$loggedIn = false;

$message = $loggedIn or true;

/*
Assignment first:
$message = false
Then OR
*/

echo $message; // false (unexpected!)
```

#### **Remember This**

> `or` behaves differently from `||`.  
> Replace **always** with `||` unless you 100% understand precedence.
