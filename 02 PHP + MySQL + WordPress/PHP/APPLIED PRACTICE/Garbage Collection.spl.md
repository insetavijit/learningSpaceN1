13. Core Topics — GC & Memory Model in PHP

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

### 13.1 Memory Model Basics

#### 🎯 **Scope of Learning**

**This section focuses on understanding how PHP manages memory under the hood.**  
You will learn **how variables are stored inside `zval` containers**, how **reference counting** and **Copy-on-Write (CoW)** determine when memory is shared or duplicated, and **why reading is cheap but modifying can be expensive**.  
Mastering these concepts will help you **optimize performance**, **debug memory issues with confidence**, and **prevent leaks in long-running applications** such as **queue workers, daemons, and streaming services**.


##### **1. The Core Building Block: `zval`**

In PHP, a variable name is just a pointer — the **actual value lives inside a container called a `zval`**. Every piece of data in PHP is wrapped in one.

|`zval` Field|What It Means|
|---|---|
|**value**|The actual stored data (int, float, string, pointer to array or object)|
|**type**|Data type indicator like `IS_LONG`, `IS_ARRAY`, `IS_OBJECT`, `IS_REFERENCE`|
|**u.refcount**|Number of variable names pointing to this value|
|**u.is_ref**|Set when using real references (`&$var`), disabling CoW|

Understanding how `refcount` and `is_ref` behave explains almost all surprising memory issues.


##### **2. What Happens During Assignment (Copy-on-Write)**

```php
$a = range(1, 1_000,000);   // Huge array
$b = $a;                    // Shared zval — no copy made
```

|State|Memory|refcount|is_ref|Meaning|
|---|---|---|---|---|
|`$a = …`|~40–50MB|1|0|One huge array created|
|`$b = $a`|~40–50MB|2|0|`$b` only points to `$a`’s data|
|`$b[] = 999`|~80–100MB|1 each|0|Full copy triggered on modification|

This is **Copy-on-Write (CoW)**:

- **Cheap for reads and assignments**
    
- **Expensive for mutations**
 
##### **3. Why This Matters in Real Projects**

|Scenario|Without Knowing CoW|With CoW Knowledge|
|---|---|---|
|Passing large arrays|Fear of performance loss|Confident: no duplication unless modified|
|Queue workers / daemons|Memory blows up unexpectedly|Understand where copy happens & prevent it|
|Debugging leaks|Assume GC is broken|Check refcounts & shared references|
|Framework collection methods|Unexpected mutation effects|Plan immutability & safe transformations|


##### **4. Visual Lifecycle**

```
$a = [massive array];     → zval#123 (refcount=1)
$b = $a;                  → zval#123 (refcount=2)
$b[0] = 'X';              → write triggers split
    ├ $a → zval#123 (refcount=1)
    └ $b → zval#456 (refcount=1)
```


### 13.2 Reference Counting

#### 🎯 Scope of Learning

This section reveals how PHP's engine decides **exactly when** memory should be freed or retained. You'll understand the mechanics behind reference counter increments and decrements, why memory releases **instantly** when `refcount` hits zero, and how seemingly innocent code patterns create hidden leaks in complex data structures and long-running processes.

Mastering reference counting gives you X-ray vision into PHP's memory lifecycle—essential for writing leak-free code, debugging production memory issues, and understanding why certain patterns cause slow memory bloat over time.


#### 1. What Reference Counting Really Means

Every `zval` container carries an invisible counter tracking **how many variables currently point to it**. This counter is called `refcount`, and it's the primary mechanism PHP uses to determine if memory can be safely freed.

**Example:**

```php
$a = 42;   // zval created with refcount = 1
$b = $a;   // Same zval, refcount = 2 (shared efficiently)
unset($a); // refcount = 1 (value still alive via $b)
unset($b); // refcount = 0 → memory IMMEDIATELY freed
```

|Operation|`refcount` Change|Result|
|---|---|---|
|Variable assigned|`refcount++`|Same value shared in memory|
|Variable `unset()`|`refcount--`|Memory freed when zero|
|Scope exit|Decrements all locals|Triggers automatic cleanup|
|Copy-on-write trigger|Split & reset|Separate memory allocations|

**🧠 Key Insight:** Sharing values (`$b = $a`) is **not copying**—both variables point to the same zval until one tries to modify it (copy-on-write).


#### 2. When Memory Is Actually Destroyed

Unlike languages with deferred garbage collection (Java, Python), PHP frees memory **the instant** `refcount` reaches `0`. There's no waiting, no GC pause—it happens immediately.

**Example:**

```php
$a = "hello world";
$b = $a;        // refcount = 2 (shared string)
unset($a);      // refcount = 1 (NOT freed yet—$b still holds it)
unset($b);      // refcount = 0 → INSTANT deallocation
```

**🧠 Key Concept:** This is why simple PHP scripts are so memory-efficient—most values are freed the moment you leave scope or unset them. Garbage collection **doesn't run at all** for acyclic structures.

**Function Scope Example:**

```php
function process() {
    $data = range(1, 100000); // Large array
    // ... use $data ...
}  // ← Memory freed HERE instantly when function exits

process();
// Array is already gone—no GC needed
```


#### 3. Where Reference Counting Fails — Circular References

Reference counting has **one fatal weakness**: circular references. When two or more structures reference each other, their refcounts never reach zero—even when no external variables point to them.

**Classic Leak Example:**

```php
class Node {
    public $next;
}

$a = new Node();
$b = new Node();
$a->next = $b;
$b->next = $a;  // Circular link created

unset($a, $b);  // ⚠️ LEAK! Objects still in memory
                // $a->next keeps $b alive (refcount ≥ 1)
                // $b->next keeps $a alive (refcount ≥ 1)
```

|Condition|Outcome|
|---|---|
|Refcount never reaches 0|Value becomes **unreachable but retained**|
|Creates cycle|Needs **GC cycle collector** to detect and break|
|Common in|Linked lists, tree structures, parent-child relationships, event listeners|

**Real-World Danger Zones:**

```php
// ❌ Parent-child bidirectional link
$parent->child = $child;
$child->parent = $parent;  // Leak if not broken manually

// ❌ Event listener holding object reference
$object->addEventListener($object);  // Self-reference leak

// ❌ Closure capturing $this
class Worker {
    public function start() {
        $this->callback = fn() => $this->process();  // Holds $this
    }
}
```

**This is the root cause of most real-world memory leaks in PHP.**


#### 4. Real Project Impact

|Scenario|Without Understanding Refcount|With Refcount Knowledge|
|---|---|---|
|Objects referencing each other|Persistent leaks & rising memory|Design acyclic relationships or use `WeakReference`|
|Queue workers / daemons|Memory ballooning over hours|Actively `unset()` large structures or break cycles|
|Debugging memory issues|Assume GC is failing|Inspect live refcounts & eliminate cycles with `xdebug_debug_zval()`|
|Using references (`&`)|Accidental leak creation|Use sparingly or avoid completely|
|Long-running CLI scripts|Out-of-memory crashes|Understand when memory actually releases|


#### 5. Senior-Level Takeaways

✅ Memory frees **immediately** when `refcount → 0` (no GC delay)  
✅ Shared values are **normal and safe** (copy-on-write handles modifications)  
✅ PHP references (`&`) should be **avoided unless absolutely needed**  
✅ **Circular references trap memory**—GC cycle collector is required to clean them  
✅ Essential knowledge for debugging leaks in CLI workers, message queues, async pipelines, and WebSocket servers  
✅ Use `WeakReference` (PHP 7.4+) or manual `unset()` to break cycles in production code


### 13.03. Circular References & Cycle Collection

#### 🎯 Scope of Learning

This section reveals **why PHP's garbage collector exists at all**. You'll understand the exact scenarios where reference counting fails catastrophically, how PHP's cycle collector detects and breaks unreachable circular structures, and why this knowledge is critical for preventing memory leaks in production systems—especially in long-running processes, event-driven architectures, and complex domain models.

Mastering cycle collection transforms you from "hoping memory gets freed" to **engineering systems that provably don't leak**, even under complex object relationships.


#### 1. Why Circular References Are the Enemy

Reference counting works perfectly—until values reference each other. When structures form a cycle, their refcounts never reach zero, even when no external code can access them anymore.

**The Core Problem:**

```php
class Node {
    public $data;
    public $next;
}

$a = new Node();
$b = new Node();
$a->next = $b;  // $a holds reference to $b
$b->next = $a;  // $b holds reference to $a (cycle!)

unset($a, $b);  // ⚠️ BOTH OBJECTS LEAK
                // $a refcount = 1 (held by $b->next)
                // $b refcount = 1 (held by $a->next)
                // No external variables exist, but memory remains allocated
```

|Situation|Reference Count|Accessibility|Memory State|
|---|---|---|---|
|Before `unset()`|`$a = 2, $b = 2`|Reachable via `$a` and `$b`|In use|
|After `unset()`|`$a = 1, $b = 1`|**Unreachable from code**|**LEAKED**|
|Without GC|Stays `1, 1` forever|Never accessible again|Permanent leak|
|With GC|Detected as cycle|Collector breaks links|**Freed**|

**🧠 Key Insight:** These objects are **garbage** (unreachable) but refcount can't detect it because they hold each other alive.


#### 2. Real-World Circular Reference Patterns

##### Pattern 1: Parent-Child Bidirectional Links

```php
class Parent {
    public $children = [];
}

class Child {
    public $parent;
}

$parent = new Parent();
$child = new Child();
$parent->children[] = $child;
$child->parent = $parent;  // ⚠️ Cycle created

unset($parent, $child);  // LEAK without GC
```

##### Pattern 2: Event Listeners Holding Object References

```php
class Button {
    private $listeners = [];
    
    public function onClick($callback) {
        $this->listeners[] = $callback;
    }
}

$button = new Button();
$button->onClick(function() use ($button) {  // ⚠️ Closure captures $button
    $button->doSomething();
});

unset($button);  // LEAK: closure in $listeners holds $button
```

##### Pattern 3: Doubly Linked Lists

```php
class DNode {
    public $prev;
    public $next;
}

$head = new DNode();
$tail = new DNode();
$head->next = $tail;
$tail->prev = $head;  // ⚠️ Cycle

unset($head, $tail);  // LEAK without GC
```

##### Pattern 4: ORM/Entity Relationships

```php
class User {
    public $posts = [];
}

class Post {
    public $author;
}

$user = new User();
$post = new Post();
$user->posts[] = $post;
$post->author = $user;  // ⚠️ Common in Doctrine/Eloquent patterns

unset($user, $post);  // LEAK if not handled
```


#### 3. How PHP's Cycle Collector Works

##### Detection Algorithm (Simplified)

PHP uses a **mark-and-sweep** algorithm specifically for detecting cycles:

**Step 1: Root Buffer Collection**

- PHP maintains a buffer of "possible roots" (objects/arrays with refcount changes)
- When buffer fills (default: 10,000 items) → trigger GC

**Step 2: Mark Phase**

```
For each potential root:
  - Simulate removing all internal references
  - If refcount would become 0 → not a cycle, skip
  - If refcount stays > 0 → potential cycle, mark for sweep
```

**Step 3: Sweep Phase**

```
For each marked structure:
  - Verify it's truly unreachable from any root variable
  - If confirmed → break circular links
  - Decrement refcounts → triggers normal destruction
```

**Example Walk-Through:**

```php
class Node {
    public $ref;
}

$a = new Node();
$b = new Node();
$a->ref = $b;  // $b refcount = 2
$b->ref = $a;  // $a refcount = 2

unset($a);  // $a refcount = 1 (held by $b->ref) → added to root buffer
unset($b);  // $b refcount = 1 (held by $a->ref) → added to root buffer

// GC triggers:
// 1. Simulates removing $a->ref: $b would become 0 ✓
// 2. Simulates removing $b->ref: $a would become 0 ✓
// 3. Confirms both unreachable from symbol table
// 4. Breaks links: $a->ref = null, $b->ref = null
// 5. Refcounts drop to 0 → memory freed
```


#### 4. When GC Actually Runs

|Trigger|Condition|Typical Scenario|
|---|---|---|
|**Root buffer full**|10,000 potential cycles collected|Long-running scripts with many object destructions|
|**Manual call**|`gc_collect_cycles()`|After processing large batch of data|
|**Memory pressure**|PHP approaching memory_limit|Emergency cleanup before fatal error|
|**Request end**|Automatic full cleanup|Web requests (usually unnecessary)|

**🧠 Key Concept:** GC **does not run constantly**. Simple scripts may never trigger it because reference counting handles everything.


#### 5. Performance Impact

##### Cost of Cycle Collection

```php
// Benchmarking GC impact
$start = memory_get_usage();

for ($i = 0; $i < 100000; $i++) {
    $a = new stdClass();
    $b = new stdClass();
    $a->ref = $b;
    $b->ref = $a;
    unset($a, $b);  // Creates 100k cycles
}

gc_collect_cycles();  // Costs ~10-50ms for 100k cycles

$end = memory_get_usage();
echo "Memory freed: " . ($start - $end) . " bytes\n";
```

**Performance Characteristics:**

- **Best case:** O(n) where n = number of roots in buffer
- **Worst case:** O(n²) for deeply nested cycles (rare)
- **Typical overhead:** 1-5% for normal applications
- **When it matters:** CLI daemons, queue workers, WebSocket servers


#### 6. Real Project Impact

|Scenario|Without Understanding Cycles|With Cycle Knowledge|
|---|---|---|
|**Doctrine/Eloquent entities**|Memory leaks in batch processing|Use `clear()` to break relationships|
|**Event-driven systems**|Listeners never released|Use `WeakReference` or manual detachment|
|**Graph algorithms**|Out-of-memory in tree traversal|Break parent links or use `WeakMap`|
|**Long-running workers**|Memory grows indefinitely|Strategic `gc_collect_cycles()` calls|
|**Caching layers**|Cache entries hold circular refs|Design acyclic cache structures|


#### 7. Prevention Strategies

##### Strategy 1: Break Cycles Manually

```php
class Node {
    public $next;
    
    public function __destruct() {
        $this->next = null;  // Break link proactively
    }
}
```

##### Strategy 2: Use WeakReference (PHP 7.4+)

```php
class Child {
    private WeakReference $parent;
    
    public function setParent(Parent $p) {
        $this->parent = WeakReference::create($p);
    }
    
    public function getParent(): ?Parent {
        return $this->parent->get();  // Returns null if parent destroyed
    }
}
```

##### Strategy 3: WeakMap for Caches (PHP 8.0+)

```php
$cache = new WeakMap();  // Keys auto-removed when objects destroyed
$cache[$object] = $expensiveComputation;
```

##### Strategy 4: Explicit Cleanup in Long-Running Processes

```php
while (true) {
    $batch = fetchWorkItems();
    
    foreach ($batch as $item) {
        processItem($item);
    }
    
    unset($batch);  // Release references
    gc_collect_cycles();  // Force cycle cleanup
}
```


#### 8. Debugging Circular References

##### Tool 1: xdebug_debug_zval()

```php
$a = new stdClass();
$b = new stdClass();
$a->ref = $b;
$b->ref = $a;

xdebug_debug_zval('a');
// Shows refcount and structure—look for refcount > 1 after unset
```

##### Tool 2: memory_get_usage() Monitoring

```php
$before = memory_get_usage();
// ... code that might leak ...
$after = memory_get_usage();

if ($after - $before > 1000000) {  // 1MB leak
    error_log("Potential memory leak detected");
}
```

##### Tool 3: gc_status() for GC Activity

```php
$stats = gc_status();
echo "Runs: {$stats['runs']}\n";
echo "Collected: {$stats['collected']}\n";
echo "Roots: {$stats['roots']}\n";
```


#### 9. Senior-Level Takeaways

✅ **Circular references are the ONLY reason GC exists**—reference counting handles everything else  
✅ GC runs **infrequently** (every 10k root buffer fills)—don't rely on it for timely cleanup  
✅ **Parent-child, event listeners, closures** are the most common leak sources  
✅ `WeakReference` and `WeakMap` solve 90% of cycle problems in modern PHP  
✅ Long-running processes **must** manually manage cycles or call `gc_collect_cycles()`  
✅ ORMs (Doctrine) require explicit `clear()` or `detach()` in batch operations  
✅ **Never disable GC** (`gc_disable()`) unless you've proven cycles don't exist in your code

### 13.04. GC Triggers

#### 🎯 Scope of Learning

This section reveals **exactly when PHP decides to run garbage collection**, dispelling myths about "automatic memory management" and giving you precise control over when cleanup happens. You'll understand the conditions that trigger cycle collection, how to measure GC impact on performance, when to manually invoke cleanup, and when disabling GC is actually beneficial.

Mastering GC triggers transforms you from passive observer to **active performance engineer**—critical for optimizing high-throughput APIs, long-running workers, and memory-sensitive applications.


#### 1. The Four GC Trigger Conditions

PHP's garbage collector doesn't run continuously or randomly. It triggers under **specific, measurable conditions**:

| Trigger Type          | Condition                        | Frequency                  | Controllable                |
| --------------------- | -------------------------------- | -------------------------- | --------------------------- |
| **Root Buffer Full**  | 10,000 potential roots collected | Most common in normal code | ✅ Yes (configure threshold) |
| **Manual Invocation** | `gc_collect_cycles()` called     | Developer-controlled       | ✅ Yes (explicit call)       |
| **Memory Pressure**   | Approaching `memory_limit`       | Rare (emergency only)      | ❌ No (automatic safety)     |
| **Request Shutdown**  | Script termination               | Once per web request       | ❌ No (automatic cleanup)    |

**🧠 Key Insight:** For most web requests (< 1 second), GC **never runs** because reference counting handles everything. GC is primarily for long-running processes.


#### 2. Root Buffer Trigger (Primary Mechanism)

##### What is the Root Buffer?

PHP maintains an internal buffer tracking "possible cycle roots"—objects or arrays whose refcount changed in a way that might indicate a cycle.

**How It Works:**

```php
// Each operation below adds to root buffer
$a = new stdClass();
$b = new stdClass();
$a->ref = $b;
$b->ref = $a;  // Cycle created → both added to buffer

unset($a);     // refcount decreases → added to buffer
unset($b);     // refcount decreases → added to buffer

// When buffer reaches 10,000 entries → GC runs automatically
```

##### Threshold Configuration

```php
// Check current threshold
$info = gc_status();
echo "Threshold: " . $info['threshold'] . "\n";  // Default: 10000
echo "Roots: " . $info['roots'] . "\n";          // Current buffer size

// Increase threshold for better performance (fewer GC runs)
gc_mem_caches(50000);  // PHP 8.3+ only

// Decrease for more aggressive cleanup (more GC runs)
gc_mem_caches(5000);   // Trade CPU for lower memory
```

**Performance Trade-off:**

|Threshold|GC Frequency|Memory Usage|CPU Usage|Use Case|
|---|---|---|---|---|
|**5,000**|High (more runs)|Lower peak|Higher overhead|Memory-constrained environments|
|**10,000**|Default balance|Moderate|Moderate|General purpose|
|**50,000**|Low (fewer runs)|Higher peak|Lower overhead|High-throughput APIs|
|**Disabled**|Never|Highest risk|Lowest overhead|Proven no cycles exist|


#### 3. Manual GC Invocation

##### When to Call `gc_collect_cycles()`

**✅ Good Use Cases:**

```php
// ✅ After processing large batches
foreach (array_chunk($items, 1000) as $batch) {
    processBatch($batch);
    unset($batch);
    gc_collect_cycles();  // Clean up between batches
}

// ✅ In long-running queue workers
while ($job = $queue->pop()) {
    $job->process();
    unset($job);
    
    if (++$count % 100 === 0) {
        gc_collect_cycles();  // Periodic cleanup
    }
}

// ✅ After ORM bulk operations (Doctrine example)
$em->flush();
$em->clear();           // Break entity relationships
gc_collect_cycles();    // Force cycle cleanup

// ✅ Before critical memory operations
gc_collect_cycles();    // Ensure clean slate
$hugeDataset = loadMassiveFile();
```

**❌ Bad Use Cases:**

```php
// ❌ Inside tight loops (huge performance hit)
foreach ($items as $item) {
    process($item);
    gc_collect_cycles();  // Called millions of times!
}

// ❌ In web requests (usually unnecessary)
public function handleRequest() {
    $result = $this->service->getData();
    gc_collect_cycles();  // Waste of CPU—request ends anyway
    return $result;
}

// ❌ "Just in case" without measurement
gc_collect_cycles();  // Hope-driven development
```

##### Measuring GC Impact

```php
// Benchmark GC collection time
$start = hrtime(true);
$collected = gc_collect_cycles();
$duration = (hrtime(true) - $start) / 1e6;  // Convert to milliseconds

echo "Collected: {$collected} cycles\n";
echo "Duration: {$duration}ms\n";

// Example output:
// Collected: 1523 cycles
// Duration: 2.3ms
```

**Real-World Performance Data:**

|Cycles Collected|Typical Duration|Impact|
|---|---|---|
|0-100|< 0.1ms|Negligible|
|100-1,000|0.5-2ms|Acceptable for batch jobs|
|1,000-10,000|2-10ms|Noticeable in real-time systems|
|10,000+|10-100ms|Significant—optimize code instead|


#### 4. Memory Pressure Trigger (Emergency Mode)

When PHP nears `memory_limit`, it triggers an **emergency GC run** as a last-ditch effort to avoid fatal errors.

```php
ini_set('memory_limit', '128M');

$data = [];
while (true) {
    $data[] = range(1, 10000);  // Consume memory
    
    // PHP will trigger emergency GC automatically around ~120M
    // If GC can't free enough → Fatal error: Allowed memory size exhausted
}
```

**Characteristics:**

- ⚠️ **Unpredictable timing** (depends on actual memory usage)
- ⚠️ **Performance spike** (runs during critical allocation)
- ⚠️ **Often too late** (may not prevent OOM if no cycles exist)

**🧠 Key Insight:** If you're hitting memory pressure triggers, your code likely has a **design problem**, not a GC tuning problem.


#### 5. Request Shutdown Trigger

At the end of every script execution, PHP performs a **full cleanup**:

```php
// Web request example
echo "Response sent";

// After output, PHP automatically:
// 1. Decrements refcounts for all variables
// 2. Runs destructors
// 3. Collects cycles (if any remain)
// 4. Frees all memory back to OS
```

**Why This Matters:**

|Script Type|Shutdown Impact|Optimization Strategy|
|---|---|---|
|**Web requests**|Irrelevant (process ends)|Don't bother with manual GC|
|**CLI scripts**|Only at final exit|Clean up in loops, not at end|
|**Daemons/workers**|Never (infinite loop)|**Must** manage memory manually|


#### 6. GC Statistics & Monitoring

##### Real-Time Monitoring

```php
function logGCStats(string $label) {
    $stats = gc_status();
    error_log(sprintf(
        "[%s] GC Stats - Runs: %d, Collected: %d, Roots: %d/%d (%.1f%% full)",
        $label,
        $stats['runs'],
        $stats['collected'],
        $stats['roots'],
        $stats['threshold'],
        ($stats['roots'] / $stats['threshold']) * 100
    ));
}

// Usage in long-running process
logGCStats('startup');

while ($job = $queue->pop()) {
    $job->process();
    
    if (++$count % 1000 === 0) {
        logGCStats("after_1000_jobs");
    }
}
```

**Example Output:**

```
[startup] GC Stats - Runs: 0, Collected: 0, Roots: 0/10000 (0.0% full)
[after_1000_jobs] GC Stats - Runs: 3, Collected: 1247, Roots: 3421/10000 (34.2% full)
[after_2000_jobs] GC Stats - Runs: 7, Collected: 2893, Roots: 6789/10000 (67.9% full)
```

##### Memory Leak Detection Pattern

```php
class MemoryLeakDetector {
    private int $baseline;
    private int $threshold;
    
    public function __construct(int $thresholdMB = 10) {
        $this->baseline = memory_get_usage(true);
        $this->threshold = $thresholdMB * 1024 * 1024;
    }
    
    public function check(string $context): void {
        gc_collect_cycles();  // Force cleanup first
        
        $current = memory_get_usage(true);
        $growth = $current - $this->baseline;
        
        if ($growth > $this->threshold) {
            error_log(sprintf(
                "⚠️ Memory leak detected in %s: +%.2fMB (baseline: %.2fMB, current: %.2fMB)",
                $context,
                $growth / 1024 / 1024,
                $this->baseline / 1024 / 1024,
                $current / 1024 / 1024
            ));
        }
    }
}

// Usage
$detector = new MemoryLeakDetector(threshold: 5);

while ($job = $queue->pop()) {
    $job->process();
    
    if (++$count % 100 === 0) {
        $detector->check("after_{$count}_jobs");
    }
}
```


#### 7. Disabling GC (When and Why)

##### When to Disable GC

```php
gc_disable();  // Stop automatic collection

// ✅ Good reasons to disable:
// 1. You've proven no circular references exist
// 2. Short-lived script with simple data structures
// 3. High-throughput API where 1-2% overhead matters
// 4. Benchmarking to isolate GC impact

// ❌ Bad reasons to disable:
// "My script is slow" (probably not GC's fault)
// "I don't understand GC" (learn it instead)
// "Someone on Stack Overflow said to" (cargo cult)
```

##### Performance Comparison

```php
// Benchmark with GC enabled
gc_enable();
$start = microtime(true);

for ($i = 0; $i < 100000; $i++) {
    $obj = new stdClass();
    $obj->data = range(1, 100);
}

$withGC = microtime(true) - $start;

// Benchmark with GC disabled
gc_disable();
$start = microtime(true);

for ($i = 0; $i < 100000; $i++) {
    $obj = new stdClass();
    $obj->data = range(1, 100);
}

$withoutGC = microtime(true) - $start;

printf("With GC: %.3fs | Without GC: %.3fs | Overhead: %.1f%%\n",
    $withGC,
    $withoutGC,
    (($withGC - $withoutGC) / $withoutGC) * 100
);

// Typical output:
// With GC: 0.523s | Without GC: 0.518s | Overhead: 1.0%
```

**🧠 Key Insight:** GC overhead is usually **1-3%** for normal code. If your app is slow, GC is probably not the culprit.


#### 8. Real Project Scenarios

##### Scenario 1: High-Throughput API

```php
// Problem: 50,000 req/sec, every millisecond counts
// Solution: Increase GC threshold
gc_mem_caches(50000);  // Reduce GC frequency by 5x

// Monitor memory growth over time
// If stable → good trade-off
// If growing → cycles exist, investigate code
```

##### Scenario 2: Queue Worker Processing Large Batches

```php
// Problem: Memory grows from 50MB → 500MB over 1 hour
// Solution: Manual GC after each batch

while ($batch = $queue->getBatch(100)) {
    foreach ($batch as $job) {
        $this->process($job);
    }
    
    unset($batch);
    gc_collect_cycles();  // Force cleanup every 100 jobs
    
    // Log memory after cleanup
    if (memory_get_usage(true) > 100 * 1024 * 1024) {
        error_log("⚠️ Memory > 100MB after GC—potential leak");
    }
}
```

##### Scenario 3: Doctrine ORM Batch Insert

```php
// Problem: EntityManager holds circular references
// Solution: Clear + manual GC

$batchSize = 500;
foreach ($records as $i => $record) {
    $entity = new Entity($record);
    $em->persist($entity);
    
    if (($i % $batchSize) === 0) {
        $em->flush();
        $em->clear();           // Break entity relationships
        gc_collect_cycles();    // Clean up cycles immediately
    }
}
```

##### Scenario 4: WebSocket Server (ReactPHP/Swoole)

```php
// Problem: Infinite event loop, memory must stay bounded
// Solution: Periodic GC + monitoring

$loop->addPeriodicTimer(60, function() {
    $before = memory_get_usage(true);
    $collected = gc_collect_cycles();
    $after = memory_get_usage(true);
    
    error_log(sprintf(
        "GC Run: collected=%d, freed=%.2fMB, current=%.2fMB",
        $collected,
        ($before - $after) / 1024 / 1024,
        $after / 1024 / 1024
    ));
});
```


#### 9. Debugging GC Performance Issues

##### Step 1: Identify If GC Is the Problem

```php
$stats = gc_status();
echo "Total GC runs: {$stats['runs']}\n";
echo "Total collected: {$stats['collected']}\n";

// If runs > 100 in a short script → investigate
// If collected > 10,000 → major cycle problem
```

##### Step 2: Profile GC Timing

```php
register_tick_function(function() {
    static $lastRuns = 0;
    $stats = gc_status();
    
    if ($stats['runs'] > $lastRuns) {
        error_log("⚡ GC triggered at " . debug_backtrace()[1]['function']);
        $lastRuns = $stats['runs'];
    }
});

declare(ticks=1);  // Enable tick profiling (expensive!)
```

##### Step 3: Measure Impact

```php
// Disable GC and compare performance
gc_disable();
$withoutGC = benchmarkCode();

gc_enable();
$withGC = benchmarkCode();

if ($withGC - $withoutGC > 0.1) {  // > 100ms difference
    echo "⚠️ GC causing significant overhead\n";
    // Investigate circular references in code
}
```


#### 10. Senior-Level Takeaways

✅ **GC triggers at 10,000 root buffer fills**—not continuously, not randomly  
✅ **Web requests rarely trigger GC** (too short-lived)—optimize for long-running processes  
✅ **Manual `gc_collect_cycles()` is strategic**, not routine—use after batches, not in loops  
✅ **Increase threshold (50k) for high-throughput**, decrease (5k) for memory-constrained  
✅ **Monitor `gc_status()` in production**—track runs, collected cycles, and buffer fullness  
✅ **GC overhead is typically 1-3%**—if your app is slow, look elsewhere first  
✅ **Disable GC only after proving no cycles exist**—measure, don't guess  
✅ **Emergency memory pressure triggers mean your code has a leak**—fix the design


### 13.05. Manual GC Control 

#### 🎯 Scope of Learning

This section reveals **how to take direct control of PHP's garbage collector** instead of relying on automatic triggers. You'll learn the precise functions for managing GC, when manual control is beneficial versus harmful, patterns for integrating GC into worker processes and batch jobs, and how to measure whether your interventions actually improve performance or memory usage.

Mastering manual GC control separates junior developers who "hope memory gets freed" from senior engineers who **architect predictable, bounded memory behavior** in production systems.


#### 1. The Manual GC Control Functions

PHP provides four core functions for controlling garbage collection:

|Function|Purpose|When to Use|Return Value|
|---|---|---|---|
|`gc_collect_cycles()`|Force immediate cycle collection|After large batches, periodic cleanup|Number of cycles collected|
|`gc_enable()`|Enable automatic GC|Re-enable after disabling|`void`|
|`gc_disable()`|Disable automatic GC|High-performance scenarios, proven no cycles|`void`|
|`gc_status()`|Get GC runtime statistics|Monitoring, debugging, profiling|Array of stats|

##### Function Details

```php
// 1. Force collection and see results
$collected = gc_collect_cycles();
echo "Freed {$collected} circular reference cycles\n";

// 2. Disable automatic triggers (use with caution)
gc_disable();
// ... code that definitely has no cycles ...
gc_enable();

// 3. Check if GC is enabled
if (gc_enabled()) {
    echo "GC is active\n";
}

// 4. Comprehensive statistics (PHP 7.3+)
$stats = gc_status();
print_r($stats);
/*
Array (
    [runs] => 12              // Total GC executions since script start
    [collected] => 3456       // Total cycles freed
    [threshold] => 10000      // Root buffer size trigger
    [roots] => 4521           // Current items in root buffer
    [running] => false        // Whether GC is currently executing
    [protected] => false      // Whether GC is protected from recursion
    [full] => false           // Whether a full collection was performed
    [buffer_size] => 262144   // Internal buffer size (PHP 8.3+)
)
*/
```


#### 2. Strategic Manual Collection Patterns

##### Pattern 1: Batch Processing with Periodic GC

**Problem:** Processing 1 million records causes memory to grow from 50MB → 2GB over 30 minutes.

**Solution:**

```php
class BatchProcessor {
    private int $batchSize;
    private int $gcInterval;
    private int $processed = 0;
    
    public function __construct(int $batchSize = 1000, int $gcInterval = 10) {
        $this->batchSize = $batchSize;
        $this->gcInterval = $gcInterval;
    }
    
    public function process(array $items): void {
        foreach (array_chunk($items, $this->batchSize) as $batch) {
            $this->processBatch($batch);
            
            $this->processed++;
            
            // GC every N batches (e.g., every 10,000 items)
            if ($this->processed % $this->gcInterval === 0) {
                $this->performGC();
            }
        }
    }
    
    private function performGC(): void {
        $before = memory_get_usage(true);
        $collected = gc_collect_cycles();
        $after = memory_get_usage(true);
        $freed = $before - $after;
        
        error_log(sprintf(
            "GC after %d batches: collected=%d cycles, freed=%.2fMB, current=%.2fMB",
            $this->processed,
            $collected,
            $freed / 1024 / 1024,
            $after / 1024 / 1024
        ));
    }
    
    private function processBatch(array $batch): void {
        foreach ($batch as $item) {
            // Process item...
        }
        unset($batch); // Important: release batch reference
    }
}

// Usage
$processor = new BatchProcessor(batchSize: 1000, gcInterval: 10);
$processor->process($millionRecords);
```

**Key Principles:**

- ✅ GC **between batches**, not inside item loops
- ✅ Use `unset()` to release large structures before GC
- ✅ Log memory metrics to verify effectiveness
- ✅ Tune `gcInterval` based on memory growth rate


##### Pattern 2: Queue Worker with Memory Bounds

**Problem:** Long-running queue worker must stay under 256MB indefinitely.

**Solution:**

```php
class QueueWorker {
    private const MEMORY_LIMIT_MB = 200; // Safety margin before 256MB limit
    private const GC_INTERVAL_JOBS = 50;
    private const FORCE_RESTART_THRESHOLD_MB = 220;
    
    private int $jobsProcessed = 0;
    
    public function run(QueueInterface $queue): void {
        while ($job = $queue->pop()) {
            try {
                $this->processJob($job);
            } finally {
                unset($job); // Always release job reference
                $this->jobsProcessed++;
                
                // Strategy 1: Periodic GC
                if ($this->jobsProcessed % self::GC_INTERVAL_JOBS === 0) {
                    $this->performMaintenanceGC();
                }
                
                // Strategy 2: Memory threshold GC
                if ($this->getMemoryUsageMB() > self::MEMORY_LIMIT_MB) {
                    $this->performEmergencyGC();
                }
                
                // Strategy 3: Graceful restart if leak suspected
                if ($this->getMemoryUsageMB() > self::FORCE_RESTART_THRESHOLD_MB) {
                    $this->gracefulRestart();
                }
            }
        }
    }
    
    private function performMaintenanceGC(): void {
        $collected = gc_collect_cycles();
        
        if ($collected > 0) {
            error_log("Maintenance GC: collected {$collected} cycles");
        }
    }
    
    private function performEmergencyGC(): void {
        $before = $this->getMemoryUsageMB();
        $collected = gc_collect_cycles();
        $after = $this->getMemoryUsageMB();
        
        error_log(sprintf(
            "⚠️ Emergency GC triggered: %.2fMB → %.2fMB (freed %.2fMB, %d cycles)",
            $before,
            $after,
            $before - $after,
            $collected
        ));
        
        // If GC didn't help much, log warning
        if ($before - $after < 10 && $after > self::MEMORY_LIMIT_MB) {
            error_log("⚠️ GC ineffective—possible non-circular memory leak");
        }
    }
    
    private function gracefulRestart(): void {
        error_log("🔄 Memory limit exceeded—initiating graceful restart");
        exit(0); // Process manager (Supervisor) will restart
    }
    
    private function getMemoryUsageMB(): float {
        return memory_get_usage(true) / 1024 / 1024;
    }
    
    private function processJob($job): void {
        // Job processing logic...
    }
}

// Supervisor config (/etc/supervisor/conf.d/queue-worker.conf)
// [program:queue-worker]
// command=php /app/worker.php
// autorestart=true        ← Restart on exit(0)
// startretries=999999     ← Infinite restarts
```

**Multi-Layer Defense:**

1. **Periodic GC**: Regular cleanup every N jobs
2. **Threshold GC**: Emergency cleanup when memory high
3. **Graceful Restart**: Nuclear option if leak persists


##### Pattern 3: ORM Batch Operations (Doctrine Example)

**Problem:** Doctrine's EntityManager holds bidirectional entity references causing massive leaks during bulk inserts.

**Solution:**

```php
class DoctrineBulkInserter {
    private EntityManagerInterface $em;
    private int $batchSize;
    
    public function __construct(EntityManagerInterface $em, int $batchSize = 500) {
        $this->em = $em;
        $this->batchSize = $batchSize;
    }
    
    public function insertBatch(array $data): void {
        foreach ($data as $i => $item) {
            $entity = new MyEntity($item);
            $this->em->persist($entity);
            
            // Flush + clear every N entities
            if (($i + 1) % $this->batchSize === 0) {
                $this->em->flush();
                
                // CRITICAL: Clear EntityManager to break circular references
                $this->em->clear();
                
                // Force GC to clean up detached entities
                gc_collect_cycles();
                
                error_log(sprintf(
                    "Processed %d/%d entities, memory: %.2fMB",
                    $i + 1,
                    count($data),
                    memory_get_usage(true) / 1024 / 1024
                ));
            }
        }
        
        // Final flush for remaining entities
        $this->em->flush();
        $this->em->clear();
        gc_collect_cycles();
    }
}

// Without this pattern:
// - 10k inserts: 500MB memory (grows indefinitely)
// 
// With this pattern:
// - 10k inserts: 50MB stable memory
```

**Why This Works:**

- `$em->clear()` **breaks circular references** between entities and UnitOfWork
- `gc_collect_cycles()` **immediately cleans up** the now-unreachable entity graph
- Memory stays **flat instead of growing**


##### Pattern 4: Event-Driven Systems (ReactPHP/Amp)

**Problem:** Event loop runs forever; memory must be bounded.

**Solution:**

```php
use React\EventLoop\Loop;

class EventLoopMemoryManager {
    private float $lastGCTime;
    private int $gcIntervalSeconds;
    
    public function __construct(int $gcIntervalSeconds = 60) {
        $this->gcIntervalSeconds = $gcIntervalSeconds;
        $this->lastGCTime = microtime(true);
    }
    
    public function install(): void {
        // Periodic GC timer
        Loop::addPeriodicTimer($this->gcIntervalSeconds, function() {
            $this->performScheduledGC();
        });
        
        // Memory threshold check every 5 seconds
        Loop::addPeriodicTimer(5, function() {
            $this->checkMemoryThreshold();
        });
    }
    
    private function performScheduledGC(): void {
        $start = microtime(true);
        $before = memory_get_usage(true);
        $collected = gc_collect_cycles();
        $after = memory_get_usage(true);
        $duration = (microtime(true) - $start) * 1000;
        
        error_log(sprintf(
            "Scheduled GC: collected=%d, freed=%.2fMB, took=%.2fms, current=%.2fMB",
            $collected,
            ($before - $after) / 1024 / 1024,
            $duration,
            $after / 1024 / 1024
        ));
    }
    
    private function checkMemoryThreshold(): void {
        $currentMB = memory_get_usage(true) / 1024 / 1024;
        
        if ($currentMB > 200) {
            error_log("⚠️ Memory threshold exceeded: {$currentMB}MB");
            
            // Force immediate GC
            $collected = gc_collect_cycles();
            $afterMB = memory_get_usage(true) / 1024 / 1024;
            
            // If still high after GC, possible leak
            if ($afterMB > 180) {
                error_log("🚨 Memory leak suspected: {$afterMB}MB after GC");
                // Could trigger alerts, dump heap, etc.
            }
        }
    }
}

// Usage
$gcManager = new EventLoopMemoryManager(gcIntervalSeconds: 60);
$gcManager->install();

Loop::run(); // Event loop runs forever with bounded memory
```


#### 3. Disabling GC Strategically

##### When Disabling Makes Sense

**✅ Scenario 1: High-Performance API (No Cycles)**

```php
// Proven: No circular references in codebase
// Benefit: 1-3% performance gain matters at scale

gc_disable();

// Handle 50,000 requests/second
$app->run();

// GC runs once at shutdown anyway
```

**✅ Scenario 2: Short-Lived CLI Script**

```php
// Script runs < 5 seconds, simple data structures
gc_disable();

$data = fetchFromAPI();
$transformed = transformData($data);
saveToDatabase($transformed);

// Memory freed automatically at script end
```

**✅ Scenario 3: Benchmarking**

```php
// Isolate GC impact for accurate measurements
gc_disable();
$start = microtime(true);

runExpensiveOperation();

$duration = microtime(true) - $start;
gc_enable();
```

##### When Disabling is Dangerous

**❌ Scenario 1: Unknown Code Patterns**

```php
// DANGEROUS: Third-party libraries might create cycles
gc_disable();
$result = $thirdPartyLib->process($data); // Could leak!
```

**❌ Scenario 2: ORM/Entity Management**

```php
// DANGEROUS: ORMs almost always create cycles
gc_disable();
$users = $entityManager->getRepository(User::class)->findAll(); // Leaks!
```

**❌ Scenario 3: Long-Running Processes**

```php
// DANGEROUS: Memory will grow unbounded
gc_disable();

while (true) {
    $job = $queue->pop();
    $job->process(); // If cycles exist, they accumulate forever
}
```


#### 4. Measuring GC Effectiveness

##### Baseline Measurement Template

```php
class GCEffectivenessProfiler {
    private array $metrics = [];
    
    public function profileGC(callable $operation, string $label): void {
        // Baseline measurement
        $beforeMem = memory_get_usage(true);
        $beforeStats = gc_status();
        $beforeTime = hrtime(true);
        
        // Run operation
        $operation();
        
        // Force GC
        $gcStart = hrtime(true);
        $collected = gc_collect_cycles();
        $gcDuration = (hrtime(true) - $gcStart) / 1e6; // ms
        
        // Post-GC measurement
        $afterMem = memory_get_usage(true);
        $afterStats = gc_status();
        $totalDuration = (hrtime(true) - $beforeTime) / 1e6; // ms
        
        $this->metrics[$label] = [
            'memory_before_mb' => $beforeMem / 1024 / 1024,
            'memory_after_mb' => $afterMem / 1024 / 1024,
            'memory_freed_mb' => ($beforeMem - $afterMem) / 1024 / 1024,
            'cycles_collected' => $collected,
            'gc_runs_delta' => $afterStats['runs'] - $beforeStats['runs'],
            'gc_duration_ms' => $gcDuration,
            'total_duration_ms' => $totalDuration,
            'gc_overhead_pct' => ($gcDuration / $totalDuration) * 100,
        ];
    }
    
    public function report(): void {
        echo "\n=== GC Effectiveness Report ===\n";
        foreach ($this->metrics as $label => $data) {
            echo "\n{$label}:\n";
            echo "  Memory: {$data['memory_before_mb']:.2f}MB → {$data['memory_after_mb']:.2f}MB ";
            echo "(freed {$data['memory_freed_mb']:.2f}MB)\n";
            echo "  Cycles collected: {$data['cycles_collected']}\n";
            echo "  GC runs: {$data['gc_runs_delta']}\n";
            echo "  GC duration: {$data['gc_duration_ms']:.2f}ms ";
            echo "({$data['gc_overhead_pct']:.1f}% overhead)\n";
        }
    }
}

// Usage
$profiler = new GCEffectivenessProfiler();

$profiler->profileGC(function() {
    processBatch(range(1, 10000));
}, 'batch_10k');

$profiler->profileGC(function() {
    $em->flush();
    $em->clear();
}, 'doctrine_flush_clear');

$profiler->report();
```

**Example Output:**

```
=== GC Effectiveness Report ===

batch_10k:
  Memory: 45.23MB → 42.18MB (freed 3.05MB)
  Cycles collected: 1247
  GC runs: 1
  GC duration: 2.34ms (0.8% overhead)

doctrine_flush_clear:
  Memory: 128.67MB → 52.19MB (freed 76.48MB)
  Cycles collected: 8932
  GC runs: 3
  GC duration: 18.72ms (4.2% overhead)
```


#### 5. Advanced Control: Tuning GC Threshold (PHP 8.3+)

##### Dynamic Threshold Adjustment

```php
// PHP 8.3+ only
function optimizeGCForWorkload(string $workloadType): void {
    switch ($workloadType) {
        case 'high_throughput_api':
            // Fewer GC runs, accept higher memory
            gc_mem_caches(50000);
            error_log("GC threshold set to 50k (high throughput mode)");
            break;
            
        case 'memory_constrained':
            // More GC runs, lower memory ceiling
            gc_mem_caches(5000);
            error_log("GC threshold set to 5k (memory constrained mode)");
            break;
            
        case 'balanced':
        default:
            gc_mem_caches(10000);
            error_log("GC threshold set to 10k (balanced mode)");
            break;
    }
}

// Apply at application bootstrap
optimizeGCForWorkload(getenv('APP_WORKLOAD_TYPE') ?: 'balanced');
```


#### 6. Production Integration Patterns

##### Pattern 1: Monitoring Metrics

```php
class GCMetricsCollector {
    private $statsClient; // StatsD, Prometheus, etc.
    
    public function collectMetrics(): void {
        $stats = gc_status();
        
        $this->statsClient->gauge('php.gc.runs', $stats['runs']);
        $this->statsClient->gauge('php.gc.collected', $stats['collected']);
        $this->statsClient->gauge('php.gc.roots', $stats['roots']);
        $this->statsClient->gauge('php.gc.threshold', $stats['threshold']);
        $this->statsClient->gauge('php.gc.buffer_utilization', 
            ($stats['roots'] / $stats['threshold']) * 100
        );
        $this->statsClient->gauge('php.memory.usage_mb', 
            memory_get_usage(true) / 1024 / 1024
        );
    }
}

// Collect every minute in background job
Loop::addPeriodicTimer(60, fn() => $collector->collectMetrics());
```

##### Pattern 2: Structured Logging

```php
function logGCEvent(string $context, int $collected, float $duration): void {
    error_log(json_encode([
        'event' => 'gc_collection',
        'context' => $context,
        'cycles_collected' => $collected,
        'duration_ms' => round($duration, 2),
        'memory_mb' => round(memory_get_usage(true) / 1024 / 1024, 2),
        'timestamp' => time(),
        'pid' => getmypid(),
    ]));
}
```

##### Pattern 3: Health Check Endpoint

```php
// GET /health/memory
$app->get('/health/memory', function() {
    $stats = gc_status();
    $memoryMB = memory_get_usage(true) / 1024 / 1024;
    $limitMB = ini_get('memory_limit');
    
    $healthy = $memoryMB < 200 && ($stats['roots'] / $stats['threshold']) < 0.9;
    
    return json_encode([
        'status' => $healthy ? 'healthy' : 'warning',
        'memory_usage_mb' => round($memoryMB, 2),
        'memory_limit' => $limitMB,
        'gc' => [
            'runs' => $stats['runs'],
            'collected' => $stats['collected'],
            'buffer_fullness_pct' => round(($stats['roots'] / $stats['threshold']) * 100, 1),
        ],
    ]);
});
```


#### 7. Common Pitfalls and Solutions

|Pitfall|Symptom|Solution|
|---|---|---|
|**GC in tight loops**|Massive performance degradation|Move `gc_collect_cycles()` outside loops|
|**Never calling GC**|Memory grows until OOM|Implement periodic GC in long-running processes|
|**Disabling GC without proof**|Mysterious leaks in production|Profile with GC enabled first; disable only if proven safe|
|**Ignoring `unset()`**|GC can't help if references still exist|Explicitly `unset()` large structures before GC|
|**GC without monitoring**|Don't know if it's helping|Log metrics before/after GC calls|
|**Assuming GC fixes all leaks**|Non-circular leaks persist|GC only handles cycles; fix reference leaks separately|


#### 8. Decision Flowchart

```
Is this a long-running process (worker/daemon)?
│
├─ YES → Implement periodic gc_collect_cycles()
│         Frequency: Every 50-1000 jobs or 1-5 minutes
│         Monitor: Memory usage trends
│
└─ NO → Is this a web request?
         │
         ├─ YES → Do nothing (GC at shutdown is sufficient)
         │
         └─ NO → Is this a batch job?
                  │
                  ├─ YES → GC after each batch
                  │
                  └─ NO → Profile to determine need
```


#### 9. Senior-Level Takeaways

✅ **Manual GC is strategic, not routine**—use between batches, not in loops  
✅ **Always `unset()` before `gc_collect_cycles()`**—GC can't free reachable references  
✅ **Measure effectiveness**—log cycles collected and memory freed  
✅ **Combine with monitoring**—track GC runs, buffer fullness, memory trends  
✅ **Use multi-layer defense**—periodic GC + threshold GC + graceful restart  
✅ **Never disable GC in production without proof**—measure first, optimize later  
✅ **ORM operations require manual GC**—`$em->clear()` + `gc_collect_cycles()` pattern  
✅ **Event loops need bounded memory**—timer-based GC every 30-60 seconds

### 13.06. Unset() vs Scope Exit 

#### 🎯 Scope of Learning

This section reveals the **mechanical differences** between manually releasing variables with `unset()` and waiting for automatic scope cleanup, and when each strategy matters for memory management. You'll understand how PHP's symbol table works, when `unset()` actually frees memory versus just removes a reference, the performance implications of each approach, and patterns for aggressive memory reclamation in memory-sensitive contexts.

Mastering this distinction transforms you from writing "clean code that works" to writing **memory-efficient code that scales** in high-throughput and resource-constrained environments.


#### 1. The Fundamental Difference

##### How Scope Exit Works

When a function returns or a block ends, PHP **automatically decrements refcounts** for all local variables in that scope:

```php
function processData() {
    $largeArray = range(1, 1000000);  // 64MB allocated
    $result = array_sum($largeArray);  // Use the data
    
    return $result;
    // ← PHP automatically decrements $largeArray refcount here
    // ← If refcount reaches 0, memory freed immediately
}

$sum = processData();
// $largeArray is already gone—freed at function exit
```

**What happens internally:**

1. Function exits
2. PHP walks through local symbol table
3. For each variable: `refcount--`
4. If `refcount == 0` → immediate memory deallocation
5. Symbol table cleared

##### How unset() Works

`unset()` **manually removes a variable from the symbol table** before scope exit:

```php
function processData() {
    $largeArray = range(1, 1000000);  // 64MB allocated
    $result = array_sum($largeArray);  // Use the data
    
    unset($largeArray);  // ← Manually decrement refcount NOW
    // Memory freed immediately (if refcount → 0)
    
    doOtherWork();  // This runs with 64MB already freed
    
    return $result;
}
```

**What happens internally:**

1. `unset($largeArray)` called
2. Remove `$largeArray` from symbol table
3. Decrement refcount
4. If `refcount == 0` → immediate memory deallocation
5. Remaining code executes with freed memory


#### 2. When They're Identical (Most Cases)

In **90% of code**, `unset()` provides **zero benefit** because the variable isn't used again anyway:

```php
// ❌ Pointless unset()
function calculate($data) {
    $temp = expensiveTransform($data);
    $result = process($temp);
    unset($temp);  // ← WASTE: function ends immediately anyway
    return $result;
}

// ✅ Identical behavior—let scope handle it
function calculate($data) {
    $temp = expensiveTransform($data);
    $result = process($temp);
    return $result;  // $temp freed here automatically
}
```

**Performance comparison:**

```php
// Both approaches: ~0.123 seconds (identical)
```

**🧠 Key Insight:** If your function ends within 5 lines after variable use, `unset()` is **premature optimization** that clutters code for zero gain.


#### 3. When unset() Actually Matters

##### Scenario 1: Long-Running Function with Peak Memory

**Problem:** Function allocates large structure early, uses it briefly, then continues with other work.

```php
// ❌ Without unset(): Peak memory = 200MB
function processLargeDataset() {
    // Step 1: Load massive file (150MB)
    $rawData = file_get_contents('huge-file.json');
    $parsed = json_decode($rawData, true);
    
    // Step 2: Extract small result (5MB)
    $summary = extractSummary($parsed);  // Only need tiny subset
    
    // Step 3: Do other work (50MB more allocations)
    $report = generateReport($summary);
    $validated = validateReport($report);
    $formatted = formatOutput($validated);
    
    return $formatted;
    // Peak memory: 150MB + 50MB = 200MB (held entire time)
}
```

```php
// ✅ With unset(): Peak memory = 55MB
function processLargeDataset() {
    // Step 1: Load massive file (150MB)
    $rawData = file_get_contents('huge-file.json');
    $parsed = json_decode($rawData, true);
    
    // Step 2: Extract small result (5MB)
    $summary = extractSummary($parsed);
    
    unset($rawData, $parsed);  // ← FREE 150MB IMMEDIATELY
    // Memory drops from 150MB → 5MB
    
    // Step 3: Do other work (50MB more allocations)
    $report = generateReport($summary);
    $validated = validateReport($report);
    $formatted = formatOutput($validated);
    
    return $formatted;
    // Peak memory: max(150MB, 5MB + 50MB) = 150MB
    // But only briefly—average much lower
}
```

**Impact:**

- **Without `unset()`**: 200MB peak, held for entire function
- **With `unset()`**: 150MB peak (briefly), then 55MB average
- **Benefit**: 73% reduction in sustained memory


##### Scenario 2: Loop Processing Large Items

**Problem:** Loop processes many large items; each iteration holds previous item in memory.

```php
// ❌ Without unset(): Memory grows linearly
function processBatches(array $files) {
    foreach ($files as $file) {
        $data = file_get_contents($file);  // 50MB per file
        processData($data);
        // $data still in memory when next iteration starts!
    }
    // After 10 files: 500MB peak memory
}

// ✅ With unset(): Flat memory usage
function processBatches(array $files) {
    foreach ($files as $file) {
        $data = file_get_contents($file);  // 50MB per file
        processData($data);
        unset($data);  // ← Free before next iteration
    }
    // After 10 files: 50MB peak (constant)
}
```

**Memory profile comparison:**

|Iteration|Without unset()|With unset()|
|---|---|---|
|1|50MB|50MB|
|2|100MB|50MB|
|3|150MB|50MB|
|10|500MB|50MB|

**🧠 Key Insight:** In loops, `unset()` prevents **memory accumulation** by freeing each iteration's data before the next starts.


##### Scenario 3: Long-Running Worker/Daemon

**Problem:** Worker processes jobs indefinitely; must keep memory bounded.

```php
// ❌ Without unset(): Memory creeps up over hours
class Worker {
    public function run() {
        while ($job = $this->queue->pop()) {
            $context = $this->buildContext($job);  // 10MB
            $result = $this->process($job, $context);
            $this->saveResult($result);
            
            // Scope never exits—function never returns!
            // $context, $result accumulate (even if refcount=1, they're in scope)
        }
    }
}

// ✅ With unset(): Bounded memory
class Worker {
    public function run() {
        while ($job = $this->queue->pop()) {
            $context = $this->buildContext($job);
            $result = $this->process($job, $context);
            $this->saveResult($result);
            
            unset($job, $context, $result);  // ← Explicit cleanup
            
            // Memory usage flat—ready for next job
        }
    }
}
```

**Real-world impact:**

- **Without `unset()`**: 100MB → 2GB over 8 hours (eventual OOM crash)
- **With `unset()`**: 100MB stable for days


#### 4. When unset() Does NOT Help

##### Trap 1: Shared References (Copy-on-Write)

```php
$original = range(1, 1000000);  // 64MB, refcount=1
$copy = $original;               // Same 64MB, refcount=2 (shared!)

unset($original);  // refcount=2 → refcount=1
                   // ❌ Memory NOT freed ($copy still holds it)

echo memory_get_usage();  // Still shows 64MB in use
```

**🧠 Key Insight:** `unset()` only frees memory when **refcount reaches 0**. If other variables share the value, memory stays allocated.


##### Trap 2: Object Properties

```php
class DataHolder {
    public $data;
    
    public function process() {
        $this->data = range(1, 1000000);  // 64MB
        
        $result = $this->compute();
        
        unset($this->data);  // ❌ Does NOT free memory!
        // It only removes property—object still exists
        
        return $result;
    }
}

// ✅ Correct way
class DataHolder {
    public $data;
    
    public function process() {
        $this->data = range(1, 1000000);
        
        $result = $this->compute();
        
        $this->data = null;  // ← Set to null, not unset()
        // Now refcount → 0, memory freed
        
        return $result;
    }
}
```

**Rule:** For object properties, use `$this->prop = null` instead of `unset($this->prop)`.


##### Trap 3: Array Elements

```php
$data = [
    'large' => range(1, 1000000),  // 64MB
    'small' => [1, 2, 3],
];

unset($data['large']);  // ✅ This DOES free the 64MB
                        // (assuming no other references)

// But this is often clearer:
$data['large'] = null;  // Also works, more explicit
```


#### 5. Scope Exit Strategies

##### Pattern 1: Early Return for Memory Release

```php
// ❌ Holds memory unnecessarily
function analyze($file) {
    $data = file_get_contents($file);  // 100MB
    $parsed = json_decode($data, true);
    
    if (!validate($parsed)) {
        return null;  // Memory still held!
    }
    
    return process($parsed);
}

// ✅ Free memory before continuing
function analyze($file) {
    $data = file_get_contents($file);
    $parsed = json_decode($data, true);
    unset($data);  // Free raw string (50% of memory)
    
    if (!validate($parsed)) {
        return null;  // Only $parsed held (already smaller)
    }
    
    return process($parsed);
}
```


##### Pattern 2: Nested Scopes (Closure Trick)

```php
function processDataset() {
    // Create artificial scope with closure
    $summary = (function() {
        $huge = loadHugeData();  // 500MB
        $result = extract($huge);  // 5MB result
        return $result;
        // ← $huge freed here automatically (scope exit)
    })();
    
    // Continue with only $summary (5MB)
    return generateReport($summary);
}

// Without closure:
function processDataset() {
    $huge = loadHugeData();  // 500MB
    $summary = extract($huge);  // 5MB result
    unset($huge);  // Manual cleanup needed
    
    return generateReport($summary);
}
```

**Trade-off:**

- **Closure approach**: Automatic cleanup, but less readable
- **Unset approach**: Explicit, clearer intent


#### 6. Performance Considerations

##### Benchmark: unset() Overhead

```php
// Test 1: No unset()
$start = microtime(true);
for ($i = 0; $i < 100000; $i++) {
    $data = range(1, 1000);
}
$noUnset = microtime(true) - $start;

// Test 2: With unset()
$start = microtime(true);
for ($i = 0; $i < 100000; $i++) {
    $data = range(1, 1000);
    unset($data);
}
$withUnset = microtime(true) - $start;

echo "No unset: {$noUnset}s\n";
echo "With unset: {$withUnset}s\n";
echo "Overhead: " . (($withUnset - $noUnset) / $noUnset * 100) . "%\n";

// Typical output:
// No unset: 0.523s
// With unset: 0.528s
// Overhead: 0.9%
```

**🧠 Key Insight:** `unset()` has **negligible performance cost** (~1%). The memory benefit almost always outweighs the tiny CPU overhead.


#### 7. Real-World Patterns

##### Pattern 1: CSV Processing

```php
class CSVProcessor {
    public function process(string $filename): array {
        // Load entire file (200MB)
        $contents = file_get_contents($filename);
        
        // Parse into array (300MB)
        $rows = str_getcsv($contents, "\n");
        unset($contents);  // ← Free raw string (200MB)
        
        $results = [];
        foreach ($rows as $row) {
            $parsed = str_getcsv($row);
            $results[] = $this->transform($parsed);
            unset($row, $parsed);  // ← Free each row after processing
        }
        unset($rows);  // ← Free row array (300MB)
        
        return $results;  // Only return transformed data (50MB)
    }
}
```

**Memory profile:**

- Peak: 500MB (contents + rows briefly overlap)
- With strategic `unset()`: 250MB peak
- Without `unset()`: 550MB peak


##### Pattern 2: Image Processing

```php
function resizeImages(array $files): void {
    foreach ($files as $file) {
        // Load original (25MB JPEG)
        $image = imagecreatefromjpeg($file);
        
        // Create thumbnail (1MB)
        $thumb = imagescale($image, 200, 200);
        
        // Save thumbnail
        imagejpeg($thumb, "thumb_{$file}");
        
        // Free both images immediately
        imagedestroy($image);  // ← Free 25MB
        imagedestroy($thumb);  // ← Free 1MB
        unset($image, $thumb);
        
        // Next iteration starts with clean slate
    }
}
```


##### Pattern 3: Database Result Sets

```php
class ReportGenerator {
    public function generate(): string {
        // Fetch large result set (500MB)
        $results = $this->db->query("SELECT * FROM huge_table")->fetchAll();
        
        // Aggregate to summary (5MB)
        $summary = $this->aggregate($results);
        unset($results);  // ← Free 500MB immediately
        
        // Continue with summary only
        $formatted = $this->format($summary);
        unset($summary);  // ← Free 5MB
        
        return $formatted;
    }
}
```


#### 8. Debugging Memory with unset()

##### Technique 1: Memory Checkpoint Pattern

```php
function debugMemoryUsage(string $label): void {
    static $last = 0;
    $current = memory_get_usage(true);
    $delta = $current - $last;
    
    printf(
        "[%s] Memory: %.2fMB (Δ %+.2fMB)\n",
        $label,
        $current / 1024 / 1024,
        $delta / 1024 / 1024
    );
    
    $last = $current;
}

// Usage
debugMemoryUsage('start');

$data = range(1, 1000000);
debugMemoryUsage('after load');

$result = process($data);
debugMemoryUsage('after process');

unset($data);
debugMemoryUsage('after unset');

// Output:
// [start] Memory: 2.00MB (Δ +0.00MB)
// [after load] Memory: 66.00MB (Δ +64.00MB)
// [after process] Memory: 70.00MB (Δ +4.00MB)
// [after unset] Memory: 6.00MB (Δ -64.00MB)  ← Proof unset() worked!
```


##### Technique 2: Verify Reference Count

```php
// Using xdebug
$data = range(1, 1000000);
xdebug_debug_zval('data');
// data: (refcount=1, is_ref=0)

$copy = $data;
xdebug_debug_zval('data');
// data: (refcount=2, is_ref=0)  ← Shared!

unset($copy);
xdebug_debug_zval('data');
// data: (refcount=1, is_ref=0)  ← Back to 1

unset($data);
// Now fully freed (refcount=0)
```


#### 9. Common Mistakes

|Mistake|Why It Fails|Correct Approach|
|---|---|---|
|`unset($this->prop)`|Removes property, doesn't free value|`$this->prop = null`|
|`unset($data)` when shared|Refcount > 1, memory stays|Check references first|
|`unset()` immediately before return|Pointless—scope exits anyway|Just return|
|`unset()` in tight loop|1% overhead for no gain|Only if memory-constrained|
|Forgetting `unset()` in infinite loop|Memory accumulates forever|Always unset in loops|


#### 10. Decision Matrix

```
Should I use unset() here?

├─ Is this in a loop?
│  ├─ YES → ✅ Use unset() at end of each iteration
│  └─ NO → Continue...
│
├─ Does function continue for >10 lines after variable use?
│  ├─ YES → ✅ Use unset() after last use
│  └─ NO → Continue...
│
├─ Is this a long-running process (worker/daemon)?
│  ├─ YES → ✅ Explicitly unset all large structures
│  └─ NO → Continue...
│
├─ Is memory critically constrained (< 128MB limit)?
│  ├─ YES → ✅ Aggressively unset() everything large
│  └─ NO → ❌ Let scope handle it (cleaner code)
```


#### 11. Senior-Level Takeaways

✅ **Scope exit auto-frees in 90% of cases**—don't clutter code with unnecessary `unset()`  
✅ **Use `unset()` when function continues significantly after variable's last use**  
✅ **Always `unset()` large variables in loops** to prevent memory accumulation  
✅ **Long-running processes (workers, daemons) must explicitly `unset()`** large structures  
✅ **`unset()` only frees memory when refcount → 0**—check for shared references  
✅ **For object properties, use `$this->prop = null`, not `unset($this->prop)`**  
✅ **`unset()` overhead is < 1%**—memory savings almost always worth it  
✅ **Combine with `memory_get_usage()` to verify effectiveness**

Absolutely — here is a **high-quality, mentor-style expansion** for:

| **Long-Running Script Strategy** | Critical for daemons, queues, imports, real-time processes |


### 13.07. Long-Running Script Strategy 

#### 🎯 **Scope of Learning**

This section teaches how to **keep memory usage stable over long periods** in scripts that **never terminate**, such as queue workers, background daemons, schedulers, data importers, WebSocket servers, and streaming pipelines. You’ll learn **why memory naturally grows in these architectures**, how to **prevent accumulation with strategic cleanup**, and how to use `unset()`, `gc_collect_cycles()`, batching, and graceful restarting to achieve **bounded memory and predictable performance**.


##### **1. The Core Problem**

In a long-running script, scope never ends — meaning:

- Local variables **don’t automatically release memory**
    
- Reference cycles accumulate and **GC isn’t always triggered**
    
- Data structures and temporary buffers gradually increase memory
    
- Eventually, the process **hits `memory_limit` and crashes**
    

**Unlike web requests**, which reset memory every request, these scripts **must** manage memory manually.


##### **2. Strategy for Stable and Predictable Memory**

|Technique|Purpose|When to Use|
|---|---|---|
|**Manual cleanup with `unset()`**|Release large unneeded data|After each iteration or batch|
|**Periodic `gc_collect_cycles()`**|Collect leaked cyclic references|Every N jobs or seconds|
|**Batch processing**|Free memory between chunks|Imports, exports, ETL|
|**Monitoring memory usage**|Detect growing leaks|Logging or alerts|
|**Graceful restarts**|Reset worker state safely|After runtime or memory threshold|


##### **3. Practical Real-World Pattern**

```php
$counter = 0;

while ($job = $queue->pop()) {

    process($job);        // Heavy data
    unset($job);          // Free job payload

    if (++$counter % 100 === 0) {
        gc_collect_cycles();     // Clean reference cycles
        logMemoryUsage();        // Monitor growth
    }

    if (memory_get_usage(true) > 250 * 1024 * 1024) {
        exit;  // Graceful restart via Supervisor/PM2
    }
}
```


##### **4. Memory Stability Across Batches**

|Batch #|Without Strategy|With Strategy|
|---|---|---|
|0|50MB|50MB|
|100|210MB|52MB|
|1,000|1.2GB (OOM crash)|55MB|
|10,000|Crashed long ago|Still stable|


##### **5. Senior-Level Takeaways**

- **Memory hygiene is mandatory** — not optional — in persistent scripts
    
- **`unset()` + batching + periodic GC** is the backbone pattern
    
- **GC does not run frequently enough by itself**
    
- **Queued workers must be restartable** — never assume infinite runtime
    
- **Monitoring memory trends reveals leaks early**
    
- **Stable memory == stable performance & uptime**
 Absolutely — here is a **high-quality, mentor-style expansion** for:

| **Long-Running Script Strategy** | Critical for daemons, queues, imports, real-time processes |


### 13.08. Long-Running Script Strategy 

#### 🎯 **Scope of Learning**

This section teaches how to **keep memory usage stable over long periods** in scripts that **never terminate**, such as queue workers, background daemons, schedulers, data importers, WebSocket servers, and streaming pipelines. You’ll learn **why memory naturally grows in these architectures**, how to **prevent accumulation with strategic cleanup**, and how to use `unset()`, `gc_collect_cycles()`, batching, and graceful restarting to achieve **bounded memory and predictable performance**.


##### **1. The Core Problem**

In a long-running script, scope never ends — meaning:

- Local variables **don’t automatically release memory**
    
- Reference cycles accumulate and **GC isn’t always triggered**
    
- Data structures and temporary buffers gradually increase memory
    
- Eventually, the process **hits `memory_limit` and crashes**
    

**Unlike web requests**, which reset memory every request, these scripts **must** manage memory manually.


##### **2. Strategy for Stable and Predictable Memory**

|Technique|Purpose|When to Use|
|---|---|---|
|**Manual cleanup with `unset()`**|Release large unneeded data|After each iteration or batch|
|**Periodic `gc_collect_cycles()`**|Collect leaked cyclic references|Every N jobs or seconds|
|**Batch processing**|Free memory between chunks|Imports, exports, ETL|
|**Monitoring memory usage**|Detect growing leaks|Logging or alerts|
|**Graceful restarts**|Reset worker state safely|After runtime or memory threshold|


##### **3. Practical Real-World Pattern**

```php
$counter = 0;

while ($job = $queue->pop()) {

    process($job);        // Heavy data
    unset($job);          // Free job payload

    if (++$counter % 100 === 0) {
        gc_collect_cycles();     // Clean reference cycles
        logMemoryUsage();        // Monitor growth
    }

    if (memory_get_usage(true) > 250 * 1024 * 1024) {
        exit;  // Graceful restart via Supervisor/PM2
    }
}
```


##### **4. Memory Stability Across Batches**

|Batch #|Without Strategy|With Strategy|
|---|---|---|
|0|50MB|50MB|
|100|210MB|52MB|
|1,000|1.2GB (OOM crash)|55MB|
|10,000|Crashed long ago|Still stable|


##### **5. Senior-Level Takeaways**

- **Memory hygiene is mandatory** — not optional — in persistent scripts
    
- **`unset()` + batching + periodic GC** is the backbone pattern
    
- **GC does not run frequently enough by itself**
    
- **Queued workers must be restartable** — never assume infinite runtime
    
- **Monitoring memory trends reveals leaks early**
    
- **Stable memory == stable performance & uptime**
 Here is a **mentor-style, high-quality expansion** for:

| **Debugging Memory Leaks** | Must-know tools and techniques for diagnosing problems |


### 13.09. Debugging Memory Leaks 

#### 🎯 **Scope of Learning**

This section equips you with **practical tools and disciplined techniques** for identifying and eliminating memory leaks in PHP applications. You’ll learn how to **detect abnormal memory growth**, inspect **reference counts and circular references**, and use profiling tools to pinpoint exactly **where memory is being held**. Mastering leak debugging transforms reactive panic into **confident, data-driven problem solving**—critical for production workers, APIs, and high-volume processing systems.


##### **1. How to Recognize a Memory Leak**

A leak is suspected when:

- Memory usage **continues increasing over time**
    
- Worker/daemon processes **crash with `Allowed memory size exhausted`**
    
- Memory **doesn’t drop** after completing work or unsetting variables
    
- GC runs frequently but memory never returns to baseline
    

Example monitoring snippet:

```php
echo memory_get_usage(true) / 1024 / 1024 . " MB\n";
```


##### **2. Core Tools for Debugging**

|Tool / Function|What It Helps Identify|
|---|---|
|**`xdebug_debug_zval()`**|Inspect `refcount` and shared references|
|**`gc_status()`**|Track GC runs, collected cycles, root buffer fullness|
|**`gc_collect_cycles()`**|Check if cleanup reduces memory|
|**`memory_get_usage()` / `memory_get_peak_usage()`**|Track growth patterns|
|**Valgrind / Massif**|Native memory leaks inside extensions / C-level|
|**Profiling with Blackfire / Tideways**|Allocation hotspots and object lifetime|
|**Xdebug profiler (`cachegrind`)**|Trace memory allocations & call graph|


##### **3. Debugging Workflow (Battle-Tested)**

```php
debug("START");

$data = loadDataset();                    // Huge
debug("After load");

process($data);
debug("After process");

unset($data);
gc_collect_cycles();
debug("After cleanup");

function debug($label) {
    echo sprintf("%s: %.2fMB\n", $label, memory_get_usage(true)/1024/1024);
}
```

🔍 If memory **does not drop** after `unset()` + `gc_collect_cycles()`, investigate:

- Shared references (`refcount > 1`)
    
- Circular reference leaks
    
- Objects retained in containers (caches, registries, ORM, closures)
 
##### **4. Circular Reference Detection Example**

```php
class A { public $b; }
class B { public $a; }

$a = new A();
$b = new B();
$a->b = $b;
$b->a = $a;

unset($a, $b);
gc_collect_cycles();   // Must run to break the cycle

echo gc_status()['collected'];
```

If `collected > 0`, you found a cycle-based leak.


##### **5. Real-World Leak Hotspots**

|Source|Why it leaks|
|---|---|
|Bidirectional model relationships (ORM)|circular references retain entire graphs|
|Long-running queue workers|no natural scope exit|
|Closures capturing `$this`|hidden backward reference|
|Event listeners, observers|unsubscribed references retained|
|Static registries / caches|objects never released|
|Large arrays reused & mutated|partial retention due to CoW|


##### **6. Senior-Level Takeaways**

- **Memory that grows continuously is a leak until proven otherwise**
    
- **Measure before guessing** — logs tell the truth, not intuition
    
- **Check `refcount`** to understand where memory is really held
    
- **GC only cleans cycles**, not regular references
    
- **If GC frees only a little — leak is not cyclic**
    
- **If GC frees a lot — break relationships sooner**
    
- **Long-running scripts must use periodic cleanup & diagnostics**
 Here is a **high-quality mentor-style expansion** for:

| **Web vs CLI GC Behavior** | Impacts resource planning in persistent apps like Swoole / RoadRunner |


### 13.10. Web vs CLI GC Behavior 

#### 🎯 **Scope of Learning**

This section clarifies **why garbage collection behaves very differently in web request environments versus long-running CLI processes**. You’ll learn how PHP’s lifecycle impacts memory cleanup, why traditional web apps rarely face GC issues, and why **persistent runtimes (Swoole, RoadRunner, ReactPHP, long-running queues, daemons)** require deliberate GC strategy and memory hygiene.

Understanding this distinction is critical for designing **predictable, stable, and scalable** backend systems.


##### **1. Key Difference: Script Lifecycle**

|Environment|Lifecycle|Memory Cleanup|GC Relevance|
|---|---|---|---|
|**Web (FPM / Apache / Nginx + PHP-FPM)**|Short-lived per-request|Memory fully reset after each request|GC almost never needed|
|**CLI / Persistent runtime**|Runs indefinitely|No automatic cleanup|GC strategy mandatory|


##### **2. Web Requests — Cleanup Is Free & Automatic**

```php
// index.php
$data = range(1, 1_000_000);  // 64MB
echo "Done";

// After response, PHP process ends
// → All memory returned to OS automatically
// → GC usually not triggered
```

🧠 **Insight:** In traditional request/response PHP, memory is fully recycled, so leaks have little effect.


##### **3. CLI / Persistent Apps — Memory Keeps Accumulating**

```php
while (true) {
    $data = range(1, 1_000_000);  // 64MB per loop
    process($data);
    unset($data);                 // Required for cleanup

    gc_collect_cycles();          // Required to clean cycles
}
```

Without cleanup:

- Memory grows each iteration
    
- Eventually fatal `Allowed memory size exceeded`
    
- GC threshold triggers periodically, but **not reliably enough**
 
##### **4. Real-World Impact Scenarios**

|Platform|What Happens Without GC Strategy|
|---|---|
|**Swoole workers**|Memory keeps rising between requests because workers persist|
|**RoadRunner**|PHP worker stays alive → memory growth unless controlled|
|**Laravel queue workers**|Gradual memory creep → forced restart via Supervisor|
|**WebSocket / ReactPHP servers**|Must maintain stable memory forever|
|**ETL / Imports**|Each batch adds retained memory unless freed manually|


##### **5. Practical Strategy Differences**

|Concern|Web PHP|CLI / Persistent|
|---|---|---|
|`unset()` for early release|Mostly unnecessary|**Critical**|
|`gc_collect_cycles()`|Rarely used|**Periodic cleanup required**|
|Memory monitoring|Minimal|**Mandatory metrics & logging**|
|Graceful restart|Not required|**Essential safety mechanism**|
|Reference leaks|Auto-fixed via exit|**Accumulate over time**|


##### **6. Senior-Level Takeaways**

- **Web PHP auto-resets memory** — GC rarely matters
    
- **CLI workers live forever** — leaks accumulate without cleanup
    
- **Persistent runtimes require strategic GC and `unset()` usage**
    
- **Batching, cycling workers, and monitoring prevent OOM crashes**
    
- **Don’t write persistent PHP like web PHP — lifecycle completely changes**
 Here is a **high-quality mentor-style expansion** for:

| **Best Practices & Real-World Scenarios** | Converts theory into production decisions |


### 13.11. Best Practices & Real-World Scenarios 

#### 🎯 **Scope of Learning**

This section distills everything learned about PHP memory management into **concrete, actionable engineering practices**. You’ll learn how to apply zval, CoW, reference counting, GC, and cleanup strategies to **real production environments**, ensuring your applications remain **stable, predictable, and leak-free** under demanding workloads.

Master these principles and you’ll move from theory to **professional-grade system design decisions.**


##### **1. Core Best Practices for Memory-Safe PHP**

|Best Practice|Why It Matters|
|---|---|
|**Avoid unnecessary copying of large arrays & objects**|Prevent accidental CoW splits that double memory|
|**Never overuse references (`&`)**|They disable CoW and commonly create leaks|
|**Break circular references early**|Prevent GC-dependent leaks from accumulating|
|**Use `unset()` inside loops & worker cycles**|Free memory immediately when scope is persistent|
|**Batch processing instead of giant loads**|Bound peak memory usage|
|**Profile memory, don’t guess**|Metrics reveal leaks better than assumptions|
|**Use `gc_collect_cycles()` strategically**|Clean cyclic garbage at predictable points|
|**Restart long-running workers gracefully**|Final safety net for unknown leak patterns|


##### **2. Real-World Application Scenarios**

|Scenario|What Can Go Wrong|Correct Strategy|
|---|---|---|
|**CSV / JSON imports**|Memory explodes reading entire file|Process in chunks + `unset()` + periodic GC|
|**Queue workers / jobs**|Memory creeps until OOM crash|Batch cleanup + restart thresholds|
|**Doctrine / Eloquent ORM**|Persistent entity graph leaks|`$em->clear()` / detach + GC after flush|
|**WebSocket / ReactPHP servers**|Never-ending loop retains references|Timed GC + memory monitoring|
|**Swoole / RoadRunner apps**|Workers persist long after requests|Explicit cleanup after each request lifecycle|
|**Large image or PDF processing**|Resources never freed|`imagedestroy()` / release native buffers|
|**Event listeners + closures**|Captured references trap `$this`|Remove listeners or use `WeakReference`|


##### **3. Practical Pattern — Stable Queue Worker**

```php
while ($job = $queue->pop()) {

    handle($job);

    unset($job);                  // Release job data
    if (++$i % 100 === 0) {
        gc_collect_cycles();      // Cleanup cyclic garbage
        monitorMemory();          // Log growth
    }

    if (memory_get_usage(true) > 250 * 1024 * 1024) {
        exit;  // Graceful restart (Supervisor/PM2 will relaunch)
    }
}
```

**Outcome:** Memory stays flat, uptime stable, no OOM failures.


##### **4. Senior-Level Takeaways**

- **Theory only matters if it improves system stability & performance**
    
- **Design for bounded memory, not infinite runtime**
    
- **Measure before solving—assumptions waste time**
    
- **Cleanup strategy must be intentional in persistent runtimes**
    
- **Avoid hidden reference traps: closures, listeners, bidirectional objects**
    
- **Monitoring + controlled GC + graceful restart = production resilience**
 