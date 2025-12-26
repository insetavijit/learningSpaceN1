```php

Q1: Why do empty("0"), empty(0), empty(false) return true? | They check falsy values. "0", 0, false are falsy. `isset("0")` returns true because it exists and is not null. Bug: OTP "0" rejected.

Q2: When does isset() return false even when key exists? | When value is null OR key implicitly created but not assigned. Use array_key_exists().

Q3: Why can property_exists() be true after unset()? | unset removes runtime value but property declaration remains.

Q4: Which functions preserve keys and which lose them? | array_map & array_filter preserve keys; array_reduce loses keys.

Q5: Why does strict in_array('1', ['01','1'], true) fail? | Strict mode requires exact match; '1' !== '01'. Breaks ACL logic.

Q6: Why is unserialize() dangerous? | Can rehydrate objects and trigger magic methods → RCE. json_decode() is safe.

Q7: Which json flag throws exceptions? | JSON_THROW_ON_ERROR. Decode objects with json_decode($json, false).

Q8: Which functions halt execution? | require & require_once. include & include_once only warn.

Q9: Is match faster than switch? | Yes. Strict comparison, optimized engine path.

Q10: Why does count(null) throw TypeError? | PHP 8 strict countable rules; old behavior masked bugs.

Q11: array_merge vs + difference? | merge reindexes numeric keys; + preserves keys.

Q12: Why does explode return empty items? | Repeated delimiters. preg_split(..., PREG_SPLIT_NO_EMPTY) removes them.

Q13: How to trim Unicode whitespace? | preg_replace('/\p{Z}+/u', ...). trim() fails for NBSP & ZWSP.

Q14: Purpose of first-class callables? | Cleaner mapping/transform pipelines, replace list()+callbacks.

Q15: Why ob_start()? | Prevents premature output; headers/cookies fail without buffering.

Q16: Why avoid eval()? | RCE vulnerabilities. Replace with call_user_func() or whitelisting.

Q17: ?-> vs ?? difference? | ?-> stops on null, ?? provides fallback.

Q18: Why does unsetting while foreach breaks array? | iterating by value. Change syntax to foreach($arr as $key => $item).

Q19: Why avoid die()/exit()? | Break middleware & testing. Use abort()/throw.

Q20: Functional email-clean pipeline steps? | split → trim/lowercase → filter → unique → validate via filter_var.
```


### 1. Explain why `empty("0")`, `empty(0)`, and `empty(false)` all return `true`, but `isset("0")` returns `true`. Show a real login-validation bug that this causes.

**Answer —**  
`empty()` checks whether a value is considered **falsy**, not whether it exists. The string `"0"`, the integer `0`, and the boolean `false` are all evaluated as **falsy** in PHP’s loose coercion rules — therefore `empty()` returns `true` for all of them.  
However, `isset()` checks **only whether a variable is defined and not `null`**, so `"0"` is considered a valid non-null variable and returns `true` with `isset()`.

In authentication workflows (such as login or OTP validations), treating `"0"` as empty introduces a dangerous bug where users whose credential or code equals `"0"` are wrongly rejected.

**Example Bug — Real Login Validation Failure**

```php
// login process
$otp = $_POST['otp']; // user enters: "0"

if (empty($otp)) {
    die("OTP cannot be empty"); // incorrectly triggered
}

// Correct OTP verification happens here, but never reached
```

**Correct Fix — Use `isset()` or strict check**

```php
if (!isset($otp) || $otp === '') {
    die("OTP cannot be empty");
}
```

or strict type validation:

```php
if ($otp === null || $otp === '') {
    die("OTP cannot be empty");
}
```

**Doubt Table**

|Doubt|Yes|No|
|---|:-:|:-:|
|Does `empty("0")` incorrectly treat `"0"` as empty?|✔️||
|Does `isset("0")` correctly detect the value exists?|✔️||
|Can this cause real login/OTP/account-lock bugs?|✔️||

---
### Q2. When does `isset($array['key'])` return `false` even though the key visually exists? Provide two distinct scenarios.

**Answer —**  
`isset()` returns `false` when a key exists but its value is `null`, because it checks both **existence** and **not-nullness**.  
It also returns `false` for **keys created implicitly or expected but not assigned**, even if visible structurally.

---

**Scenario 1 — Key Exists but Value is `null`**

```php
$data = ['name' => null];

var_dump(isset($data['name']));         // false
var_dump(array_key_exists('name', $data)); // true
```

**Scenario 2 — Key Expected But Never Assigned**

```php
$user = [];
$user['profile']['email'] = 'test@example.com';

var_dump(isset($user['profile']['name'])); // false
```

Or created implicitly:

```php
$arr = [];
$arr['x']['y'] ??= null;

var_dump(isset($arr['x']['y'])); // false
var_dump(array_key_exists('y', $arr['x'])); // true
```

#### **Real Bug Example — API Validation**

```php
// phone number allowed to be null
if (!isset($payload['phone'])) {
    throw new Exception("Phone is required"); // incorrectly triggered
}
```

✔️ Correct fix:

```php
if (!array_key_exists('phone', $payload)) {
    throw new Exception("Phone is required");
}
```

**Doubt Table**

|Doubt|Yes|No|
|---|:-:|:-:|
|Does `isset()` return false if the value is null?|✔️||
|Can a key visually exist yet fail `isset()`?|✔️||
|Should `array_key_exists()` be used when null is acceptable?|✔️||

---
### Q3. After `unset($obj->prop)`, why does `isset($obj->prop)` return `false`, but `property_exists($obj, 'prop')` may still return `true`?

**Answer —**  
`unset()` removes the **value** from an object property at runtime, but it does **not remove the property definition** from the class structure.  
So `isset()` returns `false` because the value no longer exists, while `property_exists()` returns `true` because it checks for **declaration**, not value state.

---

**Example**

```php
class User {
    public $token;
}

$user = new User();
unset($user->token);

var_dump(isset($user->token));             // false
var_dump(property_exists($user, 'token')); // true
```

---

#### **Real Bug Example — Token Revocation Misinterpreted**

```php
if (!isset($user->token)) {
    denyAccess(); // incorrectly assumes token was never issued
}
```

Correct interpretation: token property exists but was cleared (logout or refresh).

---

✔️ **Correct Fix**

```php
if (!property_exists($user, 'token') || $user->token === null) {
    denyAccess();
}
```

---

**Doubt Table**

|Doubt|Yes|No|
|---|:-:|:-:|
|Does `unset()` remove only the value but not the class property?|✔️||
|Does `isset()` return false because the value is gone?|✔️||
|Can `property_exists()` still return true after `unset()`?|✔️||

---
Understood — here is **Q4 with the question properly bolded**, matching the exact improved formatting style.

---

### [Q4]. Write one-line examples proving that `array_map()` preserves keys, `array_filter()` preserves keys by default in PHP 8+, but `array_reduce()` does not.

**Answer —**  
`array_map()` and `array_filter()` retain original array keys, while `array_reduce()` rebuilds a new array without keys, returning only a final aggregated value.

---

**Examples**

```php
print_r(
	array_map(
		fn($v) => $v * 2, [5 => 10, 6 => 20]
	)
);
// [5 => 20, 6 => 40]  (keys preserved)
```

```php
print_r(array_filter([5 => 'x', 6 => ''], fn($v) => $v !== ''));
// [5 => "x"]   (keys preserved in PHP 8+)
```

```php
print_r(array_reduce([5 => 10, 6 => 20], fn($c, $v) => ($c ?? []) + [$v]));
// [10, 20]  (keys lost, reindexed)
```

---

#### **Real Bug Example — Losing Primary Keys in Aggregation**

```php
$totals = array_reduce($items, fn($c, $item) => ($c ?? []) + [$item['id'] => $item['qty']]);
// developer expects preserved IDs but receives numeric indexes
```

---

**Doubt Table**

|Doubt|Yes|No|
|---|:-:|:-:|
|Does `array_map()` preserve keys?|✔️||
|Does `array_filter()` preserve keys by default in PHP 8+?|✔️||
|Does `array_reduce()` destroy keys and rebuild a new array?|✔️||

---

### Q5. Why does `in_array('1', ['01', '1', '2'], true)` return `false` in strict mode, and how can this break role/ACL checks?

**Answer —**  
In strict mode (`===` comparison), `in_array()` requires both **value and type** to match exactly.  
The string `'1'` does **not** strictly equal the string `'01'`, even though they would be considered equal in loose comparison.  
When role IDs, permission flags, or numeric string identifiers depend on strict matching, missing a match can incorrectly deny access or break authorization logic.

---

**Example**

```php
$roles = ['01', '1', '2'];

var_dump(in_array('1', $roles, true));  // false (strict mode, '1' !== '01')
var_dump(in_array('1', $roles));        // true (loose mode, '1' == '01')
```

---

#### **Real Bug — Permission Check Fails**

```php
$userRole = "1"; // editor
$allowed  = ['01', '02']; // from DB padded with zeros

if (!in_array($userRole, $allowed, true)) {
    denyAccess(); // incorrectly denies access
}
```

✔️ **Correct Fix — Normalized values**

```php
$allowed = array_map('intval', $allowed);

if (!in_array((int)$userRole, $allowed, true)) {
    denyAccess();
}
```

---

**Doubt Table**

|Doubt|Yes|No|
|---|:-:|:-:|
|Does strict `in_array()` avoid type coercion problems?|✔️||
|Can padded numeric strings fail strict checks?|✔️||
|Can this cause ACL / role / permission failures?|✔️||

---

### **Q6. Explain why `unserialize()` on untrusted data has caused remote code execution vulnerabilities historically, while `json_decode()` is generally safe.**

**Answer —**  
`unserialize()` can reconstruct **PHP objects** with their magic methods like `__wakeup()` and `__destruct()` automatically executed.  
When attacker-controlled payloads are unserialized, they can instantiate dangerous objects from existing classes and trigger arbitrary code execution or file manipulation.  
`json_decode()` only returns **scalar values, arrays, or stdClass objects**, without invoking class internals or executing code — making it much safer for external input.

---

**Example of Dangerous Serialized Payload**

```php
$payload = 'O:8:"Exploit":0:{}'; // attacker crafts object of a system class
unserialize($payload); // triggers __wakeup() / __destruct() → RCE vulnerability
```

---

**Safe Alternative**

```php
$data = json_decode($json, true); // returns arrays, no executable lifecycle hooks
```

---

#### **Real Hack Example — File Deletion via Magic Method**

```php
class FileCleaner {
    public function __destruct() {
        unlink($this->path); // attacker forces file deletion
    }
}
```

Attacker payload:

```php
O:11:"FileCleaner":1:{s:4:"path";s:12:"config.php";}
```

Placed into session or cookie → executed on unserialize.

---

✔️ **Correct Fixes**

```php
// Never unserialize external input
json_decode($input, true);
```

Or whitelist-only deserialization:

```php
unserialize($input, ['allowed_classes' => ['SafeDTO']]);
```

---

**Doubt Table**

|Doubt|Yes|No|
|---|:-:|:-:|
|Can `unserialize()` instantiate attacker-controlled objects?|✔️||
|Can this trigger magic methods like `__wakeup()` / `__destruct()`?|✔️||
|Is `json_decode()` safer because it returns simple data types?|✔️||

---

### **Q7. Which flags force `json_encode()` to throw a `JsonException` instead of returning `false`, and which option returns objects instead of associative arrays?**

**Answer —**  
`json_encode()` normally returns `false` silently on failure (e.g., malformed UTF-8), which can hide critical data-loss bugs.  
Using the flag `JSON_THROW_ON_ERROR` forces it to throw a `JsonException` instead, making failures explicit and catchable.  
For decoding, passing `false` as the second parameter in `json_decode()` returns results as **objects (`stdClass`)** instead of associative arrays.

---

**Example — Safe Encoding with Exception Handling**

```php
$json = json_encode($data, JSON_THROW_ON_ERROR);
```

**Example — Decode as Objects**

```php
$obj = json_decode($json, false); // object mode
```

**Decode as Associative Array**

```php
$arr = json_decode($json, true); // array mode
```

---

#### **Real Bug Example — Silent Failure in Logging / API Call**

```php
$payload = ["msg" => "Hello \xB1"]; // invalid character

file_put_contents("log.json", json_encode($payload)); 
// returns false silently → corrupted logs / broken API request
```

✔️ **Correct Fix**

```php
try {
    file_put_contents("log.json", json_encode($payload, JSON_THROW_ON_ERROR));
} catch (JsonException $e) {
    error_log($e->getMessage());
}
```

---

**Doubt Table**

|Doubt|Yes|No|
|---|:-:|:-:|
|Does `JSON_THROW_ON_ERROR` force error visibility?|✔️||
|Does `json_encode()` normally fail silently with `false`?|✔️||
|Does `json_decode($json, false)` return objects instead of arrays?|✔️||

---

### **Q8. Which of `include`, `require`, `include_once`, and `require_once` throw catchable `Error` exceptions in PHP 8+, and which halt execution?**

**Answer —**  
In PHP 8+, `require` and `require_once` throw a **fatal `Error`** when the file cannot be loaded, immediately stopping execution.  
`include` and `include_once` only emit a **warning**, allowing execution to continue, but may cause undefined-function or missing-class errors later in the flow.

All four can be handled in `try/catch` since PHP 8 converts fatal errors related to file loading into catchable `Error` exceptions.

---

**Example — Behavior Differences**

```php
include 'missing.php';      // Warning, script continues
include_once 'missing.php'; // Warning, script continues

require 'missing.php';      // Fatal Error → stops execution
require_once 'missing.php'; // Fatal Error → stops execution
```

---

#### **Safe Modern Usage with try/catch**

```php
try {
    require 'config.php';
} catch (Error $e) {
    error_log($e->getMessage());
    exit("Configuration loading failed.");
}
```

---

#### **Real Bug Example — Partial Template Rendering in Production**

```php
include 'header.php'; // file missing → page renders without header
renderPage();         // broken UI, no crash → harder to debug
```

✔️ Correct fix:

```php
require 'header.php'; // fail loud, detect immediately
```

---

**Doubt Table**

|Doubt|Yes|No|
|---|:-:|:-:|
|Do `require` & `require_once` halt execution when file missing?|✔️||
|Do `include` & `include_once` continue execution with warning?|✔️||
|Can all be caught via try/catch in PHP 8+?|✔️||

---

### **Q9. Is `match` faster than `switch` in real-world performance?**

**Answer —**  
Yes — `match` is generally faster or equal in performance because it is optimized for **expression evaluation**, performs **strict comparison (`===`)**, and avoids the branching overhead and loose type coercion that `switch` incurs.  
Benchmarks in real production environments (Laravel Octane, Symfony, Swoole, RoadRunner) consistently show `match` outperforming or matching `switch` in throughput and latency-sensitive routing and event pipelines.

---

**Example — Performance-Oriented Branching**

```php
$priority = match ($event->type) {
    'alert'   => 1,
    'warning' => 2,
    'info'    => 3,
    default   => 4,
};
```

Equivalent `switch`:

```php
switch ($event->type) {
    case 'alert':   $priority = 1; break;
    case 'warning': $priority = 2; break;
    case 'info':    $priority = 3; break;
    default:        $priority = 4;
}
```

---

#### **Real Bug Example — Loose Comparison Causing Wrong Case Hit**

```php
$type = "0"; // event id

switch ($type) {
    case 0:
        $handler = "system-event"; // incorrectly executed
        break;
}
```

✔️ **Correct Fix — Use match**

```php
$handler = match($type) {
    "0" => "low-level",
    default => "standard",
};
```

---

**Doubt Table**

|Doubt|Yes|No|
|---|:-:|:-:|
|Is `match` generally faster or equal to `switch`?|✔️||
|Is `match` optimized for expression-based evaluation?|✔️||
|Does `switch` have any performance advantage in 2025?||✔️|

---

### **Q10. In PHP 8+, why does `count(null)` throw a `TypeError`, and what older behavior does this break?**

**Answer —**  
In PHP 8, strict type rules were introduced to improve reliability and prevent silent logic failures.  
Calling `count(null)` now throws a **`TypeError`**, because `count()` expects a **countable value** (array or object implementing `Countable`).  
Before PHP 8, `count(null)` returned `0`, which often masked bugs caused by missing or uninitialized data structures — leading to unpredictable behavior in loops, pagination, and API responses.

---

**Example — PHP 7 Behavior**

```php
var_dump(count(null)); 
// int(0) → silently treated null as an empty array
```

**Example — PHP 8 Behavior**

```php
var_dump(count(null));
// TypeError: count(): Argument #1 ($value) must be of type Countable|array, null given
```

---

#### **Real Bug Example — Pagination Crash**

```php
$rows = $db->fetchAll(); // returns null on failure

if (count($rows) > 0) {   // PHP 8 → TypeError crash
    renderTable($rows);
}
```

✔️ **Correct Fix**

```php
if (!empty($rows) && is_countable($rows) && count($rows) > 0) {
    renderTable($rows);
}
```

Or simpler:

```php
if (($rows ?? []) !== [] ) renderTable($rows);
```

---

**Doubt Table**

|Doubt|Yes|No|
|---|:-:|:-:|
|Did `count(null)` return `0` in PHP 7?|✔️||
|Does PHP 8 now throw `TypeError` for null values?|✔️||
|Can this break existing pagination / loops / fetch logic?|✔️||

---

### Q11. Demonstrate why `array_merge([1 => 'a'], [0 => 'b'])` behaves differently from `[1 => 'a'] + [0 => 'b']`, showing results.

**Answer —**  
`array_merge()` **reindexes numeric keys**, producing a new sequential array starting from `0`.  
The `+` **array union operator** preserves numeric keys and does not overwrite existing ones — it only adds elements whose keys do not already exist.

---

**Example Comparison**

```php
print_r(array_merge([1 => 'a'], [0 => 'b']));
```

**Result**

```
Array
(
    [0] => a
    [1] => b
)
```

🔹 Keys are **reindexed**, values merged in order.

---

```php
print_r([1 => 'a'] + [0 => 'b']);
```

**Result**

```
Array
(
    [1] => a
    [0] => b
)
```

🔹 Keys are **preserved** and the second array values are appended only when key not already present.

---

#### **Real Bug Example — Losing Database Row IDs**

```php
$ids = array_merge([5 => 'u1'], [10 => 'u2']);
// becomes [0=>'u1',1=>'u2'] → primary key info lost
```

✔️ **Correct Fix — Use `+` when keys represent identity**

```php
$ids = [5 => 'u1'] + [10 => 'u2']; // keys preserved
```

---

**Doubt Table**

|Doubt|Yes|No|
|---|:-:|:-:|
|Does `array_merge()` reindex numeric keys?|✔️||
|Does `+` preserve original numeric keys?|✔️||
|Can wrong choice destroy identity/index relationships?|✔️||

---

### Q12. Why does `explode(',', 'a,,b')` return an empty element, but `preg_split()` can remove it with `PREG_SPLIT_NO_EMPTY`? When is this important?

**Answer —**  
`explode()` splits strictly on the delimiter without applying pattern logic, so consecutive delimiters create **empty string elements** in the result.  
`preg_split()` interprets the separator as a **regular expression**, and with the flag `PREG_SPLIT_NO_EMPTY`, it automatically **removes empty values**, producing cleaner results — critical for data sanitization, CSV parsing, tag processing, and form input normalization.

---

**Example — explode() Producing Empty Element**

```php
print_r(explode(',', 'a,,b'));
```

**Result**

```
Array
(
    [0] => a
    [1] => 
    [2] => b
)
```

---

**Example — preg_split() Removing Empty Elements**

```php
print_r(preg_split('/,/', 'a,,b', -1, PREG_SPLIT_NO_EMPTY));
```

**Result**

```
Array
(
    [0] => a
    [1] => b
)
```

---

#### **Real Bug Example — Email / Tag Parsing**

```php
$emails = explode(',', 'x@example.com,,y@example.com');
// array contains "" → later validation fails unexpectedly
```

✔️ Correct Fix

```php
$emails = preg_split('/[,\s]+/', 'x@example.com,,y@example.com', -1, PREG_SPLIT_NO_EMPTY);
```

---

**Doubt Table**

|Doubt|Yes|No|
|---|:-:|:-:|
|Does `explode()` create empty elements when delimiters repeat?|✔️||
|Can `preg_split()` remove them using `PREG_SPLIT_NO_EMPTY`?|✔️||
|Is this important in CSV/tag/email parsing?|✔️||

---
### **Q13. Which trimming function removes _all_ Unicode whitespace such as zero-width space and non-breaking space?**

**Answer —**  
The standard `trim()`, `ltrim()`, and `rtrim()` only remove a limited ASCII-based set of whitespace characters.  
To remove **all Unicode whitespace**, including `\u{00A0}` (non-breaking space), `\u{200B}` (zero-width space), and other multibyte characters, you must use `preg_replace()` with a Unicode pattern using the `\p{Zs}` class.

---

**Correct Unicode-Safe Trimming**

```php
$text = preg_replace('/^\p{Z}+|\p{Z}+$/u', '', $text);
```

Or remove **all** internal Unicode whitespace too:

```php
$text = preg_replace('/\p{Z}+/u', ' ', $text);
```

---

#### **Example — Normal trim() Fails**

```php
$str = " Hello "; // contains non-breaking space \u{00A0}
var_dump(trim($str)); // " Hello " (unchanged)
```

#### **Unicode Trim Works**

```php
$str = preg_replace('/^\p{Z}+|\p{Z}+$/u', '', $str);
var_dump($str); // "Hello"
```

---

#### **Real Bug Example — Sanitizing Usernames**

```php
$username = "​admin"; // leading zero-width space
if ($username === "admin") denyAccess(); // bypasses comparison
```

✔️ Fix:

```php
$username = preg_replace('/^\p{Z}+|\p{Z}+$/u', '', $username);
```

---

**Doubt Table**

|Doubt|Yes|No|
|---|:-:|:-:|
|Does `trim()` fail on Unicode whitespace?|✔️||
|Does `preg_replace()` with `\p{Z}` handle full Unicode whitespace?|✔️||
|Can this impact login, normalization & security?|✔️||

---

### **Q14. How does PHP 8.1’s first-class callable syntax replace `list()` + `array_map()` for unpacking structured data?**

**Answer —**  
Before PHP 8.1, transforming or extracting object or array fields often required verbose anonymous functions or `list()` destructuring inside callbacks.  
PHP 8.1 introduced **first-class callable syntax** (`Class::method(...)`) allowing methods to be passed directly as callable references, making mapping, transforming, and pipeline-style operations cleaner, faster, and more readable.

---

**Old Approach — Using Anonymous Function + list()**

```php
$users = [
    ['id' => 1, 'name' => 'A'],
    ['id' => 2, 'name' => 'B'],
];

$names = array_map(fn($u) => $u['name'], $users);
```

---

**PHP 8.1 First-Class Callable**

```php
class UserDTO {
    public function __construct(
        public int $id,
        public string $name
    ) {}
}

$objects = array_map(UserDTO::class . '::new', $users);
// directly maps arrays to objects
```

Or extracting method return values via callable reference:

```php
$names = array_map(UserDTO::getName(...), $objects);
```

---

#### **Real Improvement Example — Transforming API Payload**

```php
$clean = array_map(UserDTO::fromArray(...), $payload);
// concise, safe, no manual unpacking or callbacks
```

---

**Doubt Table**

|Doubt|Yes|No|
|---|:-:|:-:|
|Does first-class callable syntax reduce callback boilerplate?|✔️||
|Can it replace `list()` extraction patterns?|✔️||
|Does it improve readability in DTO / transformer pipelines?|✔️||

---

### **Q15. Why do templating engines wrap views with `ob_start()` and `ob_get_clean()`, and what fails without them regarding headers/cookies?**

**Answer —**  
Templating engines use output buffering (`ob_start()`, `ob_get_clean()`) to **capture rendered HTML into a string** instead of sending it directly to the browser.  
Without output buffering, any accidental whitespace or echo output **locks the response stream**, preventing PHP from sending or modifying **headers, cookies, redirects, or HTTP status codes**, since headers must be sent _before_ body output.

Output buffering ensures clean, controlled response generation and supports layered layouts (master template + partials) without risking premature output.

---

**Example — Template Rendering with Output Buffering**

```php
ob_start();
include 'view.php';
$html = ob_get_clean(); // full HTML captured safely
echo $html;
```

---

#### **What Goes Wrong Without Buffering**

```php
include 'view.php'; // view prints content immediately
setcookie('auth', 'xyz'); // Warning: headers already sent
header("Location: /login"); // fails, user not redirected
```

---

#### **Real Bug — Whitespace Before `<?php`**

```php
// <- accidental whitespace here
<?php
header("Location: /admin"); // breaks because output already started
```

✔️ Correct fix — wrap views:

```php
ob_start();
include 'dashboard.php';
render(ob_get_clean());
```

---

**Doubt Table**

|Doubt|Yes|No|
|---|:-:|:-:|
|Do `ob_start()` & `ob_get_clean()` capture output safely?|✔️||
|Does direct output break headers, cookies, and redirects?|✔️||
|Is buffering required for layout nesting and templating?|✔️||

---

### **Q16. Name three real-world hacks caused by `eval()` on input, and show the correct one-line alternative approach.**

**Answer —**  
`eval()` executes a string as raw PHP code, meaning **any injected input is executed with full server privileges**, enabling Remote Code Execution (RCE).  
Historically, insecure usage of `eval()` in CMS plugins, template engines, and API interpreters has allowed attackers to upload code, read config files, run shell commands, or gain full system control.

---

#### **Three Real-World Exploits Caused by `eval()`**

```php
eval($_GET['cmd']);              // run system commands → RCE
eval("return $userInput;");      // parse filters → database dump
eval(base64_decode($payload));   // webshell injection inside CMS
```

---

#### **Correct Safe Alternative — Never Execute Code**

Use **`call_user_func()`**, **anonymous functions**, or **whitelisted operations** instead.

```php
$result = call_user_func($allowedFunctions[$action], $data);
```

Or safe expression evaluation via libraries:

```php
$result = eval_math_expression($input); // safe parser, no code execution
```

Or interpret JSON logic:

```php
$logic = json_decode($input, true); // controlled evaluation
```

---

#### **Real Bug Example — Template Engine Backdoor**

```php
eval("echo $template;"); // attacker embeds `phpinfo();`
```

✔️ Safe fix:

```php
echo str_replace('{{name}}', $name, $template); // string processing only
```

---

**Doubt Table**

|Doubt|Yes|No|
|---|:-:|:-:|
|Can `eval()` lead to remote code execution?|✔️||
|Has `eval()` caused real security breaches in CMS/plugins?|✔️||
|Is replacing it with whitelisted callbacks safer?|✔️||

---

### **Q17. Explain the difference between the null-safe operator `?->` and the null-coalescing operator `??` using nested object chaining.**

**Answer —**  
The **null-safe operator (`?->`)** stops method/property access when any part of the chain is `null` and returns `null` instead of throwing an error.  
The **null-coalescing operator (`??`)** returns a fallback value **only if the entire expression is `null` or undefined**, but it **does not protect method/property access** or prevent fatal errors.

Use `?->` for **safe chaining**, and `??` for **default value fallback**.

---

**Example — Null-safe chaining**

```php
$email = $user?->profile?->contact?->email;
// if any part is null → $email becomes null safely
```

---

**Example — Null-coalescing fallback**

```php
$email = $user->profile->contact->email ?? 'not-provided';
// throws Error if profile or contact is null
```

---

#### **Real Bug Example — API Profile Response**

```php
// throws TypeError when contact missing
return $user->profile->contact->phone ?? "N/A"; 
```

✔️ **Correct Fix**

```php
return $user?->profile?->contact?->phone ?? "N/A";
```

---

**Doubt Table**

|Doubt|Yes|No|
|---|:-:|:-:|
|Does `?->` prevent fatal errors on null property/method chain?|✔️||
|Does `??` provide fallback only after evaluation completes?|✔️||
|Do they solve different problems (safety vs fallback)?|✔️||

---

### **Q18. Why does the following loop corrupt the array, and what single-character change fixes it?**

**Answer —**  
Inside a `foreach`, modifying (`unset`) elements of the same array **while iterating by value** breaks internal array pointers and causes elements to be skipped or incorrectly removed.  
The loop iterates over **a copy of the value**, not the original array indexes — so unsetting by key while iterating by value disrupts traversal order.

Changing `foreach ($items as $item)` to `foreach ($items as **$key** => $item)` ensures iteration uses real keys and allows safe removal.

---

**Original Buggy Loop**

```php
foreach ($items as $item) {
    if ($item['delete']) unset($items[$item['id']]);
}
```

🔴 **Problem** — iterating **by value** while unsetting **by key** → array corruption & skipped entries.

---

**Correct Fix — Add Key in Loop**

```php
foreach ($items as $key => $item) {
    if ($item['delete']) unset($items[$key]);
}
```

Just **one character (`$key`) added** makes removal stable.

---

#### **Real Bug Example — Deleting Cart Items**

```php
foreach ($cart as $item) {
    if ($item['qty'] === 0) unset($cart[$item['id']]);
}
// removes wrong items or skips items that should be removed
```

✔️ **Correct**

```php
foreach ($cart as $id => $item) {
    if ($item['qty'] === 0) unset($cart[$id]);
}
```

---

**Doubt Table**

|Doubt|Yes|No|
|---|:-:|:-:|
|Does modifying an array while iterating by value corrupt traversal?|✔️||
|Is iterating by key the safe correct method?|✔️||
|Is the fix literally a one-character change (`$key`)?|✔️||

---

### **Q19. Why do modern frameworks (Laravel, Symfony) forbid `die()` and `exit()`? Explain the two core problems in pipeline execution and automated testing.**

**Answer —**  
`die()` and `exit()` **terminate execution immediately**, bypassing framework lifecycle flow, middleware stacks, event dispatchers, and cleanup handlers.  
They also **break automated testing, queue runners, and CI pipelines**, since the test cannot capture responses, assertions, or error states when execution stops abruptly.

Modern frameworks require **structured exception handling** and **controlled response generation**, not abrupt termination.

---

#### **Core Problem 1 — Pipeline / Middleware Execution Break**

```php
die("Unauthorized");
```

🔴 Skips:

- request lifecycle
    
- response formatting (JSON/XML/HTML)
    
- logging, rate limiting, auditing
    
- connection cleanup (DB, Redis, queues)
    
- event dispatchers (`terminate`, `finally` blocks)
    

Correct:

```php
abort(401); // Laravel HTTP Exception
```

---

#### **Core Problem 2 — Automated Testing Fails**

```php
exit("Error");
```

🔴 PHPUnit / Pest cannot:

- assert response body
    
- assert status code
    
- intercept exceptions
    
- continue remaining tests
    

Correct:

```php
throw new \Exception("Error message");
```

---

#### **Real Bug Example — API returns raw text instead of structured response**

```php
if (!$user->active) die("banned");
```

Result: API clients break due to inconsistent response format.

✔️ Fix:

```php
abort(403, "User banned");
```

---

**Doubt Table**

|Doubt|Yes|No|
|---|:-:|:-:|
|Do `die()` / `exit()` skip essential pipeline work?|✔️||
|Do they break automated testing and response assertions?|✔️||
|Should structured exceptions replace abrupt termination?|✔️||

---

### **Q20. Write a single PHP 8+ functional-style pipeline that:**

- splits emails by comma/semicolon/newlines
    
- trims and lowercases
    
- removes duplicates and empty elements
    
- validates email format
    
- returns only valid email addresses
    

**Answer —**  
We can build a clean, chainable functional pipeline using `preg_split()`, array functions, and `filter_var()` to safely normalize and validate email lists from user input.

---

**Functional Pipeline — One Compact Expression**

```php
$emails = array_values(array_unique(array_filter(
    array_map('strtolower',
        array_map('trim',
            preg_split('/[,\s;]+/', $input, -1, PREG_SPLIT_NO_EMPTY)
        )
    ),
    fn($email) => filter_var($email, FILTER_VALIDATE_EMAIL)
)));
```

---

#### **Step-by-step breakdown**

|Stage|Operation|
|---|---|
|`preg_split()`|Split by `,`, `;`, space, tabs, newlines|
|`trim()`|Cleans surrounding whitespace|
|`strtolower()`|Standardizes casing|
|`array_filter()`|Keeps only valid emails|
|`filter_var()`|RFC validation|
|`array_unique()`|Removes duplicates|
|`array_values()`|Reindexes cleanly|

---

#### **Real Bug Example — User input from CSV & textarea fields**

```php
$input = "ADMIN@example.com,  user@site.com; ,,, test@test.com
User@Site.com";

echo json_encode($emails);
// ["admin@example.com","user@site.com","test@test.com"]
```

---

**Doubt Table**

|Doubt|Yes|No|
|---|:-:|:-:|
|Does this pipeline normalize and sanitize reliably?|✔️||
|Does it remove duplicates & empty entries?|✔️||
|Does it validate emails according to RFC rules?|✔️||
